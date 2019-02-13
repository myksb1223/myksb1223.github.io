---
layout: post
title:  "리액트 네이티브(React Native) 사용기 1편 - 설치부터 구동까지..."
date:   2019-02-14 01:24:00
author: Seungbeom Kim
categories: develop_diary
tags:	ReactNative 리액트네이티브 Expo
sitemap :
  changefreq : daily
  priority : 1.0
---

일단 ReactNative를 직접 체험하기 위해서 조금 찾아보니...

### 1. 설치
설치에서는 Expo의 설치 또는 Native 코드 사용을 위한 ReactNative의 설치 이렇게 두 가지 방식이 존재했다. 둘 다 Javascript를 사용하는 것은 같은데 Expo는 ReactNative에서 갈라져 나온 하나의 개발 플랫폼인 것 같다.

Expo는 Xcode와 AndroidStudio의 설치 없이도 자유롭게 앱을 만들 수 있도록 하는데, ReactNative에서 Expo SDK라는 것이 포함된 것이라고 생각하면 될 듯 하다.

나는 Expo로만 구현하는 것을 목표로 했기에 Native 코드 사용을 위한 설치는 하지 않았다.

일단, 설치 전에 Node의 버전이 8이상 사용해야 한다고 한다.

나는 nvm을 이용하여 버전을 관리했기에

    $ nvm install v11.9.0
    $ nvm use v11.9.0

위와 같이 node를 설치했다.

이제, `expo-cli`의 설치가 필요하다.

    $ npm install -g expo-cli

이렇게 한 후에 꼭... `watchman`이라는 것을 꼭 설치하도록!! 아마 이를 설치하지 않아서 구동이 제대로 안되었던 느낌이 든다.

    $ brew install watchman

이렇게 설치를 다 했으면 이제 프로젝트를 만들어 볼 시간이다.

    $ expo init AwesomeProject

옵션들이 나오는데 알아서 선택하고 넘어가면 만들어 진다.

    $ cd AwesomeProject
    $ npm start

이렇게 하면 실행된다.

### 2. 실행

아래 그림처럼 브라우저가 뜨는데...

<figure>
<img src="{{ site.baseurl }}/assets/develop_diary/react_native_1_1.png" title="ReactNative 1" class="post-image">
<figcaption style="text-align: center;">개발 환경 서버</figcaption>
</figure>

나는 Genymotion을 설치해 두어서 Genymotion으로 시뮬레이터를 킨 후, 왼쪽에 보이는 `Run on Android device/emulator`를 클릭했다.

아래와 같은 에러가 떴다.

    error: could not install *smartsocket* listener: Address already in use
    ...

검색해보면 adb의 버전이 맞지 않아서 그렇다고 한다.

    $ sudo find / -name adb

그러면 `expo-cli` 디렉토리 밑의 adb와 `sdk` 디렉토리 밑의 adb가 존재한다.

이 때, 두 adb 버전이 다르기 때문에 일어나는 현상이다.

나는 `expo-cli`의 adb가 1.0.32였고, `sdk`의 adb가 1.0.36이라 `sdk`의 adb를 `expo-cli`로 붙여넣었다.

단 이때, 당연히 Genymotion의 adb는 `sdk`의 adb여야 한다.

이렇게 성공하면...

아래의 그림처럼 뜬다.

<figure>
<img src="{{ site.baseurl }}/assets/develop_diary/react_native_1_2.png" title="ReactNative 2" class="post-image-double">
<figcaption style="text-align: center;">Genymotion으로 실행한 화면</figcaption>
</figure>

<p style="clear: left;">
여기까지 하는데만 2시간 걸린 것 같다... 언제 그 다음을 올릴지...
