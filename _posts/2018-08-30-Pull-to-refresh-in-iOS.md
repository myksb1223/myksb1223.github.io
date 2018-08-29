---
layout: post
title:  "아이폰 당겨서 새로고침(Pull to refresh in iOS)"
date:   2018-08-30 04:52:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp 아이폰 iOS 당겨서새로고침 PullToRefresh Objective-c
sitemap :
  changefreq : daily
  priority : 1.0
---

약 6-7년 전에 pull to refresh를 구현할 수 없었고 해당 기능이 iOS 자체적으로 지원하지 않았기 때문에 라이브러리를 사용했다.

내가 사용한 라이브러리는 pull to loadmore도 구현되어 있었다. 그래서 계속 쓰려고 했지만 어느 순간 에러가 나기 시작해서... 역시나 해당 라이브러리는 더 이상 업데이트가 없었다.

결국 그냥 pull to loadmore는 맨 아래쪽으로 가면 더 부르는 방식으로 바꿨고, pull to refresh는 iOS에서 지원해주는 것으로 바꿨다.

```Objective-c
UIRefreshControl *refreshControl = [[UIRefreshControl alloc] init];
[refreshControl addTarget:self action:@selector(refreshTable:) forControlEvents:UIControlEventValueChanged];

if(iOS version >= 10) {
    [self.tableView setRefreshControl:refreshControl];
}
else {
    [self.tableView addSubview:refreshControl];
}
```

다른 부분은 신경안써도 괜찮지만 iOS 10이상과 미만을 구분해서 사용해주면 된다.

간단하게 구현되서 정말 편했다.
