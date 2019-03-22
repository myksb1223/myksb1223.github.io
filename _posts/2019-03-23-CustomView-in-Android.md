---
layout: post
title:  "안드로이드 커스텀 뷰 1편.(CustomView in Android 1st)."
date:   2019-03-23 01:02:00
author: Seungbeom Kim
categories: develop_diary
tags:	안드로이드 Android 커스텀뷰 CustomView
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에서도 그렇고 다른 앱을 만들 때에도 그렇지만 내가 원하는 뷰를 따로 만들고 싶은 경우가 있다. 즉, 커스텀 뷰(CustomView)를 만들어야 하는 상황이 있다.

보통 커스텀 뷰(CustomView)를 만드는 상황은 아래와 같다.

1. 기존에 존재하는 뷰에 기능적인 부분을 조금 더 구현할 때.
2. 기존에 존재하는 뷰에 커스텀한 속성을 넣고 싶을 때.
3. 여러 뷰를 통합해서 하나의 뷰로 가져가면서 내가 원하는 기능 및 속성을 넣고 싶을 때.
4. 여러 화면에서 같은 기능 및 속성을 가지는 뷰를 사용해야 할 때.

전부 일맥상통하는 말이지만 결국 코드를 조금 더 깔끔하게 짜고, 조금 더 구조적으로 정교하게 코딩하기 위해서인 것 같다.

그러면 안드로이드에서 간단하게 커스텀하는 방법을 적어보려고 한다.

커스텀하는 케이스에는 크게 2가지가 존재하는 것 같다.

1. ViewGroup을 상속받고 만들어 둔 레이아웃(Layout) xml을 추가하는 경우.
2. View를 상속받아서 구현하는 경우.

당연히 두가지를 전부 혼용할 수 있다.

### ViewGroup을 상속받고 레이아웃을 추가하는 경우.

일단 먼저, xml로 레이아웃 파일을 만들어야 한다. 이 때, 신경쓰면 좋은 것은 `merge` 태그를 쓰면 좋다. 레이아웃의 DOM 구조가 깊으면 깊을 수록 속도에 영향을 주기 때문이다.

```xml
<!-- custom_layout.xml -->

<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent" android:layout_height="match_parent">

    // ...

</merge>
```

위와 같이 구현하고 java코드는 아래와 같이 사용한다. 내가 만약 해당 xml파일의 레이아웃을 `ConstraintLayout`에 맞게 작성하였다면 아래와 같이 작성할 수 있다.

```Java
public class CustomView extends ConstraintLayout {

    public CustomView(Context context) {
        super(context);
    }

    public CustomView(Context context, AttributeSet attrs) {
        super(context, attrs);
        init(context);
    }

    public CustomView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(context);
    }

    public CustomView(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
        init(context);
    }

    private void init(Context context) {
        View view = View.inflate(context, R.layout.custom_layout, this);
        // ...
    }
}
```

위와 같이 `init` 메소드에서 내가 xml에서 만든 레이아웃을 추가할 수 있다.

그러면 `view.findViewById`등의 메소드를 이용하여 xml에 만든 뷰를 접근할 수 있다.

    merge를 사용한 레이아웃 내에 사용할 뷰들의 id는 이 커스텀 뷰가 추가될 Activity나 Fragment내에 존재하는 id와 겹치지 않도록 주의해야한다.

그러면 Activity와 Fragment의 레이아웃 xml파일에서는 아래와 같이 작성 가능하다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <com.example.MyApp.CustomView
        android:id="@+id/customView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true" />

    <!-- ... -->
</RelativeLayout>
```

즉, `CustomView`는 `ConstraintLayout`와 같은 타입이다.

이제 여기서 나만의 속성을 넣고 싶을 수 있다.
이 경우는 res/values/attrs.xml 파일을 생성한 후 원하는 키와 해당되는 값의 타입을 작성해주면 된다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
	<declare-styleable name="CustomView">
       <attr name="boolKey" format="boolean" />
       <attr name="intKey" format="integer" />
       <attr name="stringKey" format="string" />
       <!-- ... -->
   </declare-styleable>
</resources>
```

이렇게 작성하면 아래와 같이 변경시킬 수 있다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:custom="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <com.example.MyApp.CustomView
        android:id="@+id/customView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        custom:boolKey="true"
        custom:intKey="1"
        custom:stringKey="CustomView" />

    <!-- ... -->
</RelativeLayout>
```

이렇게 한 후, Java 파일에서도 아래와 같이 수정해 준다.

```Java
public class CustomView extends ConstraintLayout {
    private boolean boolKey;
    private int intKey;
    private String stringKey;

    // ...

    public CustomView(Context context, AttributeSet attrs) {
        super(context, attrs);

        TypedArray a = context.getTheme().obtainStyledAttributes(
                attrs,
                R.styleable.CustomView,
                0, 0
        );

        try {
            boolKey = a.getBoolean(R.styleable.CustomView_boolKey, false));
            intKey = a.getInt(R.styleable.CustomView_intKey, 0));
            stringKey = a.getString(R.styleable.CustomView_stringKey);
        } finally {
            // release the TypedArray so that it can be reused.
            a.recycle();
        }

        // ...
    }

    // ...
}
```

이렇게 하면 기본적으로 원하는 커스텀 뷰를 간단하게 구현할 수 있다.
다음 편에서는 조금 더 심화된 부분에 대해서 글을 남겨보려고 한다.
