---
layout: post
title:  "DynamoDB에서 Global-index 생성 시 주의점(Caution When creating global-index in DynamoDB)."
date:   2019-02-07 21:11:00
author: Seungbeom Kim
categories: develop_diary
tags:	DynamoDB GlobalIndex
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)을 개발하면서 수업 공지사항 관련 개념을 수업에 종속된 것이 아닌 자유롭게 이동할 수 있는 방식으로 업데이트하는 작업을 하게되었다.

그래서 DynamoDB를 사용하고 있었기에, 새로운 Global Index를 생성할 필요가 있었다.

    Global Index를 생성할 때는 자료의 양이 많을 경우 Write Capacity를 많이 올려고 하는 것이 빠르다.
    나는 기존 테이블의 Read Capacity도 올리고 하지만, 최근에 생성할 때 보면 밑에 걸리는 시간이 계산되어 나오는 것으로 봐서는 Write Capacity만 올려도 되는 것 같다.
    기존 테이블의 양은 많더라도 Global Index의 PartitionKey로 만들 데이터의 양이 적다면 생각보다 빠르게 작업이 끝나는 느낌이다.

일단 이렇게 생성된 후에 갑자기 아래와 같은 에러가 발생했다.

    Aws::DynamoDB::Errors::ValidationException (One or more parameter values were invalid: Schema violation against backfilling index)

위의 에러의 원인은 기존 테이블의 데이터를 update하던 중에 새롭게 생성한 Global Index에서의 PartitionKey의 값을 잘못된 값으로 넣으려 했기 때문이다. 예를 들면 빈칸이나 nil과 같은 값이다.

그러므로 Global Index를 만들 때는 해당 관련된 PartitionKey는 정확한 값으로 집어 넣어주도록 하는 것이 좋다.
