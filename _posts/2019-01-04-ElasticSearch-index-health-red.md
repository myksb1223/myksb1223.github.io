---
layout: post
title:  "엘라스틱서치 index health red case.(AlarmManager with Doze mode and Battery Optimization in Android)."
date:   2019-01-04 21:13:00
author: Seungbeom Kim
categories: develop_diary
tags:	클래스업 ClassUp 엘라스틱서치 ElasticSearch
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에서 수업 검색을 위해서 ElasticSearch를 사용한다. 처음에는 schema를 직접 작성을 했지만, 여러 필드들이 추가되면서 자연스럽게 ElasticSearch에서 생성해주는 mapping을 사용하였다.

즉, 원래 맵핑이 아래와 같았다면,
```
curl -XGET 'localhost/subjects/_mapping/subject?pretty'
{
  "subjects_v3" : {
    "mappings" : {
      "subject" : {
        "properties" : {
          "@version" : {
            "type" : "keyword"
          },
          "classProf" : {
            "type" : "text"
          },
          "created_at" : {
            "type" : "date",
            "format" : "epoch_millis||dateOptionalTime"
          },
          "eighthClassRoom" : {
            "type" : "text"
          },
          "end_timestamp" : {
            "type" : "text"
          },
          "enrollCnt" : {
            "type" : "integer"
          },
          "firstClassRoom" : {
            "type" : "text"
          },
          "firstDay" : {
            "type" : "text"
          },
          "firstEndTime" : {
            "type" : "text"
          },
          "firstStartTime" : {
            "type" : "text"
          },
          "geoip" : {
            "dynamic" : "true",
            "properties" : {
              "location" : {
                "type" : "geo_point"
              }
            }
          },
          "id" : {
            "type" : "text"
          },
          "isOnline" : {
            "type" : "integer"
          },
          "notice_timestamp" : {
            "type" : "text",
            "fields" : {
              "keyword" : {
                "type" : "keyword",
                "ignore_above" : 256
              }
            }
          },
          "professor_id" : {
            "type" : "text"
          },
          "repeat_type" : {
            "type" : "integer"
          },
          "semester" : {
            "type" : "integer"
          },
          "start_timestamp" : {
            "type" : "text"
          },
          "subjectName" : {
            "type" : "text",
            "fields" : {
              "ngrams" : {
                "type" : "text",
                "analyzer" : "my_ngram_analyzer"
              }
            }
          },
          "subjectYear" : {
            "type" : "integer"
          },
          "subject_id" : {
            "type" : "text"
          },
          "unique_id" : {
            "type" : "text"
          },
          "university_id" : {
            "type" : "integer"
          },
          "update_timestamp" : {
            "type" : "text"
          },
          "updated_at" : {
            "type" : "date",
            "format" : "epoch_millis||dateOptionalTime"
          }
        }
      }
    }
  }
}
```

필드가 추가되었는데 자동적으로 ElasticSearch가 필드를 추가하게 하면 아래와 같이 된다.

```
curl -XGET 'localhost/subjects/_mapping/subject?pretty'
{
  "subjects_v3" : {
    "mappings" : {
      "subject" : {
        "properties" : {
          "@version" : {
            "type" : "keyword"
          },
          "classProf" : {
            "type" : "text"
          },
          "created_at" : {
            "type" : "date",
            "format" : "epoch_millis||dateOptionalTime"
          },
          "eighthClassRoom" : {
            "type" : "text"
          },
          "end_timestamp" : {
            "type" : "text"
          },
          "enrollCnt" : {
            "type" : "integer"
          },
          "firstClassRoom" : {
            "type" : "text"
          },
          "firstDay" : {
            "type" : "text"
          },
          "firstEndTime" : {
            "type" : "text"
          },
          "firstStartTime" : {
            "type" : "text"
          },
          "geoip" : {
            "dynamic" : "true",
            "properties" : {
              "location" : {
                "type" : "geo_point"
              }
            }
          },
          "id" : {
            "type" : "text"
          },
          "isOnline" : {
            "type" : "integer"
          },
          "notice_timestamp" : {
            "type" : "text",
            "fields" : {
              "keyword" : {
                "type" : "keyword",
                "ignore_above" : 256
              }
            }
          },
          "professor_id" : {
            "type" : "text"
          },
          "repeat_type" : {
            "type" : "integer"
          },
          "semester" : {
            "type" : "integer"
          },
          "start_timestamp" : {
            "type" : "text"
          },
          "subjectName" : {
            "type" : "text",
            "fields" : {
              "ngrams" : {
                "type" : "text",
                "analyzer" : "my_ngram_analyzer"
              }
            }
          },
          "subjectYear" : {
            "type" : "integer"
          },
          "subject_id" : {
            "type" : "text"
          },
          "unique_id" : {
            "type" : "text"
          },
          "university_id" : {
            "type" : "integer"
          },
          "update_timestamp" : {
            "type" : "text"
          },
          "updated_at" : {
            "type" : "date",
            "format" : "epoch_millis||dateOptionalTime"
          },
          "user_id" : {
            "type" : "text",
            "fields" : {
              "keyword" : {
                "type" : "keyword",
                "ignore_above" : 256
              }
            }
          }
        }
      }
    }
  }
}
```
`notice_timestamp`나 `user_id` 부분과 같이 작동한다. 나는 `user_id.keyword` 라는 필드를 사용하지 않는데 계속 이런 부분에서 에러가 난다. 그래서 데이터를 삭제한 후 merge할 때나 데이터를 넣을 때 health 상태가 red가 되고 CPU가 거의 100%까지 올라간다.

그래서 항상 필드가 추가될 경우는 직접 mapping을 새롭게 짜주는 것이 좋다.

그리고 데이터가 들어가는 도중에 index를 삭제할 경우, 바로 삭제가 되지 않는다. 그래서 항상 해당 index로 들어가는 모든 통로를 막은 후 지워야 한다.

결론적으로
- 맵핑이 바뀌면 직접 맵핑을 추가해준다.
- 항상 인덱스를 지울 때는 index로 들어오는 데이터를 모두 차단한 후 지운다.
