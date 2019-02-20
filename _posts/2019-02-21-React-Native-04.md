---
layout: post
title:  "리액트 네이티브(React Native) 사용기 4편 - async와 await를 이용한 퍼미션 체크와 카메라, 갤러리 사용"
date:   2019-02-21 12:50:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 Expo Permissions 퍼미션 async await
sitemap :
  changefreq : daily
  priority : 1.0
---

[이전 글](https://myksb1223.github.io/reactnative/2019/02/19/React-Native-03.html)에서 간단하게 `CAMERA` 퍼미션에 대한 글을 작성하였다.

오늘은 실제로는 카메라, 갤러리 둘 다 접근할 수 있으므로, 유동적으로 카메라와 갤러리를 선택하고, 각각의 퍼미션을 따로 체크하는 과정에 대한 부분을 코딩해 보았다.

일단, `async`와 `await`에 대한 개념의 이해가 필요한데 [여기](https://engineering.huiseoul.com/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%9E%91%EB%8F%99%ED%95%98%EB%8A%94%EA%B0%80-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84%EC%99%80-%EB%B9%84%EB%8F%99%EA%B8%B0-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%EC%9D%98-%EB%B6%80%EC%83%81-async-await%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%BD%94%EB%94%A9-%ED%8C%81-%EB%8B%A4%EC%84%AF-%EA%B0%80%EC%A7%80-df65ffb4e7e)로 가면 자세히 알 수 있다.

### 과정
1. 버튼을 만든다.
2. 버튼을 누르면, `Alert`가 뜬다. Camera, Gallery 선택 가능.
3. 각각의 퍼미션을 체크한 후, 체크 완료시, Camera와 Gallery로 이동한다.

일단, Camera와 Gellery를 둘 다 사용할 것이기 때문에 `app.json`에 가서 아래와 같이 등록해 준다.

```JSON
"android": {
  "permissions": [
    "CAMERA",
    "CAMERA_ROLL",
  ],
},
```

그리고 Permission체크를 각각 따로 다 만들 수 없기에 하나로 묶어서 체크하는 함수를 만든다.

```Javascript
//type : CAMERA => 0, CAMERA_ROLL => 1
//p_type : getAsync => 0, askAsync => 1

getPermissionStatus(type, p_type) {
  const { Permissions } = Expo;
  if(p_type == 0) {
    if(type == 0) {
      return Permissions.getAsync(Permissions.CAMERA);
    }
    else {
      return Permissions.getAsync(Permissions.CAMERA_ROLL);
    }
  }
  else {
    if(type == 0) {
      return Permissions.askAsync(Permissions.CAMERA);
    }
    else {
      return Permissions.askAsync(Permissions.CAMERA_ROLL);
    }
  }
}
```

이렇게 한 후, `Alert`에서 클릭했을 때, 퍼미션 체크 및 그 이후 카메라 화면 또는 갤러리 화면을 보여주는 함수를 코딩한다.

```Javascript
// type : CAMERA => 0, GALLERY => 1

async checkPermisson(type) {
  let { status } = await this.getPermissionStatus(type, 0);
  if (status !== 'granted') {
    status = await this.getPermissionStatus(type, 0);
  }

  if (status === 'granted') {
    if(type == 0) {
      this.setState({ status: status === 'granted' });
      this.props.navigation.navigate('Camera', { name: 'Jane' })
    }
    else {
      this.setState({ status: status === 'granted' });
      if(this.state.status) {
        this.pickImage();
      }
    }
  }
}
```

그리고는, `Alert`를 띄우는 함수, 카메라 화면, 갤러리 화면 등의 그 외 구현해야할 뷰를 구현하면 된다. 카메라 스크린에 대한 코드는 [Expo 예제 코드](https://docs.expo.io/versions/latest/sdk/camera/)를 사용하였다.

```Javascript
//...

async pickImage() {
  let result = await ImagePicker.launchImageLibraryAsync({
    allowsEditing: true,
    aspect: [4, 3],
  });

  console.log(result);

  if (!result.cancelled) {
    this.setState({ image: result.uri });
  }
}

showAlert() {
  Alert.alert(
    null,
    'Select menu',
    [
      {text: 'Camera', onPress: () => this.checkPermisson(0)},
      {text: 'Gallery', onPress: () => this.checkPermisson(1)},
    ],
    { cancelable: true }
  )
}

render() {
  let { navigate } = this.props.navigation;
  return (
    <View style={styles.container}>
        <Button
          title= "Select image"
          onPress={() => {
              this.showAlert();
            }
          }
        />
    </View>
  );
}

// ...
```

`async`와 `await`의 개념의 이해 후에 조금 코드가 깔끔하게 정리되었다. Javascript의 공부없이 무작정 따라가려고 하니까 정말 힘들다...
