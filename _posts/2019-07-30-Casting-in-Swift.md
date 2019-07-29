---
layout: post
title:  "Swift에서 Casting 하기(Casting in Swift)"
date:   2019-07-30 01:19:00
author: Seungbeom Kim
categories: develop_diary
tags:	iOS Swift Any AnyObject
sitemap :
  changefreq : daily
  priority : 1.0
---

Swift를 공부하면서 느낀 것은 일단 `Casting`에 대한 부분이 Objective-C와 많이 다르다는 것이다.

타입 체크는 `is`로 바뀌었다. 이 부분은 매우 만족한다.
`Casting`은 `as`라는 것으로 하는데... 이 부분이 매우 헷갈린다.

일단 Swift의 특성상 `as`, `as?`, `as!` 이렇게 3가지로 나눌 수 있다.

### as의 경우

```Swift
PHImageManager.default().requestImageData(for: asset, options: nil) {
   (data, string, orientation, info) in
    // data는 Optional이다.
    let path = "..."
    if let data = data {
        (data as NSData).write(toFile: path, atomically: true)
    }
}
```

위 경우 data의 Optional을 처리하지 않으면 아래와 같은 에러가 생긴다.

    'Data?' is not convertible to 'NSData'; did you mean to use 'as!' to force downcast?

그래서 해당 에러에 맞춰서 수정했다.

```Swift
(data as! NSData).write(toFile: path, atomically: true)
```

결과는 아래와 같은 경고가 뜬다.

    Forced cast from 'Data?' to 'NSData' only unwraps and bridges; did you mean to use '!' with 'as'?

즉, Optional에서의 Casting은 하지말라는 것이다. 결국 처음 방법과 아래와 같이 Unwrapping을 해주는 것이다.

```Swift
(data! as NSData).write(toFile: path, atomically: true)
```

### as?의 경우

```Swift
func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {

    if let image = info[UIImagePickerController.InfoKey.originalImage] as? UIImage {
        // image 처리히기.
    }

    picker.dismiss(animated: true, completion: nil)
}
```

위 코드에서 `info`부분의 value가 `Any`이기 때문에 저렇게 Casting해 주어야 한다.

`as?`는 말 그대로 형 변환에 실패하면 nil을 던져주기 때문에 Optional을 벗겨내는 방법을 사용해야 한다.

### as!의 경우

```Swift
let views = self.tableView.subviews
for view in views {
   if view is UITableViewCell {
       let cell = tableCell as! UITableViewCell
       // cell 처리
   }
}
```

위의 코드를 보면 일단 `is`를 사용해서 타입을 확인했다. 그래서 `as`를 쓰고 싶지만 사용하면 아래와 같은 에러가 뜬다.

    'UIView' is not convertible to 'UITableViewCell'; did you mean to use 'as!' to force downcast?

즉, 여기서 `as`는 **downcast**에서는 사용할 수 없다는 것을 알 수 있다.

결론적으로 `as!`는 형 변환에서 에러가 나지 않는다는 확신이 든다면 사용하면 좋을 것 같다.
