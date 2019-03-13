---
layout: post
title:  "리액트 네이티브(React Native) 사용기 14편 - 함수만 존재하는 js파일 만들기"
date:   2019-03-14 04:21:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 함수 Helper
sitemap :
  changefreq : daily
  priority : 1.0
---

[이전 글](https://myksb1223.github.io/reactnative/2019/03/13/React-Native-13.html)까지 구현한 부분중에 `Alert`를 호출하는 부분이 있다.

이 부분의 공통점은 아래와 같다.
1. 버튼이 3개이다.
2. 디바이스가 iOS인지 Android인지 구분한다.
3. 선택된 후 각각의 기능을 수행한다.

위와 같은 공통점을 가지고 있어 한번 묶어보기로 했다. 일단... `Javascript` 및 `React Native`에서 Helper나 Util 파일을 만드는 것에 대한 내용을 전혀 모르기 때문에 찾아 보았다.

먼저, 시도한 것은 내 프로젝트에 있는 `node_moduldes`에 구현된 부분 중에 함수형태로 실행되는 것을 찾아 보았다.

나는 `ImagePicker`에 관련된 부분을 참고하였다.

```Javascript
export async function launchImageLibraryAsync(options = {}) {
    if (!ExponentImagePicker.launchImageLibraryAsync) {
        throw new UnavailabilityError('ImagePicker', 'launchImageLibraryAsync');
    }
    return ExponentImagePicker.launchImageLibraryAsync(options);
}
```

일단 위와 같이 표현되고 있다. 별로 어려운 형태는 아니었다. 그래서 바로 아래와 같이 따라 구현했다.

```Javascript
export function showAlert(options = {}) {
  // ...
}
```

그리고는 바로 아래와 같이 사용할 수 있었다.

```Javascript
import { showAlert } from './KSBAlert';

showAlert(
  {
    // ...
  }
)
```

하지만 나는 `KSBAlert.showAlert({})`와 같은 형태로 사용하고 싶었다.

그래서 조금 더 뒤져보았지만... `ImagePicker`은 `ExponentImagePicker`를 import하고 있었다.

```Javascript
// ExponentImagePicker.js

import { NativeModules } from 'react-native';
export default NativeModules.ExponentImagePicker;
//# sourceMappingURL=ExponentImagePicker.js.map
```

 `NativeModules`라는 것을 찾으려고 했지만... 리액트네이티브(ReactNative)를 약간 순서없이 시작한 나에게는 `NativeModules`라는 파일이 보이지 않아서 결국 그냥 검색을 했다.

참고로 `NativeModules`는 AndroidStudio나 XCode에서 직접 소스를 짜야하는 것 같다. 제대로 보지 않았지만 조금 더 자세히 알고 싶다면 [여기](https://shift.infinite.red/native-modules-for-react-native-android-ac05dbda800d)를 참고하면 좋을 것 같다.

어쨌든 내가 원하는 표현을 만들기 위해서는 많은 방법(Javascript Class를 이용...)들이 있었지만 그 중에서 내가 택한 방법은 [여기](https://stackoverflow.com/a/53297804)에 있는 방법을 택했다.

```Javascript
// ImageProcess.js
import * as KSBAlert from './KSBAlert';

export {
   KSBAlert
};

export default class ImageProcess extends React.Component {

  render() {
    return (
      <View>
        <Button
          title= "Select image"
          onPress={() => {
              KSBAlert.showAlert({
                caller: this,
                menus: ['취소', 'Camera', 'Gallery'],
                type: 0
              });
            }
          }
        />
      </View>
    );
  }
}

// KSBAlert.js

import React from 'react';
import { Alert, ActionSheetIOS, Platform } from 'react-native';

export function showAlert(options = {}) {
  if(Platform.OS === "ios") {
    ActionSheetIOS.showActionSheetWithOptions({
      options: options["menus"],
      cancelButtonIndex: 0,
    },
    (buttonIndex) => {
      if (buttonIndex === 1) {
        // return 0;
        if(options["type"] == 0) {
          options["caller"].checkPermisson(0)
        }
        else {
          options["caller"].goToEdit(options["other"])
        }
      }
      else if(buttonIndex === 2) {
        // return 1;
        if(options["type"] == 0) {
          options["caller"].checkPermisson(1)
        }
        else {
          options["caller"].goToDelete(options["other"])
        }
      }
    });
  }
  else {
    Alert.alert(
      null,
      'Select menu',
      [
        {text: options["menus"][0], onPress: () => {}},
        {text: options["menus"][1], onPress: () => {
          if(options["type"] == 0) {
            options["caller"].checkPermisson(0)
          }
          else {
            options["caller"].goToEdit(options["other"])
          }
        }},
        {text: options["menus"][2], onPress: () => {
          if(options["type"] == 0) {
            options["caller"].checkPermisson(1)
          }
          else {
            options["caller"].goToDelete(options["other"])
          }
        }},
      ],
      { cancelable: true }
    )
  }
}
```

이렇게 하면 `KSBAlert.showAlert({})`와 같은 형태로 사용할 수 있다. 약간의 트릭이지만 만족해서 사용할 수 있을 것 같다.

다음은 `BottomTabbar`를 넣어보는 방법을 공부해서 글을 남겨보려고 한다.

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
