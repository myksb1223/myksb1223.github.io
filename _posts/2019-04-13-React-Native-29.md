---
layout: post
title:  "리액트 네이티브(React Native) 사용기 29편 - Promise(프로미스)란?"
date:   2019-04-13 02:17:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo Instagram 인스타그램 Promise 프로미스
sitemap :
  changefreq : daily
  priority : 1.0
---

리액트 네이티브(React Native), Expo로 인스타그램(Instagram) 따라 만들면서 javascript에서의 비동기 관련된 부분을 찾아보면서 `Promise`라는 개념을 만나게 되었는데 이에 대해 간략히만 써보려고 한다.

일단, `Promise`는 비동기 식으로 작동하는 부분에서 발생하는 예외상황을 위해서 사용한다. 예외상황이라면 비동기적으로 처리한 부분의 결과를 받아온 후에 다음 처리 과정으로 넘어가야 하는데 이 경우 `Promise`를 사용하지 않으면 비동기적인 처리의 결과없이 다음 처리 과정으로 이동하게 되는 상황을 말할 수 있다.

아래의 코드를 보면...

```javascript
function readSelectedUser(options = {}) {
  let data;
  db.transaction(
    tx => {
      tx.executeSql('SELECT * FROM users WHERE id = ?', [options["id"]], (_, { rows }) => {
        data = rows["_array"][0]
      });
    },
  );
  return data;
}

// ...

render() {
  return (
    <View >
      <TouchableOpacity
        onPress={() => {
          let user = this.readSelectedUser({
            id: user_id,
          });
          alert(JSON.stringify(user));
        }}>
      </TouchableOpacity>
    </View>
  );
}
```
위의 코드는 `TouchableOpacity`를 클릭하면 db에서 사용자 데이터를 읽은 후 `Alert`화면을 띄우는 것이다.

하지만 이 코드에서 결과는 `undefined`이다. 왜냐하면 db의 결과가 끝나기 전에 값이 반환되어 `user`에는 아무값도 정의되지 않았기 때문이다.

이를 해결하기 위해서는 db에서 사용자 데이터를 다 읽은 후, 내가 원하는 메소드를 실행시켜야만 한다. 당연히 Callback함수를 사용하면 해결할 수 있다.

```javascript
function readSelectedUser(options = {}) {
  db.transaction(
    tx => {
      tx.executeSql('SELECT * FROM users WHERE id = ?', [options["id"]], (_, { rows }) => {
        options["method"](rows["_array"][0])
      });
    },
  );
}

// ...

render() {
  return (
    <View >
      <TouchableOpacity
        onPress={() => {
          this.readSelectedUser({
            id: user_id,
            method: (user) => alert(JSON.stringify(user))
          });

        }}>
      </TouchableOpacity>
    </View>
  );
}
```

위와 같이 사용할 수도 있다. 하지만 `Promise`를 사용하면 조금 더 간결하고 직관적으로 짤 수 있다.

```javascript
function readSelectedUser(options = {}) {
  return new Promise(function (resolve, reject) {
    db.transaction(
      tx => {
        tx.executeSql('SELECT * FROM users WHERE id = ?', [options["id"]], (_, { rows }) => {
          resolve(rows["_array"][0])
        });
      },
    );
  });
}

// ...

render() {
  return (
    <View >
      <TouchableOpacity
        onPress={() => {
          this.readSelectedUser({
            id: user_id,
          }).then(function (user) {
              // resolve()의 결과 값이 여기로 전달됨
              alert(JSON.stringify(user))
            }.bind(this));

        }}>
      </TouchableOpacity>
    </View>
  );
}
```

위와 같이 `Promise`를 사용하여 `Promise`가 전부 수행 완료되면 `then()`이 호출된다. `catch()`를 사용하면 예외 처리도 할 수 있다.

위와 같이 막무가내로 콜백함수를 만들고 하는 방식보다는 `Promise`라는 개념을 만들고, 다 처리되면(then) 다음 내용 수행과 같은 방식이 이해도 쉽고 코드도 훨씬 간결해 진다.

다음은 인스타그램(Instagram)에서 같은 Screen을 이곳 저곳에서 부를 때 생겼던 문제와 그 문제 해결에 대한 부분을 써보려고 한다.

인스타그램(Instagram) 따라 만들기 전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
