---
layout: post
title:  "리액트 네이티브(React Native) 사용기 7편 - 컴포넌트 라이프 사이클(Component Life Cycle)."
date:   2019-02-24 00:11:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 Expo Component ComponentLifeCycle 컴포넌트 컴포넌트라이프사이클
sitemap :
  changefreq : daily
  priority : 1.0
---

`컴포넌트(Component)`도 `라이프 사이클(Life Cycle)`이 있다. 아이폰과 안드로이드는 각각 `ViewController`, `Activity`와 대응시키면 조금 편하게 이해할 수 있다.

당연히, `컴포넌트`는 꼭 화면의 개념이 아닐 수 있기에 다를 수 있지만 앱을 만들때는 기본적으로 화면을 단위로 생각하기 때문에 크게 상관이 없다고 생각한다.

### 1. constuctor

가장 먼저 실행되는 것은 constuctor이다.
이 부분은 자바의 `생성자함수`, 아이폰의 `init`이라고 생각하면 된다.

즉, 자바나 아이폰에서의 개념과 같이 constructor에서는 기본값들을 설정해주면 된다.

### 2. componentWillMount

이 함수는 컴포넌트가 추가되기 직전에 실행되는 함수이다. 안드로이드와 아이폰에서 비교할 개념은 없다.

이렇게 생각한 이유는 안드로이드의 `Activity`, 아이폰의 `ViewController`는 화면 단위이지만 컴포넌트는 화면단위가 아닐 수도 있기 때문에 위와 같은 함수가 필요하다.

즉, 컴포넌트는 DOM이나 화면에 추가되어야 개념이고, 안드로이드와 아이폰의 화면단위 자체이기 때문에 어딘가 포함될 필요없이 바로 생성되기만 하면 된다.

### 3. componentDidMount

이 함수는 컴포넌트가 DOM이나 화면에 추가된 후에 실행되는 함수이다. 즉, 모든 렌더링이 끝난 후에 실행된다.

안드로이드에서는 `onCreate`, 아이폰에서는 `viewDidLoad`라고 생각하면 이해하기 편하다.

### 4. componentWillUnmount

이 함수는 컴포넌트가 DOM이나 화면에서 빠졌을 때 실행되는 함수이다.

안드로이드에서는 `onDestroy`, 아이폰에서는 `viewDidUnLoad`라고 생각하면 이해하기 편하다. 지금은 `viewDidUnLoad`가 없어졌지만... 가장 비슷한 것은 `viewDidDisappear`이라고 생각하면 되겠다.

### 5. componentWillReceiveProps

화면이 다시 보일 때, 해야할 작업을 넣으면 된다. 파라미터로 nextProps을 받는 것으로 봐서는 기존의 `props`와 새롭게 바뀐 `nextProps`의 차이를 비교하여 관련된 처리를 하는 곳으로 생각하면 될 것 같다.

### 6. shouldComponentUpdate

이 함수는 뷰에 대한 렌더링 처리에 관한 개념이 추가된 것이다.

즉, `componentWillReceiveProps`은 값의 변화를, `shouldComponentUpdate`는 값의 변화에 대한 렌더링을 여부를 결정하는 곳이다.

### 7. componentWillUpdate

아직 이 부분은 사용해 보지 않아서 어떤 부분인지... 딱히 언제 필요할지 모르겠다.

### 8. componentDidUpdate

이 함수는 컴포넌트의 렌더링이 끝났을 때 호출된다.

지금 5, 6, 7, 8의 통합된 것이 안드로이드의 `onResume`과 아이폰의 `viewWillAppear`의 개념으로 생각하면 될 것 같다.

앱을 만들 때, 항상 `라이프 사이클(Life Cycle)`을 염두하면서 만들었기에 컴포넌트의 라이프 사이클에 대한 이해는 비교적 쉬운 것 같다.

당연히 라이프 사이클은 서로 다르지만 결국 존재하며, 비슷한 구조를 가지기에 이전에 안드로이드나 아이폰 개발을 해본 사람이면 쉽게 받아들일 수 있을 것 같다.
