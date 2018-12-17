---
layout: post
title:  "iOS의 gallery에서 가져온 image 경로 알아보기.(Gallery image path in iOS)."
date:   2018-12-18 12:51:00
author: Seungbeom Kim
categories: develop_diary
tags:	아이폰 iOS Gallery UIImagePickerController GIF
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에서 시간표의 배경화면에 움직이는 이미지, 즉 GIF 이미지도 넣을 수 있도록 수정하기로 했다.

수정을 하던 중, 몇가지 문제가 생겼는데 UIImagePickerController에서 가져온 이미지는 그냥 UIImage 형식으로 움직이는 GIF 이미지도 그냥 일반 정지된 이미지로 가져오는 것이다.

또한, `FLAnimatedImage` 라이브러리를 사용하기에 `NSData`형태의 데이터가 필요했다.

그래서 생각한 방법은 가져온 이미지의 경로를 이용하는 것이다. 경로로 NSData를 생성하는 것이다. 클래스업은 iOS 10이상부터 지원하기 때문에 `UIImagePickerControllerImageURL` 옵션을 사용할 수 없다.

그래서 `ALAssetsLibrary`나 `PHAsset`을 사용하면 된다.

```Objective-c

// iOS 11+

- (void)imagePickerController:(UIImagePickerController *)picker didFinishPickingMediaWithInfo:(NSDictionary *)info {
  NSURL *url = [info objectForKey:@"UIImagePickerControllerImageURL"];

  // Get NSData with url.
}


// ALAssetsLibrary

- (void)imagePickerController:(UIImagePickerController *)picker didFinishPickingMediaWithInfo:(NSDictionary *)info {
  NSURL *referURL = [info objectForKey:@"UIImagePickerControllerReferenceURL"];
  ALAssetsLibrary *assetLibrary=[[ALAssetsLibrary alloc] init];
  referURL resultBlock:^(ALAsset *asset)
  {
      ALAssetRepresentation *rep = [asset defaultRepresentation];
      Byte *buffer = (Byte*)malloc(rep.size);
      NSUInteger buffered = [rep getBytes:buffer fromOffset:0.0 length:rep.size error:nil];
      NSData *data = [NSData dataWithBytesNoCopy:buffer length:buffered freeWhenDone:YES];


      // Do something with NSData.


  }
               failureBlock:^(NSError *err) {
                   NSLog(@"Error: %@",[err localizedDescription]);
               }];
}

// PHAsset

- (void)imagePickerController:(UIImagePickerController *)picker didFinishPickingMediaWithInfo:(NSDictionary *)info {
  NSURL *referURL = [info objectForKey:@"UIImagePickerControllerReferenceURL"];
  PHFetchResult *result = [PHAsset fetchAssetsWithALAssetURLs:[NSArray arrayWithObjects:referURL, nil] options:nil];

  PHImageRequestOptions *options = [PHImageRequestOptions alloc] init];

  // Set options attributes.

  PHAsset *asset = result.firstObject;
  PHImageManager *manager = [PHImageManager defaultManager] requestImageDataForAsset:asset options:options resultHandler:^(NSData * _Nullable imageData, NSString * _Nullable dataUTI, UIImageOrientation orientation, NSDictionary * _Nullable info) {
    // Do something with NSData.
  }
}

```
