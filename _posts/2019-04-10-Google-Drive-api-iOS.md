---
layout: post
title:  "Objective-c로 구글드라이브 연동하기(Google Drive api in objective-c)"
date:   2019-04-10 11:26:00
author: Seungbeom Kim
categories: develop_diary
tags:	iOS Objective-c 구글드라이브 GoogleDrive
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에 [드롭박스](https://myksb1223.github.io/develop_diary/2019/04/06/DropBox-api-iOS.html)를 연동했었는데 구글드라이브 또한 연동하게 되어 블로그에 글을 남겨볼까 한다.

드롭박스보다 조금 까다로웠기에 조금 자세히 알아볼 봐야할 필요가 있다.

### 순서
1. 기본적으로 `Google Cloud Platform`에 가입 후, 프로젝트 생성 후, API 및 서비스 사용 설정에서 `Google Drive`를 찾은 후 사용하기를 해야한다.
2. OAuth client ID를 생성해야 하기 때문에 [디벨롭퍼 사이트](https://developers.google.com/identity/sign-in/ios/start-integrating)로 가서 시키는대로 쭈욱 따라한다.
3. Google Drive api는 현재 v3인데, [여기](https://developers.google.com/drive/api/v3/about-sdk?hl=ko)로 가면 자세한 정보를 알 수 있다.
4. `Podfile`을 아래와 같이 수정한다.

        pod 'Google/SignIn'
        pod 'GoogleAPIClientForREST/Drive', '~> 1.2.1'
5. [샘플](https://github.com/google/google-api-objectivec-client-for-rest)을 참고하면서 코딩하면 된다.

### 구조
1. 구글 드라이브의 사용권한을 가져온다.
2. 버튼을 클릭하면 구글 드라이브의 최상위에 존재하는 파일 및 폴더 목록을 가져와서 `UITableView`에 뿌린다.
3. 폴더를 클릭하면 해당 폴더 내의 파일 및 폴더 목록을 가져온다.
4. 파일을 클릭하면 해당 파일을 다운받아서 `UIDocumentInteractionController`을 이용하여 보여준다.
5. Google Docs 문서를 클릭할 경우 `application/pdf`로 export한 후에 pdf파일로 연다.

### 코딩

```Objective-c
// Singleton 구현이 되어 있다고 가정한다.
// GTLRDriveService *service는 singleton.service로 표현한다.

// TestViewController.m

@implementation EditNoteViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    //...

    singleton.service = [[GTLRDriveService alloc] init];
    // Turn on the library's shouldFetchNextPages feature to ensure that all items
    // are fetched.  This applies to queries which return an object derived from
    // GTLRCollectionObject.
    service.shouldFetchNextPages = YES;

    // Have the service object set tickets to retry temporary error conditions
    // automatically
    service.retryEnabled = YES;

    // 한번 로그인 후에는 다시 signIn할 필요가 없다.
    NSUserDefaults *userInfo = [NSUserDefaults standardUserDefaults];
    if([userInfo boolForKey:@"GoogleDrive"]) {
        [GIDSignIn sharedInstance].scopes = @[kGTLRAuthScopeDriveReadonly];
        [GIDSignIn sharedInstance].delegate = self;
        [GIDSignIn sharedInstance].uiDelegate = self;

        if([[GIDSignIn sharedInstance] hasAuthInKeychain]) {
            [[GIDSignIn sharedInstance] signInSilently];
        }
        else {
            [[GIDSignIn sharedInstance] signIn];
        }
    }
}

// 최초 로그인 시나 로그아웃 후에만 사용한다.
- (void)loginBtnPressed:(id)sender {
    [GIDSignIn sharedInstance].delegate = self;
    [GIDSignIn sharedInstance].uiDelegate = self;
    [GIDSignIn sharedInstance].scopes = @[kGTLRAuthScopeDriveReadonly];
    [[GIDSignIn sharedInstance] signIn];
}

- (void)signIn:(GIDSignIn *)signIn didSignInForUser:(GIDGoogleUser *)user
     withError:(NSError *)error {
    NSUserDefaults *userInfo = [NSUserDefaults standardUserDefaults];

    if(error) {
        singleton.service.authorizer = nil;
    }
    else {
        singleton.service.authorizer = user.authentication.fetcherAuthorizer;
        if([userInfo objectForKey:@"GoogleDrive"] == nil || ![userInfo boolForKey:@"GoogleDrive"]) {
            [userInfo setBool:YES forKey:@"GoogleDrive"];
            [userInfo synchronize];
        }
    }
}

- (void)signIn:(GIDSignIn *)signIn presentViewController:(UIViewController *)viewController
{
    [self presentViewController:viewController animated:YES completion:nil];
}

- (void)signIn:(GIDSignIn *)signIn dismissViewController:(UIViewController *)viewController {
    [viewController dismissViewControllerAnimated:YES completion:nil];
}

- (void)signOutBtnPressed:(id)sender {
    [[GIDSignIn sharedInstance] signOut];
    singleton.service.authorizer = nil;
    [userInfo setBool:NO forKey:@"GoogleDrive"];
    [userInfo synchronize];
}

@end

// ThirdPartyFileListViewController.m

@implementation ThirdPartyFileListViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    // Google Drive의 최상위는 root이다.
    if(self.searchPath == nil) {
        self.searchPath = @"root";
    }

    self.folderEntries = [[NSMutableArray alloc] init];

    self.fileLists.delegate = self;
    self.fileLists.dataSource = self;

    // ...
}

- (void)fetchFileList {
    __block GTLRServiceTicket *fileListTicket;

    GTLRDriveQuery_FilesList *query = [GTLRDriveQuery_FilesList query];

    // Because GTLRDrive_FileList is derived from GTLCollectionObject and the service
    // property shouldFetchNextPages is enabled, this may do multiple fetches to
    // retrieve all items in the file list.

    // Google APIs typically allow the fields returned to be limited by the "fields" property.
    // The Drive API uses the "fields" property differently by not sending most of the requested
    // resource's fields unless they are explicitly specified.
    query.fields = @"kind,nextPageToken,files(mimeType,id,kind,name,webViewLink,thumbnailLink,trashed)";
    query.q = [NSString stringWithFormat:@"trashed = false and '%@' IN parents", self.searchPath];

    fileListTicket = [singleton.service executeQuery:query
                                   completionHandler:^(GTLRServiceTicket *callbackTicket,
                                                       GTLRDrive_FileList *fileList,
                                                       NSError *callbackError) {
                                       // Callback
                                       fileListTicket = nil;
                                       if(callbackError != nil) {
                                           UIAlertController *alertController =
                                           [UIAlertController alertControllerWithTitle:@""
                                                                               message:callbackError.description
                                                                        preferredStyle:(UIAlertControllerStyle)UIAlertControllerStyleAlert];
                                           [alertController addAction:[UIAlertAction actionWithTitle:@"OK"
                                                                                               style:(UIAlertActionStyle)UIAlertActionStyleCancel
                                                                                             handler:nil]];
                                           [self presentViewController:alertController animated:YES completion:nil];
                                       }
                                       else {
                                           [self.folderEntries addObjectsFromArray:fileList.files];
                                           [self.fileLists reloadData];
                                       }
                                   }];

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
        cell.backgroundColor = [UIColor clearColor];
    }

    GTLRDrive_File *entry = [self.folderEntries objectAtIndex:[indexPath row]];
    cell.textLabel.text = entry.name;

    cell.accessoryView.hidden = YES;
    [((UIButton *)cell.accessoryView) setSelected:NO];
    if(self.deleteMode) {
        cell.accessoryView.hidden = NO;
        if([self.selectedNameForPath objectForKey:indexPath]) {
            [((UIButton *)cell.accessoryView) setSelected:YES];
        }
    }

    cell.textLabel.textColor = [[ThemeManager sharedManager] defaultTextColor];
    cell.detailTextLabel.textColor = [[ThemeManager sharedManager] defaultTextColor];
    return cell;
}

#pragma mark UITableView delegate method

- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
    GTLRDrive_File *entry = [self.folderEntries objectAtIndex:[indexPath row]];
    if([@"application/vnd.google-apps.folder" isEqualToString:entry.mimeType]) {
        ThirdPartyFileListViewController *thirdPartyFileListController = [[ThirdPartyFileListViewController alloc] initWithNibName:@"ThirdPartyFileListViewController" bundle:nil];
        thirdPartyFileListController.thirdPartyType = self.thirdPartyType;
        thirdPartyFileListController.searchPath = entry.identifier;
        [self.navigationController pushViewController:thirdPartyFileListController animated:YES];
    }
    else {
        UIAlertController *popupMsg = [UIAlertController alertControllerWithTitle:nil message:nil preferredStyle:UIAlertControllerStyleActionSheet];
        __block __typeof__(popupMsg) blockAlert = popupMsg;
        UIAlertAction *link = [UIAlertAction actionWithTitle:NSLocalizedString(@"연결하기", nil) style:UIAlertActionStyleDefault handler:^(UIAlertAction * action) {
            [blockAlert dismissViewControllerAnimated:YES completion:nil];

        }];

        UIAlertAction *preview = [UIAlertAction actionWithTitle:NSLocalizedString(@"미리보기", nil) style:UIAlertActionStyleDefault handler:^(UIAlertAction * action) {
            [blockAlert dismissViewControllerAnimated:YES completion:nil];
            if([@"application/vnd.google-apps.document" isEqualToString:entry.mimeType]) {
                if(![self existPreviewFile:[NSString stringWithFormat:@"%@.pdf", entry.name]]) {
                    [self downloadDocsFileWithFile:entry];
                }
            }
            else {
                if(![self existPreviewFile:entry.name]) {
                    [self downloadFileWithPath:entry.identifier name:entry.name];
                }
            }
        }];

        UIAlertAction *cancel = [UIAlertAction actionWithTitle:NSLocalizedString(@"Cancel", nil) style:UIAlertActionStyleCancel handler:^(UIAlertAction * action) {
            [blockAlert dismissViewControllerAnimated:YES completion:nil];
            //            [popupMsg dismissViewControllerAnimated:YES completion:nil];
        }];

        [popupMsg addAction:link];
        [popupMsg addAction:preview];
        [popupMsg addAction:cancel];
        [self presentViewController:popupMsg animated:YES completion:^{
        }];
    }

    [self tableView:tableView didDeselectRowAtIndexPath:indexPath];
}

- (void)tableView:(UITableView *)tableView didDeselectRowAtIndexPath:(NSIndexPath *)indexPath {
    UITableViewCell *cell = [tableView cellForRowAtIndexPath:indexPath];
    [cell setSelected:NO];
}

- (BOOL)existPreviewFile:(NSString *)name {
    NSString *filePath = [self.third_party_dir
                          stringByAppendingPathComponent:name];
    if ([[NSFileManager defaultManager] fileExistsAtPath:filePath]) {
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
        return YES;
    }

    return NO;
}

- (void)downloadDocsFileWithFile:(GTLRDrive_File *)entry {
    NSString *filePath = [self.third_party_dir
                          stringByAppendingPathComponent:[NSString stringWithFormat:@"%@.pdf", entry.name]];
    NSURL *destUrl = [NSURL fileURLWithPath:filePath];
    __block GTLRServiceTicket *fileListTicket;
    GTLRQuery *query = [GTLRDriveQuery_FilesExport queryForMediaWithFileId:entry.identifier mimeType:@"application/pdf"];

    //fileListTicket을 이용하여 중간에 취소할 수 있다.
    fileListTicket = [singleton.service executeQuery:query
                                   completionHandler:^(GTLRServiceTicket *callbackTicket,
                                                       GTLRDataObject *object,
                                                       NSError *callbackError) {
                                       [alert dismiss:YES];
                                       [alert removeFromSuperview];

                                       NSError *errorToReport = callbackError;
                                       NSError *writeError;
                                       if (callbackError == nil) {
                                           BOOL didSave = [object.data writeToURL:destUrl
                                                                          options:NSDataWritingAtomic
                                                                            error:&writeError];
                                           if (!didSave) {
                                               errorToReport = writeError;
                                           }
                                       }
                                       if (errorToReport == nil) {
                                           // Successfully saved the file.
                                           //
                                           // Since a downloadPath property was specified, the data argument is
                                           // nil, and the file data has been written to disk.
                                           //                    [self displayAlert:@"Downloaded"
                                           //                                format:@"%@", destinationURL.path];

                                           UIDocumentInteractionController* document = [UIDocumentInteractionController interactionControllerWithURL:destUrl];
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
                                           [self showAlertControllerTitle:@"Error Downloading File" message:[NSString stringWithFormat:@"%@", errorToReport]];

                                       }
                                   }];
}

- (void)downloadFileWithPath:(NSString *)path name:(NSString *)name {
    NSString *filePath = [self.third_party_dir
                          stringByAppendingPathComponent:name];

    __block GTLRServiceTicket *fileListTicket;
    GTLRQuery *query = [GTLRDriveQuery_FilesGet queryForMediaWithFileId:path];
    NSURL *destUrl = [NSURL fileURLWithPath:filePath];
    //fileListTicket을 이용하여 중간에 취소할 수 있다.
    fileListTicket = [singleton.service executeQuery:query
        completionHandler:^(GTLRServiceTicket *callbackTicket,
                            GTLRDataObject *object,
                            NSError *callbackError) {

            NSError *errorToReport = callbackError;
            NSError *writeError;
            if (callbackError == nil) {
                BOOL didSave = [object.data writeToURL:destUrl
                                               options:NSDataWritingAtomic
                                                 error:&writeError];
                if (!didSave) {
                    errorToReport = writeError;
                }
            }
            if (errorToReport == nil) {
                // Successfully saved the file.
                //
                // Since a downloadPath property was specified, the data argument is
                // nil, and the file data has been written to disk.
                UIDocumentInteractionController* document = [UIDocumentInteractionController interactionControllerWithURL:destUrl];
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
                NSLog(@"Error Downloading File : %@", errorToReport);
            }
        }];
}

- (UIViewController *) documentInteractionControllerViewControllerForPreview: (UIDocumentInteractionController *) controller {
    return self;
}

- (void)documentInteractionControllerWillBeginPreview:(UIDocumentInteractionController *)controller {
    AVURLAsset *avUrl = [AVURLAsset assetWithURL:controller.URL];
    CMTime time = [avUrl duration];
    time = CMTimeMultiplyByRatio(time, 1, 1000);
    float seconds = roundf(time.value/(float)(time.timescale/1000.0));

    CFStringRef UTI = UTTypeCreatePreferredIdentifierForTag(kUTTagClassFilenameExtension, (__bridge CFStringRef)controller.URL.pathExtension, NULL);

    if(UTTypeConformsTo(UTI, kUTTypeMovie) && seconds > 60.0f) {
        NSLog(@"영상의 미리보기는 1분을 넘을 수 없음.")
    }
}

- (void)documentInteractionController:(UIDocumentInteractionController *)controller didEndSendingToApplication:(NSString *)application {
    NSLog(@"전송 완료");
}
@end
```

복잡해 보이지만 위와 같이 구현할 수 있다.

1. `mimeType`은 [여기](https://developers.google.com/drive/api/v3/mime-types)에서 자세히 알아볼 수 있다.
2. 대부분 샘플에 존재하는 코드이며 나한테 맞게 바꾸었을 뿐이므로 자세한 것은 [샘플](https://github.com/google/google-api-objectivec-client-for-rest)을 참고하는 것이 좋다.
3. `third_party_dir`은 파일을 다운받았을 때, 저장할 저장소를 뜻한다. 앱 내부 저장소를 지우는 부분 구현도 당연히 필요하다.

다음은 Android에 드롭박스(DropBox) 및 구글 드라이브(Google Drive)를 연동한 내용을 올리려고 한다.
