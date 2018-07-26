---
layout: post
title:  "Bulk API를 이용한 엘라스틱 서치 데이터 정리(Manage ElasticSearch Data Using Bulk API)"
date:   2018-07-24 05:31:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp 엘라스틱서치 ElasticSearch BulkAPI 벌크API 루비 Ruby RubyOnRails
---

꾸준히 개발은 했는데 이제서야 글을...

일단 서버에서 계속 500에러를 떨어뜨리는 부분이 있어서 자세히 들여다 봤더니... 검색 쪽 문제였다. 일단 클래스업(ClassUp)은 AWS의 DynamoDB와 ElasticSearch를 사용한다.

클래스업(ClassUp)에서 수업이 검색되는 경우는 누군가가 수업을 추가하고 그 수업에 속한 사용자가 1명이상일 경우이다. 만약 사용자가 직접 수업을 추가한 후, 혼자만 듣다가 지웠다면 그 수업은 검색되지 않는다.

이제부터 사용자 수는 EnrollCnt라고 하겠다.

어느 순간 DynamoDB의 데이터가 1300만건이 넘어가면서 약간 불안해 졌다. 그래서 EnrollCnt가 0인 수업은 모두 지웠다. 여기서 지우는 방법은 나중에 다른 글에서 작성할 생각이다. 여기서 한가지 간과한 점이 당연히 DynamoDB와 엘라스틱서치(ElasticSearch) 쪽 EnrollCnt가 동일하다고 생각했다. 하지만 달랐다. 과거의 ElasticSearh EnrollCnt를 설정하는 코드가 잘못되었던 것 같다.

결과적으로 검색은 되었는데 실제 수업이 없는 경우가 발생하기 때문에 500에러가 난 것이다. 그래서 새벽에 지우기로 마음을 먹고 지웠다.

일단 DynamoDB에서 지운 수업은 약 700~800만개이다. 여기서 지운 수업을 엘라스틱서치(ElasticSearch)에도 적용을 해야하는데 1번씩 700~800만번 호출하는 것은 낭비였다. 그래서 엘라스틱서치(ElasticSearch)에서 지원하는 [Bulk API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-bulk.html)를 Ruby에서 사용했다. 클래스업의 서버는 Ruby On Rails를 사용한다.

[Bulk API의 최적의 사이즈](https://www.elastic.co/guide/en/elasticsearch/guide/current/indexing-performance.html#_using_and_sizing_bulk_requests)는 얼마일까? 라는 궁금증이 들어서 찾아보았다.

작업 순서는 아래와 같다.
1. EnrollCnt가 0인 수업을 읽는다.
2. Bulk API를 사용할 것이므로 1000개씩 묶어서 한번에 처리한다.
3. 2번을 끝날 때까지 반복한다.

```Ruby
    save_ids = []
    file = File.open("./lib/real_zero.txt", "r")
    while (line = file.gets) do
      arr = line.strip
      save_ids << arr
    end

    arr = []
    for save_id in save_ids do
      delete_hash = Hash.new
      delete_hash["delete"] = Hash.new
      delete_hash["delete"]["_index"] = "subjects_v2"
      delete_hash["delete"]["_type"] = "subject"
      delete_hash["delete"]["_id"] = save_id.to_s

      if arr.count < 1000
        arr << delete_hash
      else
        $client.bulk body: arr
        arr = []
        arr << delete_hash
    end

    if arr.count != 0
      $client.bulk body: arr
      arr = []
    end
```

위와 같이 작성하고 돌리면 정말 금방 끝났다. 1000개도 솔직히 적은 양이지만 혹시 몰라서 1000개로 했다. 과유불급이라 했으니...

잘못된 부분, 질문등은 댓글이나 쪽지주세요~!
