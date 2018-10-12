---
layout: post
title:  "엘라스틱서치 query와 bulk(ElasticSearch query and bulk)."
date:   2018-10-12 21:44:00
author: Seungbeom Kim
categories: develop_diary
tags:	엘라스틱서치 ElasticSearch 서버 Server Backend RubyOnRails Ruby
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에서 수업 검색과 학교 검색을 위해서 엘라스틱서치를 사용하고 있다.

순식간에 용량이 많이 쌓이는 수업은 주기적으로 지워주는데 아래와 같은 방법으로 지우고 있다.

```ruby
body = []
datas = $client.search index: 'index_name', body: {query: {bool: {filter: {range: {count: { lte: 0}}}}}, size: 100}
while datas["hits"]["hits"].count != 0 do
  for data in datas["hits"]["hits"] do
    delete_hash = Hash.new
    delete_hash["delete"] = Hash.new
    delete_hash["delete"]["_index"] = "index_name"
    delete_hash["delete"]["_type"] = "type_name"
    delete_hash["delete"]["_id"] = data["_id"]
    body << delete_hash
  end
  $client.bulk body: body
  sleep(1)
  body = []
  datas = $client.search index: 'index_name', body: {query: {bool: {filter: {range: {count: { lte: 0}}}}}, size: 100}
end
```

수업을 듣고있는 사용자가 0인 수업을 100개씩 읽는다. 그리고 난 후 해당 100개를 bulk api를 이용하여 모아서 한번에 지운다. sleep(1)을 해준 이유는 너무 빠르게 연속적으로 처리하다 보니 순식간에 엘라스틱서치 쪽 CPU 부하가 40%를 넘어갔기 때문이다.

약, 170만개의 수업을 지워야 했는데 대략 10시간 이상은 걸린 것 같다...

오랜만에 등록된 학교 중에 학생 수가 0인 학교를 찾아서 지우는 작업을 했다.

학교는 사용자 테이블에 직접적으로 관련된 것이므로 MySQL에서 직접 사용자가 있는지 없는지 확인하는 과정이 필요하다.

```ruby
size = 100
zero = []
datas = $client.search index: 'index_name', body: {query: {bool: {filter: {range: {student_count: { lte: 4}}}}}, size: size}
while datas["hits"]["hits"].count != 0 do
  for data in datas["hits"]["hits"] do
    users = User.find_by_sql("SELEST id FROM users WHERE university_id = \'#{data["_id"]}\'")
    @logger.info "uni_id : \'#{data["_id"]}\', user_count : \'#{users.count}\'"
    if users.count == 0
      zero << data["_id"]
    end
  end
  id = datas["hits"]["hits"].last["_id"]
  datas = $client.search index: 'index_name', body: {query: {bool: {filter: [{range: {student_count: { lte: 4}}}, {range: {id: { gt: id}}}]}}, size: size}
end

body = []
for id in zero do
  delete_hash = Hash.new
  delete_hash["delete"] = Hash.new
  delete_hash["delete"]["_index"] = "index_name"
  delete_hash["delete"]["_type"] = "type_name"
  delete_hash["delete"]["_id"] = id

  body << delete_hash

  if body.count > 100
    $client.bulk body: body
    sleep(1)
    body = []
  end
end

$client.bulk body: body
sleep(1)
body = []
```

혹시 정확한 카운팅이 제대로 되지 않았을 가능성을 감안하여 5미만의 학생이 등록된 학교를 읽는다. 약 67000개 정도가 있다.

처음에는 100개만 읽는다. 그 이후로는 이전 100개의 마지막 데이터의 id보다 큰 학교를 100개 읽는다. 사실 `from`이라는 필드를 사용할 수 있지만 10000을 넘어가지 못하기 때문에... 사용할 수 없다.

약간 주의할 사항은 `range`는 오직 하나의 필드에만 적용되기 때문에 여러번 쓰기 위해서는 위와 같이 []내에 여러개로 나눠어서 사용해야 한다.
