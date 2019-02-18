---
layout: post
title:  "리액트 네이티브(React Native) 사용기 3편 - async와 await를 이용한 퍼미션 체크"
date:   2019-02-19 05:25:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 Expo Permissions 퍼미션 async await
sitemap :
  changefreq : daily
  priority : 1.0
---

permission을 체크하려던 중, `async`와 `await`키워드라는 것이 새롭게 나왔다. 나중에 위의 키워드에 자세히 공부하기로 하고, 일단 과연 어떻게 permission을 체크할 수 있을까?

일단 `CAMERA`에 대한 퍼미션만 체크해 보기로 했다.
Document를 참고했더니 아래와 같이 사용할 수 있다.

```Javascript
async permissionCheck() {
  const { Permissions } = Expo;
  const { status } = await Permissions.getAsync(Permissions.CAMERA);
  if (status !== 'granted') {
    const { status } = await Permissions.askAsync(Permissions.CAMERA);

    // status 처리하기.
  }
}
```

즉, `getAsync()`를 이용하여 일단 상태를 먼저 확인하고 `granted`가 아닌 상태면 `askAsync()`를 이용하여 CAMERA 퍼미션을 허용할지 거부할지를 묻는 창을 띄우는 것이다.

```Javascript
// ...

class CreateScreen extends React.Component {
  //...

  constructor(props) {
    super(props);
    this.state = {status: ''};
  }

  async checkPermisson() {
    const { Permissions } = Expo;
    const { status } = await Permissions.getAsync(Permissions.CAMERA);
    if (status !== 'granted') {
      const { status } = await Permissions.askAsync(Permissions.CAMERA);
      this.state.status = status;
    } else {
      this.state.status = status;
      alert('Hey!  they are good.');
    }
  }

  render() {
    // ...
    return (
      <View style={styles.container}>
        //...
        <Button
          title= "Select image"
          onPress={() => {
            if(this.state.status !== 'granted') {
              this.checkPermisson();
            } else {
              alert('good!');
            }  }}
        />
      </View>
    );
  }
}

// ...
```

위와 같이 작성할 수 있다.

`WRITE_EXTERNAL_STORAGE`에 대한 퍼미션이 작동을 안하는데 다음에 알아봐야 겠다.
