---
layout: post
title:  "이미지 기기에 저장(Saved images in device)."
date:   2018-07-10 05:27:00
author: Seungbeom Kim
categories: develop_diary
---

지금의 클래스업(ClassUp)의 노트에서 Image는 단순히 S3(Amazon S3)에만 저장을 하고 가져오는 형식이다.
생각해 보면 크게 문제가 있지는 않다. 어차피 Image는 한번 가져오면 Caching하기 때문에 딱히, 네트워크 자원을 크게 소모한다고 생각하지 않았기에...
문제라면 몇가지를 생각해 볼 수 있다.
1. Image가 바로 보이지 않는다. S3에 올라간 후, 다운을 받아야 보인다.
2. 내용을 수정 시, Image를 변경할 수는 없다.(억지로 하면 하겠지만... 복잡하다.)

그런데 시간표의 메인 화면을 동기화되는 방식으로 바꿨는데 여기서는 Image가 기기에 저장이 된다. 워낙 자주 들어가는 곳이기도 하고 배경을 변경 시켰을 때, 바로 변경된 이미지를 적용시키기 위해서다. 이처럼 노트 쪽도 변경시켜 주기로 했다.

단순히, Image를 S3로 업로드 할 때, Cache 경로에 이미지를 S3에 저장되는 Uri를 '_'으로 변경한 값을 파일 이름으로 만들고 직접 파일 저장시켜줘도 된다.

하지만 Image가 완벽히 S3에 올라갔는지 아닌지를 확인하기 위해서는 다른 구조가 필요하다.
1. Failed 폴더와 Uploaded 폴더를 각각 만든다.
2. 무조건 올라가야하는 파일은 Failed 폴더에 넣는다.
3. 업로드가 완료된 파일은  Uploaded로 들어간다.
4. 이미지를 화면에 표현할 때, 4단계를 거친다.
-- Failed를 먼저 확인한다.
-- 있으면 Failed 표시, 없으면 Uploaded를 확인한다.
-- 있으면 Uploaded 표시, 없으면 S3로 가서 이미지를 다운로드 한다.
-- 다운로드 된 이미지는 Uploaded에 파일이 저장된다.


여기서 문제는 바로 이미지 수정 시 발생하는 문제이다.
Image의 파일 이름은 Timestamp를 이용하여 구성한 것은 아주 좋은 방법이라고 생각한다. A라는 사용자가 같은 이름으로 Image를 여러개 생성할 수 없으니까.
클래스업(ClassUp)의 Image 형식 : (user_id)_(timestamp)\_image\_(number).jpeg
그런데 수정할 때, 3개의 이미지 중, 2개를 지우고 다시 2개를 추가했다고 생각해보자. 아무런 문제가 없을 것 같이 보이지만 우리는 Cache를 하기 때문에 같은 이름일 경우에는 image가 이미 존재한다고 생각하고 예전 화면의 이미지를 출력해 준다.
결국 바뀐 이미지는 Cache가 만료 기간 동안 계속 예전 이미지를 보여줄 것이다.
그래서 Cache Image를 지울까도 생각해 보았다.

Android에서는 Fresco를 사용하기 때문에...
```java
Fresco.getImagePipeline().evictFromCache(uri);
```
위 처럼 지우면 된다.

iOS에서는 Cache 디렉토리로 가서 직접 지우면 된다.

```Objective-c
[[NSFileManager defaultManager] removeItemAtPath:failedPath error:&error];
```

위 처럼 해당 경로의 파일을 지워주면 된다.

그런데 생각보다 복잡했다. 하나씩 전부 Uri를 만들어 주면서 지워야 했고, 바로 적용시키는 코드를 만들어야 했다. 그래서 간단하게 Image가 변경되면 아예 Timestamp를 변경시켰다.

아주 간단했다. 무조건 Image가 변경되면 Timestamp가 변경되니까 별로 처리해 줄 것이 없었다.
예전 Timestamp로 저장된 이미지만 지워주면 끝나기 때문이다.

클래스업(ClassUp)은 다음 버전 부터는 이제 노트를 수정할 때  Image를 넣고 빼고를 자유롭게 할 수 있다.
