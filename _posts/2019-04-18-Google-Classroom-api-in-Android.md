---
layout: post
title:  "안드로이드로 구글 클래스룸 연동하기.(Google Classroom api in Android)."
date:   2019-04-18 02:20:00
author: Seungbeom Kim
categories: develop_diary
tags:	안드로이드 Android GoogleClassroom 구글클래스룸
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp) 안드로이드 버전에 [드롭박스](https://myksb1223.github.io/develop_diary/2019/04/11/DropBox-api-in-Android.html)와 [구글 드라이브](https://myksb1223.github.io/develop_diary/2019/04/13/Google-Drive-api-in-Android.html)를 연동했던 글을 남겼는데 이번에는 구글 클래스룸를 안드로이드에 연동하는 방법을 쓰려고 한다.

### 순서
1. 기본적으로 `Google Cloud Platform`에 가입 후, 프로젝트 생성 후, API 및 서비스 사용 설정에서 `Google Classroom`를 찾은 후 사용하기를 해야한다.
2. OAuth client ID를 생성해야 하기 때문에 [디벨롭퍼 사이트](https://developers.google.com/identity/sign-in/android/start)로 가서 시키는대로 쭈욱 따라한다.
3. Google Clasrrom api는 [여기](https://developers.google.com/classroom/reference/rest/)로 가면 자세한 정보를 알 수 있다.

### 구조

1. 구글 클래스룸의 사용권한을 가져온다.
2. 버튼을 클릭하면 구글 클래스룸의 Course를 가져온다.
3. Course에 속한 CourseWork를 가져온다.

```java
// TestActivity.java

public class TestActivity extends AppCompatActivity {
  private static final int REQUEST_CODE_SIGN_IN = 9001;
  private GoogleSignInClient mGoogleSignInClient;
  private ClassroomServiceHelper mClassroomServiceHelper;

  @Override
  protected void onResume() {
      super.onResume();

      int type = 0;
      GoogleSignInAccount account = GoogleSignIn.getLastSignedInAccount(mContext);
      Set<String> scopes = new HashSet<>();
        scopes.add(ClassroomScopes.CLASSROOM_COURSES);
        scopes.add(ClassroomScopes.CLASSROOM_COURSEWORK_STUDENTS);
      if (account != null && account.getGrantedScopes().containsAll(scopes)) {
          type = 1;
      }

      updateUI(type);
  }

  private void updateUI(int type) {
      if(type == 0) {
          // 구글 클래스룸 연동 버튼 보이기.
      }
      else {
          // Classroom 인스턴스 생성 및 코스 가져오기 버튼.
          // 로그아웃 버튼 보이기.
      }
  }

  // 구글 클래스룸 연동 버튼 눌렸을 때.
  private void loginGoogleClassroomBtnPressed(View v) {
      GoogleSignInOptions signInOptions =
              new GoogleSignInOptions.Builder(GoogleSignInOptions.DEFAULT_SIGN_IN)
                      .requestEmail()
                      .requestScopes(new Scope(ClassroomScopes.CLASSROOM_COURSES))
                      .requestScopes(new Scope(ClassroomScopes.CLASSROOM_COURSEWORK_STUDENTS))
                      .build();
      mGoogleSignInClient = GoogleSignIn.getClient(this, signInOptions);

      // The result of the sign-in Intent is handled in onActivityResult.
      startActivityForResult(mGoogleSignInClient.getSignInIntent(), REQUEST_CODE_SIGN_IN);
  }

  private void logout(final int type) {
      mGoogleSignInClient.signOut().addOnCompleteListener(this, new OnCompleteListener<Void>() {
          @Override
          public void onComplete(@NonNull Task<Void> task) {
              updateUI(0);
          }
      });
  }

  @Override
   protected void onActivityResult(int requestCode, int resultCode, Intent data) {
       super.onActivityResult(requestCode, resultCode, data);
       if(requestCode == REQUEST_CODE_SIGN_IN) {
           if (resultCode == Activity.RESULT_OK && data != null) {
               handleSignInResult(data);
           }
       }
   }

   private void handleSignInResult(Intent result) {
        GoogleSignIn.getSignedInAccountFromIntent(result)
                .addOnSuccessListener(new OnSuccessListener<GoogleSignInAccount>() {
                    @Override
                    public void onSuccess(GoogleSignInAccount googleSignInAccount) {
                        Log.d(TAG, "Signed in as " + googleSignInAccount.getEmail());

                        updateUI(1);

                        // Use the authenticated account to sign in to the Drive service.
                    }
                })
                .addOnFailureListener(new OnFailureListener() {
                    @Override
                    public void onFailure(@NonNull Exception e) {
                        Log.e(TAG, "Unable to open file from picker.", e);
                    }
                });
    }

    public void getCourseAndCourseWork(View v) {
        makeClassroomHelper();
        mClassroomServiceHelper.listCourses()
                      .addOnSuccessListener(new OnSuccessListener<List<Course>>() {
                          @Override
                          public void onSuccess(List<Course> courses) {
                              if(courses.size() > 0) {
                                  Course course = courses.get(0);

                                  // Get courseworks in selected course.
                                  mClassroomServiceHelper.listCourseWorks(course.getId())
                                          .addOnSuccessListener(new OnSuccessListener<List<CourseWork>>() {
                                              @Override
                                              public void onSuccess(List<CourseWork> courseWorks) {
                                                  Log.d(TAG, "CourseWork size : " + courseWorks.size());
                                              }
                                          })
                                          .addOnFailureListener(new OnFailureListener() {
                                              @Override
                                              public void onFailure(@NonNull Exception e) {

                                              }
                                          });
                              }
                          }
                      })
                      .addOnFailureListener(new OnFailureListener() {
                          @Override
                          public void onFailure(@NonNull Exception e) {

                          }
                      });
    }

    public void makeClassroomHelper() {
        Set<String> scopes = new HashSet<>();
        scopes.add(ClassroomScopes.CLASSROOM_COURSES);
        scopes.add(ClassroomScopes.CLASSROOM_COURSEWORK_STUDENTS);
        GoogleSignInAccount googleSignInAccount = GoogleSignIn.getLastSignedInAccount(this);
        GoogleAccountCredential credential =
                GoogleAccountCredential.usingOAuth2(
                        this,  scopes);
        credential.setSelectedAccount(googleSignInAccount.getAccount());
        Classroom service = new Classroom.Builder(AndroidHttp.newCompatibleTransport(), new GsonFactory(), credential)
                .setApplicationName("GRExample")
                .build();

        mClassroomServiceHelper = new ClassroomServiceHelper(service);
    }
}

// ClassroomServiceHelper.java

public class ClassroomServiceHelper {

    // If you don't want to multiThread, use 'newSingleThreadExecutor' method.
    private final Executor mExecutor = Executors.newFixedThreadPool(5);
    private final Classroom mClassroomService;

    public ClassroomServiceHelper(Classroom ClassroomService) {
        mClassroomService = ClassroomService;
    }

    public Task<List<Course>> listCourses() {
        return Tasks.call(mExecutor, new Callable<List<Course>>() {
            @Override
            public List<Course> call() throws Exception {
                ListCoursesResponse response = mClassroomService.courses().list()
                        .setPageSize(10)
                        .execute();

                List<Course> courses = response.getCourses();

                return courses;
            }
        });
    }

    public Task<List<CourseWork>> listCourseWorks(final String courseId) {
        return Tasks.call(mExecutor, new Callable<List<CourseWork>>() {
            @Override
            public List<CourseWork> call() throws Exception {
                ListCourseWorkResponse response = mClassroomService.courses().
                        courseWork().list(courseId).execute();


                List<CourseWork> courseWorks = response.getCourseWork();

                return courseWorks;
            }
        });
    }

    // You can make other method for getting assignments so on.

    public Task<List<Course>> listCoursesForCancalable(CancellationToken token) {
        token.onCanceledRequested(new OnTokenCanceledListener() {
            @Override
            public void onCanceled() {
                // Do something after task is canceled.
            }
        });

        final TaskCompletionSource<List<Course>> tcs = new TaskCompletionSource<>(token);

        mExecutor.execute(new Runnable() {
            @Override
            public void run() {
                try {
                    ListCoursesResponse response = mClassroomService.courses().list()
                            .setPageSize(10)
                            .execute();

                    List<Course> courses = response.getCourses();

                    // If the token is canceled, task's state is 'complete'.
                    if(!tcs.getTask().isComplete()) {
                        tcs.setResult(courses);
                    }
                } catch (IOException e) {
                    tcs.setResult(null);
                    e.printStackTrace();
                }
            }
        });

        return tcs.getTask();
    }

    public Task<List<CourseWork>> listCourseWorksForCancelable(final String courseId, CancellationToken token) {
        token.onCanceledRequested(new OnTokenCanceledListener() {
            @Override
            public void onCanceled() {
                // Do something after task is canceled.
            }
        });

        final TaskCompletionSource<List<CourseWork>> tcs = new TaskCompletionSource<>(token);

        mExecutor.execute(new Runnable() {
            @Override
            public void run() {
                try {
                    ListCourseWorkResponse response = mClassroomService.courses().
                            courseWork().list(courseId).execute();


                    List<CourseWork> courseWorks = response.getCourseWork();
                    if(!tcs.getTask().isComplete()) {
                        tcs.setResult(courseWorks);
                    }
                } catch (IOException e) {
                    tcs.setResult(null);
                    e.printStackTrace();
                }
            }
        });

        return tcs.getTask();
    }
}
```

일단, 구글 드라이브 연동과 거의 비슷하기 때문에 자세한 설명은 [구글 드라이브 연동](https://myksb1223.github.io/develop_diary/2019/04/13/Google-Drive-api-in-Android.html) 글에서 확인할 수 있다.

전체 코드는 [여기](https://github.com/myksb1223/GoogleClassroom-android-example)에 올렸으니 참고하면 된다.
