---
layout: post
title:  "안드로이드로 드롭박스 연동하기.(DropBox api in Android)."
date:   2019-04-11 11:59:00
author: Seungbeom Kim
categories: develop_diary
tags:	안드로이드 Android DropBox 드롭박스
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp) iOS 버전에 [드롭박스](https://myksb1223.github.io/develop_diary/2019/04/06/DropBox-api-iOS.html)와 [구글드라이브](https://myksb1223.github.io/develop_diary/2019/04/10/Google-Drive-api-iOS.html)를 연동했던 글을 남겼는데 이번에는 드롭박스를 안드로이드에 연동하는 방법을 쓰려고 한다.

1. [드롭박스의 디벨롭퍼 사이트](https://www.dropbox.com/developers)로 이동한다.
2. 가입 후, `App Console`로 이동한 후, `Create app`을 클릭하여 내 앱에 연동될 수 있도록 `App key`를 받도록 한다.

그 이후에 [샘플자료](https://github.com/dropbox/dropbox-sdk-java#dropbox-for-java-tutorial)를 참고해서 드롭박스 라이브러리를 전부 설치 한 후 아래와 같이 진행하면 된다.

### 구조

1. TestActivity에서 드롭박스에서 받은 토큰이 있으면 FileList 보기 버튼과 로그아웃 버튼이 보인다. 없으면 드롭박스 연동하는 버튼이 보인다.
2. 로그아웃 버튼을 누르면 로그아웃이 되고 원래대로 UI를 되돌린다.
3. 드롭박스 파일리스트 버튼을 누르면 ThirdPartyFileListActivity로 이동한다.
4. 최상위 폴더에 존재하는 파일 및 폴더의 목록을 받아온다.
5. 파일을 클릭하면 다운로드하고, 폴더를 클릭하면 ThirdPartyFileListActivity를 다시 이동한다.
6. 다운로드 된 파일은 내 핸드폰에 존재하는 앱 중에서 고른다. 참고로 다운로드 된 파일은 무조건 나중에 지워야 한다.

```java
// TestActivity.java

public class TestActivity extends AppCompatActivity {
  @Override
  protected void onResume() {
      super.onResume();

      int type = 0;
      SharedPreferences pref = ...;
      String accessToken = pref.getString("DropBoxAccessToken", null);
      if(accessToken == null) {
          accessToken = Auth.getOAuth2Token();
          if(accessToken != null) {
              singleton.edit.putString("DropBoxAccessToken", accessToken);
              singleton.edit.commit();
              type = 1;
          }
      }
      else {
        type = 1;

      }

      updateUI(type, accessToken);
  }

  private void updateUI(final int type, String accessToken) {
      if(type == 0) {
          // 드롭박스 연동 버튼 보이기.
      }
      else {
          DropboxClientFactory.init(accessToken);
          // 파일리스트 액티비티로 이동 버튼 보이기.
          // 로그아웃 버튼 보이기.
      }
  }

  // 드롭박스 연동 버튼 눌렸을 때.
  private void loginDropBoxBtnPressed(View v) {
      Auth.startOAuth2Authentication(this, "API_KEY");
  }

  // 파일 보는 화면으로 이동하는 버튼 눌렸을 때.
  private void goFileListActivity(View v) {
      Intent intent = new Intent(TestActivity.this, ThirdPartyFileListActivity.class);
      startActivity(intent);
  }

  private void logout(final int type) {
      DropboxClientFactory.logout(new DropboxClientFactory.Callback() {
          @Override
          public void finishLogout() {
              updateUI(0, null);
          }
      });
  }
}

// ThirdPartyFileListActivity.java

public class ThirdPartyFileListActivity extends AppCompatActivity {
  // ...
  private String mPath;
  @Override
  public void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      setContentView(R.layout.thirdparty_filelist);

      mPath = getIntent().getStringExtra("path");

      if(mPath == null) {
          mPath = "";
      }

      mFilesAdapter = new FileListAdapter();
      ListView fileLists = findViewById(R.id.fileLists);
      fileLists.setAdapter(mFilesAdapter);

      fileLists.setOnItemClickListener(new AdapterView.OnItemClickListener() {
          @Override
          public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
            Metadata item = (Metadata)mFilesAdapter.getItem(position);
            // 폴더면 다음 액티비티로 넘기고 파일이면 다운받는다.
            if (item instanceof FolderMetadata) {
                Intent intent = new Intent(ThirdPartyFileListActivity.this, ThirdPartyFileListActivity.class);
                intent.putExtra("email", account);
                intent.putExtra("path", item.getPathLower());
                startActivity(intent);

            }  else if (item instanceof FileMetadata) {
                final ProgressDialog dialog = new ProgressDialog(ThirdPartyFileListActivity.this);
                dialog.setProgressStyle(ProgressDialog.STYLE_SPINNER);
                dialog.setCancelable(true);
                dialog.setOnCancelListener(new DialogInterface.OnCancelListener() {
                    @Override
                    public void onCancel(DialogInterface dialog) {
                        if(downloadTask.getStatus() == AsyncTask.Status.RUNNING) {
                            downloadTask.cancel(true);
                        }
                        dialog.dismiss();
                    }
                });
                dialog.setMessage("Downloading");
                dialog.show();
                downloadTask = new DownloadFileTask(ThirdPartyFileListActivity.this, DropboxClientFactory.getClient(), new DownloadFileTask.Callback() {
                    @Override
                    public void onDownloadComplete(File result) {
                        dialog.dismiss();

                        if (result != null) {
                            Log.d(TAG, "Success.");
                            viewFileInExternalApp(result);
                        }
                        else {
                            Log.d(TAG, "file no to download file.");
                        }
                    }

                    @Override
                    public void onError(Exception e) {
                        dialog.dismiss();

                        Log.e(TAG, "Failed to download file.", e);
                        Toast.makeText(ThirdPartyFileListActivity.this,
                                "An error has occurred",
                                Toast.LENGTH_SHORT)
                                .show();
                    }
                });
                downloadTask.execute((FileMetadata)item);
            }
          }
      });
  }

  @Override
  protected void onResume() {
      super.onResume();

      String accessToken = singleton.pref.getString("DropBoxAccessToken", null);
      initAndLoadData(accessToken);
  }

  private void initAndLoadData(String accessToken) {
      DropboxClientFactory.init(accessToken);
      loadData();
  }

  private void loadData() {

      if(account == null || account.length() == 0) {
          new GetCurrentAccountTask(DropboxClientFactory.getClient(), new GetCurrentAccountTask.Callback() {
              @Override
              public void onComplete(FullAccount result) {
                  account = result.getEmail();
                  title.setText(account);
              }

              @Override
              public void onError(Exception e) {
                  Log.e(getClass().getName(), "Failed to get account details.", e);
              }
          }).execute();
      }

      if(mFilesAdapter.mFiles == null || mFilesAdapter.mFiles.size() == 0) {

          final ProgressDialog dialog = new ProgressDialog(this);
          dialog.setProgressStyle(ProgressDialog.STYLE_SPINNER);
          dialog.setCancelable(true);
          dialog.setMessage("Loading");
          dialog.show();

          new ListFolderTask(DropboxClientFactory.getClient(), new ListFolderTask.Callback() {
              @Override
              public void onDataLoaded(ListFolderResult result) {
                  dialog.dismiss();

                  mFilesAdapter.setFiles(result.getEntries());
              }

              @Override
              public void onError(Exception e) {
                  dialog.dismiss();

                  Log.e(TAG, "Failed to list folder.", e);
                  Toast.makeText(ThirdPartyFileListActivity.this,
                          "An error has occurred",
                          Toast.LENGTH_SHORT)
                          .show();
              }
          }).execute(mPath);
      }
  }

  class FileListAdapter extends BaseAdapter {
       private List mFiles;

       public void setFiles(List files) {
           mFiles = Collections.unmodifiableList(new ArrayList<>(files));
           notifyDataSetChanged();
       }

       @Override
       public int getCount() {
           return mFiles == null ? 0 : mFiles.size();
       }

       @Override
       public Object getItem(int position) {
           return mFiles == null ? null : mFiles.get(position);
       }

       @Override
       public long getItemId(int position) {
          return ((Metadata)mFiles.get(position)).getPathLower().hashCode();
       }

       @Override
       public View getView(int position, View convertView, ViewGroup parent) {
           LayoutInflater inflater = LayoutInflater.from(parent.getContext());
           ViewHolder holder = null;
           if(convertView == null) {
               holder = new ViewHolder();

               convertView = inflater.inflate(R.layout.licence_data_row, parent, false);

               holder.mainText = (TextView)convertView.findViewById(R.id.mainText);
               holder.subText = (TextView)convertView.findViewById(R.id.subText);

               convertView.setTag(holder);
           }
           else {
               holder = (ViewHolder)convertView.getTag();
           }

           Metadata item = (Metadata)mFiles.get(position);
           String name = item.getName();
           holder.mainText.setText(name);

           return convertView;
       }
   }

   static class ViewHolder {
       TextView mainText, subText;
   }

   private void viewFileInExternalApp(File result) {
       Intent intent = new Intent(Intent.ACTION_VIEW);
       MimeTypeMap mime = MimeTypeMap.getSingleton();
       String ext = result.getName().substring(result.getName().indexOf(".") + 1);
       String type = mime.getMimeTypeFromExtension(ext);

       Uri tempUri = FileProvider.getUriForFile(this,
               "com.example.android.fileprovider",
               result);
       intent.setDataAndType(tempUri, type);

       // 외부에서 해당 URI를 접근할 수 있도록 한다.
       intent.setFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);

       // Check for a handler first to avoid a crash
       Intent chooser = Intent.createChooser(intent, "Open File");
       try {
           startActivity(chooser);
       } catch (ActivityNotFoundException e) {
           // Instruct the user to install a PDF reader here, or something
       }
   }
}

// DropboxClientFactory.java

public class DropboxClientFactory {
    private static DbxClientV2 sDbxClient;
    private static LogoutHandler mHandler;


    public static void init(String accessToken) {
        if (sDbxClient == null) {
            DbxRequestConfig requestConfig = DbxRequestConfig.newBuilder("Example")
                    .withHttpRequestor(new OkHttp3Requestor(OkHttp3Requestor.defaultOkHttpClient()))
                    .build();

            sDbxClient = new DbxClientV2(requestConfig, accessToken);
        }
    }

    public static DbxClientV2 getClient() {
        if (sDbxClient == null) {
            throw new IllegalStateException("Client not initialized.");
        }
        return sDbxClient;
    }

    public static void logout(final DropboxClientFactory.Callback callback) {
        mHandler = new LogoutHandler(callback);
        new Thread() {
            public void run() {
                try {
                    sDbxClient.auth().tokenRevoke();
                } catch (DbxException e) {
                    e.printStackTrace();
                }

                mHandler.handleMessage(null);
            }
        }.start();
    }

    static class LogoutHandler extends Handler {
        private final WeakReference<DropboxClientFactory.Callback> mCallback;
        public LogoutHandler(DropboxClientFactory.Callback callback) {
            mCallback = new WeakReference<DropboxClientFactory.Callback>(callback);
        }

        @Override
        public void handleMessage(Message msg) {
            SharedPreferences.Editor edit = ...;
            edit.remove("DropBoxAccessToken").commit();
            sDbxClient = null;
            AuthActivity.result = null;

            DropboxClientFactory.Callback callback = mCallback.get();
            callback.finishLogout();
        }
    }

    interface Callback {
        void finishLogout();
    }
}

// DownloadFileTask.java
class DownloadFileTask extends AsyncTask<FileMetadata, Void, File> {

    private final Context mContext;
    private final DbxClientV2 mDbxClient;
    private final Callback mCallback;
    private Exception mException;

    public interface Callback {
        void onDownloadComplete(File result);
        void onError(Exception e);
    }

    DownloadFileTask(Context context, DbxClientV2 dbxClient, Callback callback) {
        mContext = context;
        mDbxClient = dbxClient;
        mCallback = callback;
    }

    @Override
    protected void onPostExecute(File result) {
        super.onPostExecute(result);
        if (mException != null) {
            mCallback.onError(mException);
        } else {
            mCallback.onDownloadComplete(result);
        }
    }

    @Override
    protected File doInBackground(FileMetadata... params) {
        FileMetadata metadata = params[0];
        try {
            File path = new File(third_party_dir);
            File file = new File(path, metadata.getName());

            // Make sure the Downloads directory exists.
            if (!path.exists()) {
                if (!path.mkdirs()) {
                    mException = new RuntimeException("Unable to create directory: " + path);
                }
            } else if (!path.isDirectory()) {
                mException = new IllegalStateException("Download path is not a directory: " + path);
                return null;
            }

            // Download the file.
            try (OutputStream outputStream = new FileOutputStream(file)) {
                mDbxClient.files().download(metadata.getPathLower(), metadata.getRev())
                        .download(outputStream);
            }

            // Tell android about the file
            Intent intent = new Intent(Intent.ACTION_MEDIA_SCANNER_SCAN_FILE);
            intent.setData(Uri.fromFile(file));
            mContext.sendBroadcast(intent);

            return file;
        } catch (DbxException | IOException e) {
            mException = e;
        }

        return null;
    }
}
```

일단, 샘플과 거의 비슷하며 로그아웃 부분만 따로 만들었고, 파일 다운받은 후의 원하는 앱을 고를 수 있도록 변경하였다.

주의할 점은 외부 앱으로 연결 시, 현재 내 URI의 권한을 허가해주는 것이다.

    intent.setFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);

다음 편은 구글 드라이브 연동에 대해서 글을 쓸 예정인데... 진짜 너무 삽질을 많이해서... 꼭 남기고 싶다.
