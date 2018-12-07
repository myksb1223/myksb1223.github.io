---
layout: post
title:  "HttpUrlConnection의 timeout(Timeout in HttpUrlConnection)."
date:   2018-12-08 04:21:00
author: Seungbeom Kim
categories: develop_diary
tags:	안드로이드 Android HttpUrlConnection Timeout
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에서 핵심은 동기화이다. 그래서 여러 상황 중에 네트워크 상태가 disabled된 상태에서 여러 작업을 한 후, 다시 네트워크를 켰을 때, 해당 변화가 다른 기기에 적용되는지 테스트하였다.

해당 테스트 중에 네트워크를 다시키면 HttpUrlConnection이 작동하지 않는 것을 발견했다. HttpUrlConnection은 AsyncTask와 함께 작동하기 때문에 원인을 AsyncTask에서 부터 찾았다.

하지만 AsyncTask때문은 아니었다. 왜냐하면 AsyncTask를 큐(Queue)의 형식으로 관리하기 때문이다.

어쨌든 로그를 찍어 원인을 파악하던 중, Connection이 연결되고 난 후, Response를 받지 못하고 있었다. 이로 인해, 그 다음 작업, 그 다음 작업 모두 AsyncTask 자체가 수행되지 않았고, 네트워크를 Enabled 시킨 상태에서도 아무런 통신이 이루어지지 않았던 것이다.

그래서 알아본 결과 `ReadTimeout` 이라는 것을 지정해 주어야 했다.

ReadTimeout이란 서버와 연결된 후, 서버가 나에게 Response할 데이터를 반환할 때까지 기다리는 시간을 뜻한다. 즉, HttpUrlConnection은 기본적으로 이 시간이 0인데, 이 말은 영원하다는 뜻이다.

즉, 위와 같이 기본값인 0으로 설정되어 있었기에 생긴 문제였다. 위의 값은 1초로 하기엔 너무 짧은 것 같아 5초를 주었다.

```Android
//...

URL url = new URL(addr);
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

//...

conn.setReadTimeout(5000);

//...

if(conn.getResponseCode() == HttpURLConnection.HTTP_OK) {
  // Something to do.
}

conn.disconnect();
```

위의 내용을 몰라서 항상 Activity나 Fragment의 onResume()에 재연결 코드를 넣었었는데... 이제는 그럴 필요가 없어졌다. HttpUrlConnection에 대해 조금 더 깊이 생각해 볼 수 있는 시간이었다.
