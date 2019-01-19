---
layout: post
title:  "클래스업에서의 이미지 처리법(Working image in ClassUp)."
date:   2019-01-20 12:36:00
author: Seungbeom Kim
categories: develop_diary
tags:	안드로이드 Android 이미지 image 클래스업 ClassUp
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에서 시간표에 배경이미지를 넣을 수 있다. 배경이미지는 4가지 방식으로 지원된다.

- 색
- 클래스업(ClassUp) 자체적으로 제공한 이미지
- 갤러리에서 가져온 이미지
- 직접 찍은 이미지

배경이미지의 유연한 작용을 위해서 `Fresco` 라이브러리를 사용하였다. 이유는 GIF 지원이 크다.

먼저, 갤러리에서 이미지를 가져오는 방식을 대략적으로 보면...
(단, GIF 이미지는 5MB를 넘을 수 없다.)

##### 갤러리 호출

```Android
Intent intent = new Intent(Intent.ACTION_GET_CONTENT);
intent.setType("image/*");
startActivityForResult(intent, REQUEST_GALLERY);
```

위와 같이 구현하면 `Google Drive`, `DropBox` 등에 저장된 이미지도 가져올 수 있다.

이미지를 선택하면 당연히 아래의 `onActivityResult`로 결과를 받는다. 이 때, 받는 데이터는 `Uri`형식이다.

이렇게 `Uri` 형식의 데이터를 `Bitmap`으로 변환할 때에는 `BitmapFactory.decodeStream()`을 이용하는 것이 좋다.

##### GIF 판별 및 5MB제어

`GIF` 판별은 아래와 같이 할 수 있다.

    ASCII 코드의 시작 무조건 47="G", 49="I", 46="F" 으로 구성된다.

 `bytes[]`를 이용해 읽었을 때는 아래와 같다.

    bytes[0] == 71 && bytes[1] == 73 && bytes[2] == 70

5MB제어는 아래와 같이 하면 된다.

```Android
if(bytes.length >= 5 * 1024 * 1024) {
    // GIF image is larger than 5MB.
}
```

##### 일반 이미지가 너무 큰 경우

이미지가 너무 크면 아래와 같은 에러 메시지를 볼 수 있다.

    Bitmap too large to be uploaded into a texture (480x5400, max=4096x4096)

즉, 이미지가 디바이스에서 지원할 수 없을 정도로 너무 크면 불러올 수 없으므로, `inSampleSize`에 대응되는 값을 계산해서 넣어주어야 한다. 이 때, 디바이스에서 지원하는 `MaxTextureSize`를 구할 수 있는데 아래와 같다. (찾다가 알아낸 것인데... 링크를 못찾겠다...)

```Android
public static int getMaxTextureSize() {
    // Safe minimum default size
    final int IMAGE_MAX_BITMAP_DIMENSION = 2048;

    // Get EGL Display
    EGL10 egl = (EGL10) EGLContext.getEGL();
    EGLDisplay display = egl.eglGetDisplay(EGL10.EGL_DEFAULT_DISPLAY);

    // Initialise
    int[] version = new int[2];
    egl.eglInitialize(display, version);

    // Query total number of configurations
    int[] totalConfigurations = new int[1];
    egl.eglGetConfigs(display, null, 0, totalConfigurations);

    // Query actual list configurations
    EGLConfig[] configurationsList = new EGLConfig[totalConfigurations[0]];
    egl.eglGetConfigs(display, configurationsList, totalConfigurations[0], totalConfigurations);

    int[] textureSize = new int[1];
    int maximumTextureSize = 0;

    // Iterate through all the configurations to located the maximum texture size
    for (int i = 0; i < totalConfigurations[0]; i++) {
        // Only need to check for width since opengl textures are always squared
        egl.eglGetConfigAttrib(display, configurationsList[i], EGL10.EGL_MAX_PBUFFER_WIDTH, textureSize);

        // Keep track of the maximum texture size
        if (maximumTextureSize < textureSize[0])
            maximumTextureSize = textureSize[0];
    }

    // Release
    egl.eglTerminate(display);

    // Return largest texture size found, or default
    return Math.max(maximumTextureSize, IMAGE_MAX_BITMAP_DIMENSION);
}
```

##### 이미지 로테이션
이미지가 찍거나 저장된 이미지가 불렀을 때 이상하게 회전된 경우가 있다. 이때 조정이 필요하므로... 아래와 같이 구현할 수 있다.

```Android
public static Bitmap getRotatePhoto(Bitmap bitmap, String filePath) {
    Bitmap rotatePhoto = null;
    ExifInterface exif;
    try {
        exif = new ExifInterface(filePath);
        int exifOrientation = exif.getAttributeInt(ExifInterface.TAG_ORIENTATION, ExifInterface.ORIENTATION_NORMAL);
        int exifDegree = exifOrientationToDegrees(exifOrientation);
        rotatePhoto = rotate(bitmap, exifDegree);
    }
    catch (IOException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    }

    return rotatePhoto;
}

public static int exifOrientationToDegrees(int exifOrientation)
{
    if(exifOrientation == ExifInterface.ORIENTATION_ROTATE_90)
    {
        return 90;
    }
    else if(exifOrientation == ExifInterface.ORIENTATION_ROTATE_180)
    {
        return 180;
    }
    else if(exifOrientation == ExifInterface.ORIENTATION_ROTATE_270)
    {
        return 270;
    }
    return 0;
}

public static Bitmap rotate(Bitmap image, int degrees)
{
    if(degrees != 0 && image != null)
    {
        Matrix m = new Matrix();
        m.setRotate(degrees, (float)image.getWidth(), (float)image.getHeight());

        try
        {
            Bitmap b = Bitmap.createBitmap(image, 0, 0, image.getWidth(), image.getHeight(), m, true);

            if(image != b)
            {
                image.recycle();
                image = b;
            }

            image = b;
        }
        catch(OutOfMemoryError ex)
        {
            ex.printStackTrace();
        }
    }
    return image;
}
```

##### 종합

위의 내용을 종합하면 아래와 같이 구현할 수 있다.

```Android
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);

    if(resultCode == RESULT_CANCELED) {
        return;
    }

    if(request_code == REQUEST_GALLERY) {
        output = data.getData();
        Bitmap bitmap = downloadImage(output);

        if(bitmap == null) {

        }
        else {
            Uri f_u = Uri.parse("file://" + filePath);

            // 혹시 같은 경로에 계속 이미지를 저장할 수 있기 때문에 항상 Cache는 초기화해야한다.
            clearCache(f_u);

            DraweeController controller = Fresco.newDraweeControllerBuilder()
                    .setImageRequest(ImageRequest.fromUri(f_u))
                    .setOldController(((SimpleDraweeView)iv).getController())
                    .setAutoPlayAnimations(true)
                    .build();

            if(isGIF) {
                // filePath를 이용하여 GIF처리.
                ((SimpleDraweeView)iv).setController(controller);
            }
            else {
                // 일반 이미지를 filePath 내 앱 저장소에 저장.
                if(saveFile(bitmap)) {
                    bitmap = getRotatePhoto(bitmap, fiePath);
                    // 회전시킨 이미지를 다시 저장.
                    if(saveFile(bitmap)) {
                        ((SimpleDraweeView)iv).setController(controller);
                    }
                }
            }

            // filePath가 임시 파일일 경우에는 실제 사용할 파일 경로로 옮긴다.
            // 그리고 해당 작업이 마무리 되어 실제로 사용될 경로를 이용하기 직전까지는 filePath를 삭제하지 않아야 한다.
            // 이미지 경로가 지워지면 Fresco에서 이미지가 호출되지 않기 때문이다.
        }
    }
}

public Bitmap downloadImage() {
    int maxTextureSize = getMaxTextureSize();
    try {
        InputStream is = context.getContentResolver().openInputStream(uri);
        byte[] bytes = readBytes(is);
        if(bytes.length != 0 && bytes[0] == 71 && bytes[1] == 73 && bytes[2] == 70) {
            if(bytes.length >= 5 * 1024 * 1024) {
                isOutLimit = true;
                return null;
            }

            isGIF = true;
            saveGIF(bytes);
        }

        is.close();

        // 한번 InputStream을 이용하여 읽으면 새롭게 stream을 생성해 주어야 한다.
        InputStream is2 = context.getContentResolver().openInputStream(uri);
        BitmapFactory.Options options = new BitmapFactory.Options();
        options.inJustDecodeBounds = true;
        BitmapFactory.decodeStream(is2, null, options);
        if(maxTextureSize != 0) {
            options.inSampleSize = calculateInSampleSize(options, maxTextureSize, maxTextureSize);
        }
        options.inInputShareable = true;
        options.inDither=false;
        options.inTempStorage=new byte[32 * 1024];
        options.inPurgeable = true;
        options.inJustDecodeBounds = false;
        if(is2.markSupported()) {
            is2.reset();
        }
        else {
            InputStream is3 = context.getContentResolver().openInputStream(uri);
            return BitmapFactory.decodeStream(is3, null, options);

        }
        return BitmapFactory.decodeStream(is2, null, options);
    } catch (FileNotFoundException ex) {
        ex.printStackTrace();
        return null;
    } catch (NullPointerException e) {
        e.printStackTrace();
        return null;
    } catch (IOException ie) {
        ie.printStackTrace();
        return null;
    }
}

public byte[] readBytes(InputStream inputStream) throws IOException {
    // this dynamically extends to take the bytes you read
    ByteArrayOutputStream byteBuffer = new ByteArrayOutputStream();

    // this is storage overwritten on each iteration with bytes
    int bufferSize = 1024;
    byte[] buffer = new byte[bufferSize];

    // we need to know how may bytes were read to write them to the byteBuffer
    int len = 0;
    while ((len = inputStream.read(buffer)) != -1) {
        byteBuffer.write(buffer, 0, len);
    }

    // and then we can return your byte array.
    return byteBuffer.toByteArray();
}

public int calculateInSampleSize(
            BitmapFactory.Options options, int reqWidth, int reqHeight) {
    // Raw height and width of image
    final int height = options.outHeight;
    final int width = options.outWidth;
    int inSampleSize = 1;

    if (height > reqHeight || width > reqWidth) {

        final int halfHeight = height / 2;
        final int halfWidth = width / 2;

        // Calculate the largest inSampleSize value that is a power of 2 and keeps both
        // height and width larger than the requested height and width.
        do {
            inSampleSize *= 2;
        } while ((halfHeight / inSampleSize) >= reqHeight
                && (halfWidth / inSampleSize) >= reqWidth);
    }

    return inSampleSize;
}

public boolean saveGIF(byte[] bytes) {
    // 갤러리에서 가져온 이미지를 내 앱의 저장소에 저장.
    try {
        File file = new File(filePath);
        OutputStream outStream = new FileOutputStream(filePath);
        outStream.write(bytes);
        outStream.close();
    } catch (IOException e) {
        e.printStackTrace();
    }

    return true;
}

public boolean saveFile(Bitmap bitmap) {
    File rFile = new File(filePath);
    OutputStream rOut = null;
    try {
        rOut = new FileOutputStream(rFile);
    }
    catch (FileNotFoundException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    }

    if(bitmap == null) {
        return false;
    }

    bitmap.compress(Bitmap.CompressFormat.JPEG, 100, rOut) ;
    try {
        rOut.close();
    }
    catch (IOException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    }

    return true;
}

public static void clearCache(Uri uri) {
    Fresco.getImagePipeline().evictFromMemoryCache(uri);
    Fresco.getImagePipelineFactory().getMainFileCache().remove(new SimpleCacheKey(uri.toString()));
    Fresco.getImagePipelineFactory().getSmallImageFileCache().remove(new SimpleCacheKey(uri.toString()));
}
```

이번에는 사진 찍었을 경우를 생각해 보면...

##### 카메라 호출

```Android
Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
File temp = new File(filePath);
try {
    temp.createNewFile();
} catch (IOException e) {
    e.printStackTrace();
}

if(temp != null) {
    output = Uri.fromFile(temp);
    if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP_MR1) {
        Uri tempUri = FileProvider.getUriForFile(this,
                "com.plokia.android.fileprovider",
                temp);
        intent.putExtra(MediaStore.EXTRA_OUTPUT, tempUri);

    }
    else {
        Uri tempUri = Uri.fromFile(temp);
        intent.putExtra(MediaStore.EXTRA_OUTPUT, tempUri);
    }

    recent_code = REQUEST_CAMERA;
    startActivityForResult(intent, REQUEST_CAMERA);
}
```

##### Uri를 String path로 변경할 때

당연히 uri로 downloadImage를 이용하여 받아도 된다. 하지만 path로도 가져오는 방법으로 구현해 보았다. 아래 코드도 링크를 잊어버렸다..

```Android
public static String getPath(final Context context, final Uri uri) {
    final boolean isKitKat = Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT;

		if(context == null || uri == null) {
			return null;
		}

    // DocumentProvider
    if (isKitKat && DocumentsContract.isDocumentUri(context, uri)) {
        // ExternalStorageProvider

        if (isExternalStorageDocument(uri)) {
            final String docId = DocumentsContract.getDocumentId(uri);
            final String[] split = docId.split(":");
            final String type = split[0];

            if ("primary".equalsIgnoreCase(type)) {
                return Environment.getExternalStorageDirectory() + "/" + split[1];
            }

            // TODO handle non-primary volumes
        }
        // DownloadsProvider
        else if (isDownloadsDocument(uri)) {
            final String id = DocumentsContract.getDocumentId(uri);
            final Uri contentUri = ContentUris.withAppendedId(
                    Uri.parse("content://downloads/public_downloads"), Long.valueOf(id));

            return getDataColumn(context, contentUri, null, null);
        }
        // MediaProvider
        else if (isMediaDocument(uri)) {
            final String docId = DocumentsContract.getDocumentId(uri);
            final String[] split = docId.split(":");
            final String type = split[0];

            Uri contentUri = null;
            if ("image".equals(type)) {
                contentUri = MediaStore.Images.Media.EXTERNAL_CONTENT_URI;
            } else if ("video".equals(type)) {
                contentUri = MediaStore.Video.Media.EXTERNAL_CONTENT_URI;
            } else if ("audio".equals(type)) {
                contentUri = MediaStore.Audio.Media.EXTERNAL_CONTENT_URI;
            }

            final String selection = "_id=?";
            final String[] selectionArgs = new String[] {
                    split[1]
            };

            return getDataColumn(context, contentUri, selection, selectionArgs);
        }
        else if(isGoogleDriveUri(uri)) {
            return uri.toString();
        }
        else {
            if(getDataColumn(context, uri, null, null) == null) {
              return uri.getLastPathSegment();
            }
            else {
              return getDataColumn(context, uri, null, null);
            }
        }
    }
    // MediaStore (and general)
    else if ("content".equalsIgnoreCase(uri.getScheme())) {
        // Return the remote address

        if(getDataColumn(context, uri, null, null) == null) {
            if (isGooglePhotosUri(uri)) {
              return uri.toString();
            }

            if(isGoogleDriveUri(uri)) {
              return uri.toString();
            }

            return uri.toString();
        }
        else {
            return getDataColumn(context, uri, null, null);
        }
    }
    // File
    else if ("file".equalsIgnoreCase(uri.getScheme())) {
        return uri.getPath();
    }

    return null;
}
```

##### 종합

```Android
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);

    if(resultCode == RESULT_CANCELED) {
        return;
    }

    if(request_code == REQUEST_CAMERA) {
        filePath = getPath(this, output);
        Bitmap bitmap = getBitmapFromPath(filePath);

        //해당 이미지를 원하는 경로에 저장하고 난 후...
        // clearCache(f_u);

        DraweeController controller = Fresco.newDraweeControllerBuilder()
                .setImageRequest(ImageRequest.fromUri(f_u))
                .setOldController(((SimpleDraweeView)iv).getController())
                .setAutoPlayAnimations(true)
                .build();
        ((SimpleDraweeView)iv).setController(controller);
    }
}

public Bitmap getBitmapFromPath() {
    Bitmap bm = null;
    BitmapFactory.Options options = new BitmapFactory.Options();
    options.inInputShareable = true;
    options.inDither=false;
    options.inTempStorage=new byte[32 * 1024];
    options.inPurgeable = true;
    options.inJustDecodeBounds = false;

    File f = null;

    try {
        f = new File(filePath);
    } catch (NullPointerException e) {
        e.printStackTrace();
    }

    FileInputStream fs=null;
    try {
        fs = new FileInputStream(f);
    } catch (FileNotFoundException e) {
        //TODO do something intelligent
        e.printStackTrace();
    } catch (NullPointerException ne) {
        return null;
    } catch (RuntimeException re) {
        return null;
    }

    try {
        if(fs!=null) bm=BitmapFactory.decodeFileDescriptor(fs.getFD(), null, options);
    } catch (IOException e) {
        //TODO do something intelligent
        e.printStackTrace();
    } catch(OutOfMemoryError oe) {

    }
    finally{
        if(fs!=null) {
            try {
                fs.close();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
    }

    return bm;
}
```

대략적으로 표현한 것이라 이미지 처리에 느려지는 부분을 thread로 구현하는 부분 등을 생략했고 몇 개의 파일로 나눠서 구현한 것을 하나로 뭉친 것이라 보기 힘들 수도 있지만 충분히 이해할 수 있다고 생각한다.

정말 이미지 관련해서 고생을 많이 했는데 이제는 어느정도 파악이 된 것 같아서 블로그에 글을 남겨본다.
