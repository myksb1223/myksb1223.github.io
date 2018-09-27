---
layout: post
title:  "안드로이드에서 아랍 언어와 같이 오른쪽에서 왼쪽방향의 언어를 쓸 경우(Right to left language like Arabic in Android)."
date:   2018-09-28 01:28:00
author: Seungbeom Kim
categories: develop_diary
tags:	안드로이드 Android RTS LTS Arabic
sitemap :
  changefreq : daily
  priority : 1.0
---

[클래스업(ClassUp) 아랍언어 아이폰 지원하는 글](https://myksb1223.github.io/develop_diary/2018/09/12/Right-to-left-language-in-iOS.html)에 썼지만 아랍은 좌우가 우리와 반대이다.

일단, Android는 기본적으로 상대적 뷰인 RelativeLayout을 썼다. ConstraintLayout도 있지만 나는 초창기부터 안드로이드 개발을 해왔던 터라 RelativeLayout이 훨씬 친근감이 있다.

기본적인 방법은 아래와 같다.

1. AndroidManifest.xml에 RTL 설정 추가한다.
2. Left는 Start, Right는 End로 변경 및 라인을 추가한다.
3. 현재 언어의 상태가 LTR인지 RTL인지 파악 후 정렬과 관련된 부분을 변경한다.
4. RTL만을 위한 values 리소스 파일을 만든다.

위와 같은 방법은 Android API 17 이상부터 지원한다.

1. `android:supportsRtl="true"` 을 AndroidMenifest.xml의 `<application>`에 추가한다.
2. 아래와 같이!(xml뿐만 아니라 java 코드에서도 같다.)

        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true"
        android:layout_toRightOf="@id/test"
        android:layout_toEndOf="@id/test"
        android:gravity="left|start"

3. 아래와 같이 현재 LTR인지 RTL인지 확인한다.

        Locale locale = Locale.getDefault();
        final int directionality = Character.getDirectionality(locale.getDisplayName().charAt(0));
        return directionality == Character.DIRECTIONALITY_RIGHT_TO_LEFT ||
        directionality == Character.DIRECTIONALITY_RIGHT_TO_LEFT_ARABIC;

4. `values-ldrtl` 디렉토리 생성한다.

조금 복잡한 것 같지만 정말 적용해보면 쉽다.
아마 코드로 LTR인지 RTL인지 알아내는 경우가 적을 수록... 아마 좋은 코드이지 않을까 한다.

참고로 클래스업(ClassUp)은 API 15부터 지원하기 때문에 코드로 `setMarginStart()`와 같은 코드를 지원하지 않기 때문에 버전확인코드를 같이 넣어주었다.

추가 부분

그리고 기본적으로 `android:textDirection="firstStrong"`이기 때문에 `style.xml`에 선언해주고 시작하는 것이 TextView나 EditText의 호환에 편하다.

홈 스크린 위젯(HomeScreenWidget)이나 ListView와 같은 곳에서의 Row를 위해 따로 layout을 만들어서 호출할 경우에는 `layout-ldrtl` 디렉토리를 따로 만들어서 관련된 TextView의 gravity를 적용해주는 것이 좋다.
