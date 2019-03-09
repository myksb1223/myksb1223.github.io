---
layout: post
title:  "Github 블로그 404 페이지 오류(GitHub pages 404 not found)"
date:   2019-03-09 23:52:00
author: Seungbeom Kim
categories: develop_diary
tags:	iOS GitHub Protocol
sitemap :
  changefreq : daily
  priority : 1.0
---

오늘 GitHub 블로그 Repository를 모르고 Private 설정을 했다.
이 이후로... 블로그가 갑자기 404에러를 띄웠다.

아무리 Public으로 변경해도 블로그는 돌아오지 않았다...
그래서 열심히 검색한 결과... 아래처럼 빈 커밋을 하면 된다.

    git commit --allow-empty -m "Trigger rebuild"
    git push

[여기](https://stackoverflow.com/a/45907768)에 해당 설명이 있다.
