---
layout: post
title:  "Objective-c로 구글 클래스룸 연동하기(Google Classroom api in objective-c)"
date:   2019-04-25 02:31:00
author: Seungbeom Kim
categories: develop_diary
tags:	iOS Objective-c 구글클래스룸 GoogleClassroom
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에 [드롭박스](https://myksb1223.github.io/develop_diary/2019/04/06/DropBox-api-iOS.html)와 [구글 드라이브](https://myksb1223.github.io/develop_diary/2019/04/10/Google-Drive-api-iOS.html)를 연동했었는데 구글 클래스룸 또한 연동하게 되어 블로그에 글을 남겨볼까 한다.

구글 드라이브와 매우 비슷하기 때문에 구글 드라이브에 관한 글을 참고하면 쉽게 만들 수 있다..

### 순서
1. 기본적으로 `Google Cloud Platform`에 가입 후, 프로젝트 생성 후, API 및 서비스 사용 설정에서 `Google Classroom`를 찾은 후 사용하기를 해야한다.
2. OAuth client ID를 생성해야 하기 때문에 [디벨롭퍼 사이트](https://developers.google.com/identity/sign-in/ios/start-integrating)로 가서 시키는대로 쭈욱 따라한다.
3. Google classroom api는 [여기](https://developers.google.com/classroom/guides/get-started)로 가면 자세한 정보를 알 수 있다.
4. `Podfile`을 아래와 같이 수정한다.

        pod 'Google/SignIn'
        pod 'GoogleAPIClientForREST/Classroom'
5. 샘플 코드가 없다.

### 구조
1. 구글 클래스룸의 사용권한을 가져온다.
2. 버튼을 클릭하면 구글 클래스룸에서 존재하는 수업 목록을 가져온 후, 관련 수업의 CourseWork를 가져온다.

### 코딩

```Objective-c
// Singleton 구현이 되어 있다고 가정한다.
// GTLRClassroomService *service는 singleton.service로 표현한다.

// TestViewController.m

@implementation TestViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    //...

    singleton.service = [[GTLRClassroomService alloc] init];
    // Turn on the library's shouldFetchNextPages feature to ensure that all items
    // are fetched.  This applies to queries which return an object derived from
    // GTLRCollectionObject.
    service.shouldFetchNextPages = YES;

    // Have the service object set tickets to retry temporary error conditions
    // automatically
    service.retryEnabled = YES;

    // 한번 로그인 후에는 다시 signIn할 필요가 없다.
    NSUserDefaults *userInfo = [NSUserDefaults standardUserDefaults];
    if([userInfo boolForKey:@"GoogleClassroom"]) {
        [GIDSignIn sharedInstance].scopes = @[kGTLRAuthScopeClassroomCourses, kGTLRAuthScopeClassroomCourseworkStudents];
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
    [GIDSignIn sharedInstance].scopes = @[kGTLRAuthScopeClassroomCourses, kGTLRAuthScopeClassroomCourseworkStudents];
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
        if([userInfo objectForKey:@"GoogleClassroom"] == nil || ![userInfo boolForKey:@"GoogleDrive"]) {
            [userInfo setBool:YES forKey:@"GoogleClassroom"];
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
    [userInfo setBool:NO forKey:@"GoogleClassroom"];
    [userInfo synchronize];
}

@end

// GetCourseListViewController.m

@implementation GetCourseListViewController

- (void)viewDidLoad {
    [super viewDidLoad];


    self.courses = [[NSMutableArray alloc] init];

    self.courseLists.delegate = self;
    self.courseLists.dataSource = self;

    // ...

    [self fetchCourseList];
}

- (void)fetchCourseList {
    GTLRClassroomQuery_CoursesList *query = [GTLRClassroomQuery_CoursesList query];

    [singleton.service executeQuery:query
     completionHandler:^(GTLRServiceTicket * _Nonnull callbackTicket,
      GTLRClassroom_ListCoursesResponse  *_Nullable listCoursesResponse,
       NSError * _Nullable callbackError) {

        for(GTLRClassroom_Course *course in listCoursesResponse.courses) {
            NSLog(@"course name : %@", course.name);
        }
        [self.courses addObjectsFromArray:listCoursesResponse.courses];
        [self.courseLists reloadData];
    }];
}

- (void)fetchCourseWorkListWithCourse:(GTLRClassroom_Course *)course {
    GTLRClassroomQuery_CoursesCourseWorkList *query = [GTLRClassroomQuery_CoursesCourseWorkList queryWithCourseId:course.identifier];

    [singleton.service executeQuery:query
     completionHandler:^(GTLRServiceTicket * _Nonnull callbackTicket,
      GTLRClassroom_ListCourseWorkResponse  *_Nullable listCourseWorkResponse,
       NSError * _Nullable callbackError) {
        for(GTLRClassroom_CourseWork *courseWork in listCourseWorkResponse.courseWork) {
            NSLog(@"courseWork identifier : %@", courseWork.identifier);
        }
    }];
}

#pragma mark UITableView datasource method

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
    return [self.courses count];
}

- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
    return 48;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"courseLists"];

    if(cell == nil) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:@"courseLists"];
        cell.backgroundColor = [UIColor clearColor];
    }

    GTLRClassroom_Course *course = [self.courses objectAtIndex:[indexPath row]];
    cell.textLabel.text = course.name;

    return cell;
}

#pragma mark UITableView delegate method

- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
    GTLRClassroom_Course *course = [self.courses objectAtIndex:[indexPath row]];

    [self fetchCourseWorkListWithCourse:course.identifier];

    [self tableView:tableView didDeselectRowAtIndexPath:indexPath];
}

- (void)tableView:(UITableView *)tableView didDeselectRowAtIndexPath:(NSIndexPath *)indexPath {
    UITableViewCell *cell = [tableView cellForRowAtIndexPath:indexPath];
    [cell setSelected:NO];
}

@end
```

복잡해 보이지만 위와 같이 구현할 수 있다.

다음은 구글 관련 api를 통합적으로 연동하는 방법을 android, iOS로 나눠서 작성해 보려고 한다.
