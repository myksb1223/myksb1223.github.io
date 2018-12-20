---
layout: post
title:  "클래스업 데이터베이스 구조.(Database Structure of ClassUp)."
date:   2018-12-20 11:15:00
author: Seungbeom Kim
categories: develop_diary
tags:	클래스업 ClassUp 데이터베이스 Database 구조 Structure
sitemap :
  changefreq : daily
  priority : 1.0
---

개발을 하던 도중, 클래스업(ClassUp)의 구조와 사용된 여러 알고리즘을 자주 잊어버려서 적어 놓아야 겠다는 생각을 했다. 그래서 블로그에다 작성을 조금씩 해보려고 한다.

### 클래스업(ClassUp)의 구조

클래스업(ClassUp)은 기본적으로 시간표(Schedule, Timetable), 수업(Class)로 구성된다.

시간표는 수업을 담고있는 하나의 구조화된 표이고 수업은 해당 시간표에 속하는 각각의 내용이다. 즉, 우리가 일반적으로 생각하는 시간표의 형식이라고 생각하면 된다.

기본적으로 시간표와 요일은 요일과 시간의 틀을 가지고 있다.

#### 시간표 데이터베이스 구조

**unique_id** `String`
시간표를 유일하게 구분할 수 있는 필드.

**server_timestamp** `String`
시간표 동기화를 위한 필드. 동기화 부분에서 자세히 설명할 것이다.

**startDay**, **endDay** `Integer (0 - 6)`
시간표의 시작요일과 종료요일 필드.
사용자마다 자신의 시간표의 시작요일과 종료요일을 설정 수 있다.
원래 기기에서의 weekday는 일요일이 1이고 토요일이 7인데... 모르는 상태로 개발했던터라... 0은 월요일, 6은 일요일이다.

**startHour**, **startMinute**, **endHour**, **endMinute** `Integer`
시간표의 시작시간의 시와 분, 종료시간의 시와 분 필드.
사용자는 자신의 시간표의 시작시간과 종료시간을 설정할 수 있다.

**isMain** `Integer`
메인 시간표를 나타내는 필드.
사용자는 여러개의 시간표를 만들 수 있으므로, 현재 보는 시간표가 무엇인지 확인할 필요가 있다.

**background_timestamp** `String`, **background_type** `Integer`, **background_number** `Integer`, **background_extension_type** `Integer`
배경화면을 동기화하기 위한 필드. 배경화면 동기화부분에서 자세히 설명할 것이다.

#### 수업의 데이터베이스 구조

**local_id** `String`
디바이스 내에서의 유일하게 구분할 수 있는 필드.
클래스업(ClassUp)은 여러개의 시간표를 만들 수 있고, 각 시간표 별로 같은 수업을 넣을 수 있으므로, 디바이스에서 구분할 수 있어야 한다. 시간표의 **unique_id**와 수업의 **unique_id**의 조합으로 구성된다.

**unique_id** `String`
수업의 유일하게 구분할 수 있는 필드.

**server_timestamp** `String`
수업 동기화를 위한 필드. 동기화 부분에서 자세히 설명할 것이다.

**subjectName** `String`
수업 이름을 위한 필드.

**nthDay**, **nthStartTime**, **nthEndTime**, **nthClassRoom** `String`
클래스업(ClassUp)은 한번에 수업을 10개까지 넣을 수 있다. `n`은 몇 번째 수업인지를 나타낸다.

    요일 : 월, 수
    시작시간 : 11:00(월), 13:00(수)
    종료시간 : 12:15(월), 14:14(수)
    강의실 : 50514

위와 같은 수업이 있다면 `n`의 값은 2로 즉, 8개의 필드를 사용한다.

**uuid** `String`
링크주소를 위한 필드. 링크주소 관련된 부분에서 자세히 설명할 것이다.

**start_timestamp** `String`, **end_timestamp** `String`, **repeat_type** `Integer (0, 1)`
해당 수업의 기간을 지정하기 위한 필드이다. 클래스업(ClassUp)의 달력 모드와 호환되는 기능이다.
`repeat_type`이 0이면 매주, 1면 격주이다.

    과목명 : 자료구조
    시작 : 2018년 03월 01일
    종료 : 2018년 06월 25일
    반복 : 격주
