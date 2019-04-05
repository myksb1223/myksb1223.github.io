---
layout: post
title:  "Objective-c로 드롭박스 연동하기(DropBox api in objective-c)"
date:   2019-04-06 02:01:00
author: Seungbeom Kim
categories: develop_diary
tags:	iOS Objective-c 드롭박스 DropBox
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에 드롭박스를 연동하던 참에 블로그에 글을 남겨볼까 한다.

1. [드롭박스의 디벨롭퍼 사이트](https://www.dropbox.com/developers)로 이동한다.
2. 가입 후, `App Console`로 이동한 후, `Create app`을 클릭하여 내 앱에 연동될 수 있도록 `App key`를 받도록 한다.

그 이후에 아래와 같이 한다.

```
platform :ios, '10.0'
use_frameworks!
project './MyProject/MyProject.xcodeproj'

target "MyProject" do
  // ...
  pod 'ObjectiveDropboxOfficial'
end
```

위와 같이 `Podfile`에 선언한 후, 아래처럼 해당 pod를 설치한다.

    $ pod install

`info.plist` 파일을 건드리면 된다.
[드롭박스 git](https://github.com/dropbox/dropbox-sdk-obj-c#configure-your-project)에 설명이 잘 나와 있다. 참고해서 작성하면 된다.

이제 실제로 코딩을 해보자.

```Objective-c

// AppDelegate.m

#import <ObjectiveDropboxOfficial/ObjectiveDropboxOfficial.h>
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
  [DBClientsManager setupWithAppKey:@"<APP_KEY>"];
  return YES;
}
// ...

- (BOOL)application:(UIApplication *)application
            openURL:(NSURL *)url
  sourceApplication:(NSString *)sourceApplication
         annotation:(id)annotation {

   DBOAuthResult *authResult = [DBClientsManager handleRedirectURL:url];
   if (authResult != nil) {
       if ([authResult isSuccess]) {
           NSLog(@"Success! User is logged into Dropbox.");

           // ...

           return YES;
       } else if ([authResult isCancel]) {
           NSLog(@"Authorization flow was manually canceled by user!");
       } else if ([authResult isError]) {
           NSLog(@"Error: %@", authResult);
       }
   }
}


// MyViewController.m

#import "MyViewController.h"
#import <ObjectiveDropboxOfficial/ObjectiveDropboxOfficial.h>

@implementation MyViewController

- (void)viewDidLoad {
  // ...
}

- (void)linkWithDropBox {
  [DBClientsManager authorizeFromController:[UIApplication sharedApplication]
                                 controller:self
                                    openURL:^(NSURL *url) {
                                        [[UIApplication sharedApplication] openURL:url];
                                    }];
}

@end
```

여기까지 하면 일단 연동은 성공이다. 이제부터는 파일을 가져오는 방법에 대한 부분이다.

```Objective-c
// MyViewController.m

- (void)goToDropBoxFileLists {
  ThirdPartyFileListViewController *thirdPartyFileListController = [[ThirdPartyViewController alloc] initWithNib:@"ThirdPartyFileListViewController" bundle:nil];

  // [self presentViewController:thirdPartyFileListController animated:YES completion:nil];
  [self.navigationController pushViewController:thirdPartyFileListController animated:YES];
}

// ThirdPartyFileListViewController.m

#import "ThirdPartyFileListViewController.h"
#import <ObjectiveDropboxOfficial/ObjectiveDropboxOfficial.h>

@implementation ThirdPartyFileListViewController

- (void)viewDidLoad {
  // ...

  self.folderEntries = [[NSMutableArray alloc] init];

  if(self.searchPath == nil) {
    self.searchPath = @"";
  }

  if([DBClientsManager authorizedClient]) {
    [[[DBClientsManager authorizedClient].usersRoutes getCurrentAccount] setResponseBlock:^(DBUSERSFullAccount * _Nullable result, DBNilObject * _Nullable routeError, DBRequestError * _Nullable networkError)
     {
         if (networkError != nil)
         {
             NSLog(@"getCurrentAccount failed: network error");
         }
         else if (routeError != nil)
         {
             NSLog(@"getCurrentAccount failed: route error");
         }
         else
         {
             // 현재 연동된 사용자 이메일을 이용할 때
             // result.email 이용.
         }
     }];
  }

  // self.searchPath 내부의 파일들 가져오기.
  [self fetchFileList];
}

- (void)fetchFileList {
  DBUserClient *client = [DBClientsManager authorizedClient];
  if(client) {
      DBTask *task = [[client.filesRoutes listFolder:self.searchPath]
                      setResponseBlock:^(DBFILESListFolderResult *result, DBFILESListFolderError *routeError, DBRequestError *error) {

                          if (result) {
                              //             NSLog(@"result entries : %@", result.entries);
                              [self.folderEntries addObjectsFromArray:result.entries];
                              [self.fileLists reloadData];
                          } else {
                              NSString *title = @"";
                              NSString *message = @"";
                              if (routeError) {
                                  // Route-specific request error
                                  title = @"Route-specific error";
                                  if ([routeError isPath]) {
                                      message = [NSString stringWithFormat:@"Invalid path: %@", routeError.path];
                                  }
                              } else {
                                  // Generic request error
                                  title = @"Generic request error";
                                  if ([error isInternalServerError]) {
                                      DBRequestInternalServerError *internalServerError = [error asInternalServerError];
                                      message = [NSString stringWithFormat:@"%@", internalServerError];
                                  } else if ([error isBadInputError]) {
                                      DBRequestBadInputError *badInputError = [error asBadInputError];
                                      message = [NSString stringWithFormat:@"%@", badInputError];
                                  } else if ([error isAuthError]) {
                                      DBRequestAuthError *authError = [error asAuthError];
                                      message = [NSString stringWithFormat:@"%@", authError];
                                  } else if ([error isRateLimitError]) {
                                      DBRequestRateLimitError *rateLimitError = [error asRateLimitError];
                                      message = [NSString stringWithFormat:@"%@", rateLimitError];
                                  } else if ([error isHttpError]) {
                                      DBRequestHttpError *genericHttpError = [error asHttpError];
                                      message = [NSString stringWithFormat:@"%@", genericHttpError];
                                  } else if ([error isClientError]) {
                                      DBRequestClientError *genericLocalError = [error asClientError];
                                      message = [NSString stringWithFormat:@"%@", genericLocalError];
                                  }
                              }

                            [self showAlertControllerTitle:title message:message];

                          }
                      }];
  }
}

#pragma mark UITableView datasource method

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
    return [self.folderEntries count];
}

- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
    return 48;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"fileLists"];

    if(cell == nil) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:@"fileLists"];
    }
    DBFILESMetadata *entry = [self.folderEntries objectAtIndex:[indexPath row]];
    NSString *itemName = entry.name;
    if([entry isKindOfClass:[DBFILESFolderMetadata class]]) {
        cell.detailTextLabel.text = @"folder";
    }
    //        else if([entry isKindOfClass:[DBFILESPhotoMetadata class]]) {
    //            cell.detailTextLabel.text = @"photo";
    //        }
    //        else if([entry isKindOfClass:[DBFILESMediaMetadata class]]) {
    //            cell.detailTextLabel.text = @"media";
    //        }
    else if([entry isKindOfClass:[DBFILESFileMetadata class]]) {
        cell.detailTextLabel.text = @"file";
    }
    else {
        cell.detailTextLabel.text = @"unknown";
    }

    cell.textLabel.text = itemName;
    return cell;
}

#pragma mark UITableView delegate method

- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
  DBFILESMetadata *entry = [self.folderEntries objectAtIndex:[indexPath row]];
  if([entry isKindOfClass:[DBFILESFolderMetadata class]]) {
      ThirdPartyFileListViewController *thirdPartyFileListController = [[ThirdPartyFileListViewController alloc] initWithNibName:@"ThirdPartyFileListViewController" bundle:nil];
      thirdPartyFileListController.searchPath = entry.pathDisplay;
      [self.navigationController pushViewController:thirdPartyFileListController animated:YES];
  }
  else {
      // 파일들이 선택되었을 때.
      [self downloadFileWithPath:entry.pathDisplay name:entry.name];
  }

  [self tableView:tableView didDeselectRowAtIndexPath:indexPath];
}

- (void)tableView:(UITableView *)tableView didDeselectRowAtIndexPath:(NSIndexPath *)indexPath {
    UITableViewCell *cell = [tableView cellForRowAtIndexPath:indexPath];
    [cell setSelected:NO];
}

- (void)downloadFileWithPath:(NSString *)path name:(NSString *)name {
    NSString *filePath = [third_party_dir
                          stringByAppendingPathComponent:name];
    DBUserClient *client = [DBClientsManager authorizedClient];

    DBTask *task = [[client.filesRoutes downloadData:path]
                    setResponseBlock:^(DBFILESFileMetadata *result, DBFILESDownloadError *routeError, DBRequestError *error, NSData *fileData) {

                        if (result) {
                            [fileData writeToFile:filePath atomically:YES];

                            UIDocumentInteractionController* document = [UIDocumentInteractionController interactionControllerWithURL:[NSURL fileURLWithPath:filePath]];
                            document.delegate = self;
                            BOOL openResult = [document presentPreviewAnimated:YES];
                            if(!openResult) {
                                if ([[NSFileManager defaultManager] fileExistsAtPath:filePath]) {
                                    NSError *error = nil;
                                    [[NSFileManager defaultManager] removeItemAtPath:filePath error:&error];
                                }

                                [self showAlertControllerTitle:@"" message:@"Can not open this extension of file."];
                            }
                        } else {
                            NSString *title = @"";
                            NSString *message = @"";
                            if (routeError) {
                                // Route-specific request error
                                title = @"Route-specific error";
                                if ([routeError isPath]) {
                                    message = [NSString stringWithFormat:@"Invalid path: %@", routeError.path];
                                } else if ([routeError isOther]) {
                                    message = [NSString stringWithFormat:@"Unknown error: %@", routeError];
                                }
                            } else {
                                // Generic request error
                                title = @"Generic request error";
                                if ([error isInternalServerError]) {
                                    DBRequestInternalServerError *internalServerError = [error asInternalServerError];
                                    message = [NSString stringWithFormat:@"%@", internalServerError];
                                } else if ([error isBadInputError]) {
                                    DBRequestBadInputError *badInputError = [error asBadInputError];
                                    message = [NSString stringWithFormat:@"%@", badInputError];
                                } else if ([error isAuthError]) {
                                    DBRequestAuthError *authError = [error asAuthError];
                                    message = [NSString stringWithFormat:@"%@", authError];
                                } else if ([error isRateLimitError]) {
                                    DBRequestRateLimitError *rateLimitError = [error asRateLimitError];
                                    message = [NSString stringWithFormat:@"%@", rateLimitError];
                                } else if ([error isHttpError]) {
                                    DBRequestHttpError *genericHttpError = [error asHttpError];
                                    message = [NSString stringWithFormat:@"%@", genericHttpError];
                                } else if ([error isClientError]) {
                                    DBRequestClientError *genericLocalError = [error asClientError];
                                    message = [NSString stringWithFormat:@"%@", genericLocalError];
                                }

                                [self showAlertControllerTitle:title message:message];
                            }
                        }
                    }];


}

- (UIViewController *) documentInteractionControllerViewControllerForPreview: (UIDocumentInteractionController *) controller {
    return self;
}

- (void)showAlertControllerTitle:(NSString *)title message:(NSString *)message {
    UIAlertController *alertController =
    [UIAlertController alertControllerWithTitle:title
                                        message:message
                                 preferredStyle:(UIAlertControllerStyle)UIAlertControllerStyleAlert];
    [alertController addAction:[UIAlertAction actionWithTitle:NSLocalizedString(@"Ok", nil)
                                                        style:(UIAlertActionStyle)UIAlertActionStyleCancel
                                                      handler:nil]];
    [self presentViewController:alertController animated:YES completion:nil];
}
@end
```

위와 같이 사용하면 된다. 파일이 선택되었을 때는, 다운을 받아서 `UIDocumentInteractionController`를 이용하여 다른 곳으로 전송해도 되고, preview로 보여줘도 된다.

`UIDocumentInteractionController`로 보여줄 때 몇 가지 알면 좋은 것들이 있는데 그 부분은 다음에 쓴다.
