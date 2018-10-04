---
layout: post
title:  "UITableView reloadData시 keyboard 사라짐 현상 (When do UITableView reloadData, keyboard removed)."
date:   2018-10-05 00:10:00
author: Seungbeom Kim
categories: develop_diary
tags:	아이폰 iOS UITableView UITextView UITextField Keyboard Objective-c
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에서 UITableView의 cell크기를 UITextField의 변화와 함께 변화시켜야 하는 경우가 생겼다.

상황은 아래와 같다.

1. UITableViewCell내에는 UITextField와 UIButton이 존재한다. UIButton은 안보이는 상태이고 UITextField 밑에 존재한다.
2. 2글자 이상의 글이 입력되면 해당 UITextField 밑에 버튼이 생겨야 하고 입력된 글이 UIButton에 계속 적용되어야 한다.
3. reloadData를 호출 시 UIButton이 생기면서 Keyboard가 사라진다.

이 때는 reloadData가 아닌 아래와 같은 방법으로 제어한다.

```Objective-c
- (void)textFieldDidChanged:(UITextField *)textField {

  // Input textField's text to button

  [self.tableView beginUpdates];
  [self.tableView endUpdates];
}
```

즉, `beginUpdates`와 `endUpdates`를 이용하여 단순히 높이만을 조절하게 할 수 있다.
