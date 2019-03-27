---
layout: post
title:  "리액트 네이티브(React Native) 사용기 19편 - ListView로 인스타그램 Profile화면 구현하기"
date:   2019-03-22 03:20:00
author: Seungbeom Kim
categories: reactnative
tags:	ReactNative 리액트네이티브 expo Navigator ListView 리스트뷰 Instagram 인스타그램
sitemap :
  changefreq : daily
  priority : 1.0
---

리액트 네이티브(React Native), Expo로 [이전 글](https://myksb1223.github.io/reactnative/2019/03/20/React-Native-18.html)까지 구현한 부분에서 이제 ProfileScreen을 구현할 것이다.

일단 인스타그램의 프로필 화면의 특징을 조금 살펴보자.

1. 화면 전체가 스크롤 된다.
2. 사용자 프로필 내용에 관련된 뷰와 사용자가 작성한 컨텐츠를 보여주는 뷰로 나뉜다.
3. 중간에 3개의 버튼을 누르면 컨텐츠가 보이는 모습이 Gird, List 등으로 바뀐다.

일단 크게 이렇게 살펴볼 수 있다. 그러면 어떤 뷰를 이용하여 만들어야 할까?

1. 일단 `ScrollView`?
2. 아니면 `ScrollView`와 `ListView`의 조합?
3. `ListView`만?
4. `라이브러리(library)`를 찾아볼까?

대충 이렇게 생각해 볼 수 있다.

1번은 컨텐츠가 무수히 많아질 경우 말도 안되는 노가다... 즉, 컨텐츠 수 만큼 직접 Row를 만들어야 하기 때문에 패스.
2번은 스크롤에 관한 제어를 자식 및 부모가 서로 겹치기 때문에 좋아보이지 않아 패스.
4번은 찾고 싶지만 직접구현해 보고싶어서 패스.

그러면 이제 남은 3번의 조건... 즉 `ListView`로 구현해 볼 수 있을 것 같다. 이제 구조를 어떻게 짜면 좋을지 고민해 볼 필요가 있다.

나는 아래와 같이 생각해보았다.

### 시나리오
1. 사용자 내용과 3개의 버튼 뷰를 하나의 Row로 생각한다.
2. 첫 번째 그리드(Grid)모양이 선택된 경우에는 한 줄을 하나의 Row로 생각한다.
3. 두 번째 리스트(List)모양이 선택된 경우에는 컨텐츠 각각을 하나의 Row로 생각한다.

위와 같이 생각하고 구현을 해보도록 하자.

### 구현

1. 화면이 호출되면 데이터베이스에서 현재 사용자(Current값이 1)가 작성한 컨텐츠를 모두 읽는다.
2. 최초가 그리드(Grid)모드, 3개씩 표현하기 때문에, 3개의 컨텐츠가 하나의 데이터가 되면 된다.(다차원 배열)
3. 사용자 내용에 관련된 뷰가 맨 처음 Row이므로 읽은 컨텐츠의 맨 앞에 빈 값을 넣어준다.(`unshift`)
4. 리스트 형태로도 변경 가능해야하기에 읽은 데이터를 그대로 저장할 필요도 있다. 당연히 이 데이터의 맨 앞에서 빈 값을 넣는다.
5. 맨 처음에는 읽을 데이터가 없기 때문에 빈 값으로 시작할 수 있도록 한다.

```Javascript
constructor() {
  super();

  this.state = {
    dataSource: ds.cloneWithRows([""]),
    datas: null,
    list_datas: null,
    ready: false,
    type: 0,
  };
}

read() {
  let map = new Array();
  db.transaction(
    tx => {
      tx.executeSql('SELECT * FROM contents WHERE user_id = ?', [global.currentUser["id"]], (_, { rows: { _array } }) => {
        let j = -1;
        for(var i in _array) {
          if(i % 3 === 0) {
            let subMap = new Array();
            j = j + 1;
            map[j] = subMap;
          }
          map[j][i%3] = _array[i];
        }

        map.unshift("")
        _array.unshift("")
        this.setState({ dataSource: this.state.dataSource.cloneWithRows(map), datas: map, list_datas: _array, ready: true})
      });
    },
  );
}
```

state의 `datas`는 그리드(Grid)를 위한 데이터 자료구조이고 `list_datas`는 리스트(List)를 위한 데이터 자료구조이다.

이렇게 데이터를 잘 읽었으니 `ListView`에 표현을 하면 된다. 표현하는 방법은 다음 글에서 작성하려고 한다.

아마 예전에 iOS, Android의 개발 경험이 있는 개발자라면 충분히 구현할 수 있다고 생각한다. 개념에서 다른게 없기 때문이다. 조금 복잡한 것은 그리드(Grid)모드와 리스트(List)모드의 호환이다.

다음은 리액트 네이티브(React Native), Expo에서 실제 `ListView`에 그리드(Grid) 형태로 리스트(List)로 각각 표현하는 내용을 쓸 것이다. 실제 인스타그램의 내 프로필 창과 정말 비슷해지고 있다.

인스타그램 [Profile화면 구현하기 1편](https://myksb1223.github.io/reactnative/2019/03/24/React-Native-20.html) 보기

전체 소스는 [링크](https://github.com/myksb1223/ReactNative-instagram-example)로 가면 볼 수 있다.
