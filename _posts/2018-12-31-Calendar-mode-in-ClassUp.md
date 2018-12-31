---
layout: post
title:  "클래스업 달력 모드.(Calendar mode in ClassUp)."
date:   2018-12-31 23:57:00
author: Seungbeom Kim
categories: develop_diary
tags:	클래스업 ClassUp Calendar 달력
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)을 시간표는 한국에 최적화된 시간표 화면을 가지고 있었다. 해외 사용자들은 시간표가 굉장히 유동적이다. 예를 들면 같은 수업이라도 매주 수업 시간이 다르다. 그래서 클래스업(ClassUp)의 시간표 화면을 달력형식으로 가져가기로 했다.

달력형식을 위해 필요한 부분은...
1. 시간표의 무한 페이지
2. 각 페이지 별 수업

무한 페이지를 위해서 iOS는 [FSPagerView](https://github.com/WenchaoD/FSPagerView)라는 라이브러리를 사용했고 안드로이드는 구글 Calendar 앱을 참고하여 직접 만들었다. 구글 Calendar를 참고하여 만든 소스는 조만간 블로그에 쓸 예정이다.

각 페이지 별 수업을 위해서 수업 데이터베이스 필드에 `repeat_type` 및 `start_timestamp`, `end_timestamp`를 넣었다. 자세한 것은 해당 [링크](https://myksb1223.github.io/develop_diary/2018/12/20/Database-Structure-of-ClassUp.html)를 참조하면 된다.

본격적인 코딩에 들어가기 전에 시나리오를 생각해보면...

먼저, 앱이 켜지면 시간표 화면이 뜬다. 이 때는 **오늘**이 포함된 주가 보이면 된다.

    주의할 사항은 한국은 대부분 월요일이 첫 시작이지만 해외는 일요일이 시작일 수도 있다. 즉, 클래스업(ClassUp)은 시간표의 시작요일과 종료요일을 지정할 수 있다. 즉, 월 ~ 금 또는 일 ~ 목 등과 같이 지정할 수 있다.

이번 주가 보였다면, 좌, 우 스크롤 시 이번 주의 시작 날짜 +7, -7을 하면서 다음 주, 지난 주를 표현하면 된다.

자 그러면 시간표의 시작 날짜는 어떻게 계산할 수 있을까?

    startDay : 시간표의 시작요일
    endDay : 시간표의 종료요일
    today : 오늘 요일

요일은 일요일부터 토요일까지 1에서 7의 값을 가진다.

이 때, 시작요일이 화요일이고 종료요일이 월요일이라고 가정해보자.
startDay : 3, endDay : 2이 된다.

이렇게 startDay가 endDay보다 큰 경우를 `역방향`이라고 하겠다.
그러면 반대를 `순방향`이라고 할 수 있다.

순방향인 경우는 별로 생각할 것이 없는데 역방향일 때는 생각하기가 까다롭다. 그렇기 때문에 이 역방향을 순방향으로 만들어주기 위해서 endDay에 7을 더해주어서 순방향으로 만들어서 생각한다.

아래 처럼 생각하면 endDay가 9이면 월요일이 된다.

    일(1) 월(2) 화(3) 수(4) 목(5) 금(6) 토(7) 일(8) 월(9) 화(10) 수(11) 목(12) 금(13)

자 그러면 오늘이 12월 31일 월요일이고 시간표는 화요일 ~ 월요일이라고 하면 아래와 같이 표현할 수 있다.

    startDay : 3
    endDay : 9
    today : 2

이 때, today는 무조건 startDay와 endDay사이에 와야한다. 왜냐하면 이번 주라는 것은 today가 포함된다는 뜻이기 때문이다. 그렇기 때문에 today가 startDay보다 작으면 today에 7을 더해준다.

    startDay : 3
    endDay : 9
    today : 9

시간표의 시작날짜는 오늘 날짜에서 (startDay - today)를 더하면 된다.

만약, (startDay - today)가 음수의 값이면 오늘에서 앞으로 당기면 되는 것이고, 양수면 그만큼 뒤로 물리면 된다. 그런데 무조건 0보다 작은 값만 나온다.

예를 들어 오늘이 12월 24일, 시작요일 화, 종료요일 월이면

    시간표 시작 날짜 : 12월 18일
    시간표 종료 날짜 : 12월 24일
    오늘 : 12월 24일

위와 같은 결과가 나온다.

코드로 설명하면...
```Objective-C
NSString *tableStartTimestamp = nil, *tableEndTimestamp = nil;

NSDate *date = [NSDate date];
NSCalendar *cal = [NSCalendar currentCalendar];
NSDateComponents *comps = [cal components:(NSCalendarUnitYear | NSCalendarUnitMonth | NSCalendarUnitDay | NSCalendarUnitHour | NSCalendarUnitMinute | NSCalendarUnitWeekday) fromDate:date];
[comps setHour:0];
[comps setMinute:0];
[comps setSecond:0];
date = [cal dateFromComponents:comps];

NSDateComponents *dayComponent = [[NSDateComponents alloc] init];
[dayComponent setHour:0];
[dayComponent setMinute:0];
[dayComponent setSecond:0];

NSInteger today_day = comps.weekday;
if(startDay <= endDay) {
    // Nothing to do.
}
else {
    endDay = endDay + 7;
    if(today_day < startDay) {
        today_day += 7;
    }
}

dayComponent.day = startDay-today_day;
date = [cal dateByAddingComponents:dayComponent toDate:date options:0];
self.defaultTableStartTimestamp = [NSString stringWithFormat:@"%lld", (long long)([date timeIntervalSince1970] * 1000)];

dayComponent.day = endDay-startDay;
date = [cal dateByAddingComponents:dayComponent toDate:date options:0];
date = [date dateByAddingTimeInterval:(60*60*24-1)];
tableEndTimestamp = [NSString stringWithFormat:@"%lld", (long long)([date timeIntervalSince1970] * 1000)];
```

```Java
String tableStartTimestamp = null, tableEndTimestamp = null;

Calendar cal = Calendar.getInstance();
singleton.today_day = cal.get(DAY_OF_WEEK);
cal.set(Calendar.HOUR_OF_DAY, 0);
cal.set(Calendar.MINUTE, 0);
cal.set(Calendar.SECOND, 0);
cal.set(Calendar.MILLISECOND, 0);

int today_day = singleton.today_day;
Log.d(TAG, "start : " + startDay + ", end : " + endDay + ", today_day : " + singleton.today_day);
if(startDay <= endDay) {
}
else {
    endDay = endDay + 7;
    if(today_day < startDay) {
        today_day += 7;
    }
}

cal.add(Calendar.DAY_OF_MONTH, startDay-today_day);
tableStartTimestamp = Long.toString(cal.getTimeInMillis());

cal.add(Calendar.DAY_OF_MONTH, endDay-startDay);
cal.set(Calendar.HOUR_OF_DAY, 23);
cal.set(Calendar.MINUTE, 59);
cal.set(Calendar.SECOND, 59);
cal.set(Calendar.MILLISECOND, 999);
tableEndTimestamp = Long.toString(cal.getTimeInMillis());
```

이렇게 시간표 표현에 대한 시나리오를 끝냈다면 이제는 수업을 표현할 시나리오를 짜야한다.

수업은 아래 처럼 3가지 경우가 존재한다.

- 기간이 없는 수업
- 기간이 있으면서 매주 반복인 수업
- 기간이 있으면서 격주 반복인 수업

기간이 없는 수업인 경우에는 아래와 같이 읽으면 된다.

    SELECT * FROM subject WHERE start_timestamp IS NULL AND end_timestamp IS NULL

기간이 있는 수업인 경우에는 보이는 시간표의 주의 시작 날짜와 종료 날짜를 고려해야 한다. 시간표의 시작 날짜를 table_start_timestamp, 종료 날짜를 table_end_timestamp라 한다면 아래와 같이 표현할 수 있다.

    SELECT * FROM subject WHERE CAST(start_timestamp AS INTEGER) <= table_end_timestamp AND CAST(end_timestamp AS INTEGER) >= table_start_timestamp

이렇게 읽혀진 수업이 아래와 같다고 가정해 보자.

    수업이름 : 자료구조
    수업기간 : 2018.09.03 - 2018.12.20
    수업시간 :
        월 15:00 - 16:15
        수 16:30 - 17:45
    반복타입 : 격주

이 수업은 그러면 어떻게 표현되어야 할까?

일단 월, 수 따로 시간표에 표현되어야 하기 때문에 두개를 나눠서 생각하기로 한다.

그리고 난 후에는 월요일의 날짜와 수요일의 날짜를 계산해야한다. 이를 위해서는 아래의 과정이 필요하다.

- 월요일과 수요일이 각각 실제 1~13사이에서 무슨 숫자를 가지는지 알아야 한다.
- 매주 반복인지 격주 반복인지 체크한다.
- 매주 반복이면 구한 요일 값과 시간표의 시작 요일을 기준으로 계산한 날짜가 수업의 기간에 포함되는지 확인하고 들어오면 추가한다.
- 격주 반복이면 현재 보이는 시간표의 주의 시작 날짜와 수업 시작 날짜가 포함된 주의 시간표 시작 날짜를 이용하여 현재 보이는 시간표의 주에 수업이 포함되는지 안되는지를 파악한다.

이렇게 말로 풀어서 설명하면 힘드니까 예를 들어서 설명하면...

    월, 수는 각각 9, 4의 값을 가진다.
    table_start_timestamp : 2018년 12월 18일 (화, 3)
    table_end_timestamp : 2018년 12월 24일 (월, 9)

    start_timestamp : 2018년 09월 03일 (월, 9)
    end_timestamp : 2018년 12월 20일 (목, 5)

    start_timestamp가 포함되는 시간표의 시작 날짜 : 2018년 08월 28일(화, 3)

    2018년 12월 18일 - 2018년 08월 28일 = 112일이다.
    112/7로 나누어 떨어진다. 즉, 2018년 12월 18일 ~ 2018년 12월 24일 주에서 자료구조라는 수업은 보여야 한다. 하지만 24일이 월요일 이므로 19일 수요일 수업만 보이면 된다.

즉, iOS는 아래와 같은 알고리즘으로 작성하면 된다.

```Objective-C
/*  day : 수업이 있는 요일. (예시에서 월 또는 수가 해당 됨.)
    startDay : 시간표의 시작 요일. (예시에서 화.)
    isReverse : 역방향이면 YES.
*/
+ (NSInteger)getCalendarDay:(NSString *)day startDay:(NSInteger)startDay isReverse:(BOOL)isReverse {
    NSString *kDays[8] = {@"", @"일", @"월", @"화", @"수", @"목", @"금", @"토"};
    NSString *eDays[8] = {@"", @"Sun", @"Mon", @"Tue", @"Wed", @"Thu", @"Fri", @"Sat"};

    int real_day = 0;
    for(int i=1; i<8; i++) {
        if([kDays[i] isEqualToString:day] || [eDays[i] isEqualToString:day]) {
            if(isReverse) {
                if(i < startDay) {
                    // 무조건 startDay 보다 커야만 하기에 역방향일 경우에서는 7을 더해준다. 이번 주라는 개념을 생각!
                    real_day = i + 7;
                }
                else {
                    real_day = i;
                }
            }
            else {
                real_day = i;
            }
            break;
        }
    }

    return real_day;
}

/*  day : 수업이 있는 요일. (예시에서 월 또는 수가 해당 됨.)
    isReverse : 역방향이면 YES.
    repeatType : 격주는 0, 매주는 1.
    startDay : 시간표의 시작 요일.
    endDay : 시간표의 시작 요일.
    tableStartTimestamp : 현재 보이는 시간표의 시작 날짜를 timestamp로 표시한 것.
    tableEndTimestamp : 현재 보이는 시간표의 종료 날짜를 timestamp로 표시한 것.
    startTimestamp : 수업의 시작 날짜를 timestamp로 표시한 것.
    endTimestamp : 수업의 종료 날짜를 timestamp로 표시한 것.
*/
+ (BOOL)canAddedInSubjectListsWithDay:(NSString *)day isReverse:(BOOL)isReverse repeatType:(NSInteger)repeatType startDay:(NSInteger)startDay endDay:(NSInteger)endDay tableStartTimestamp:(NSString *)tableStartTimestamp tableEndTimestamp:(NSString *)tableEndTimestamp startTimestamp:(NSString *)startTimestamp endTimestamp:(NSString *)endTimestamp {

    BOOL isCanAdded = NO; // YES일 경우에는 해당 주에 표현이 됨.

    NSCalendar *cal = [NSCalendar currentCalendar];
    NSDateComponents *comps = [[[NSDateComponents alloc] init] autorelease];

    if(startTimestamp == nil || endTimestamp == nil) {
        // 기간이 없으면 그냥 무조건 표현하면 된다.
        return YES;
    }

    if(isReverse) {
        // 역방향이면 순방향형태로 계산을 위해 7을 더해준다.
        endDay += 7;
    }

    long long l_start_timestamp = [startTimestamp longLongValue];
    long long l_end_timestamp = [endTimestamp longLongValue];
    NSInteger real_fDay = [self getCalendarDay:day startDay:startDay isReverse:isReverse];

    NSDate *date = [NSDate dateWithTimeIntervalSince1970:[tableStartTimestamp longLongValue]/1000];
    if(real_fDay >= startDay && real_fDay <= endDay) {
        if(repeatType == 0) {
            comps.day = real_fDay - startDay;
            NSDate *f_date = [cal dateByAddingComponents:comps toDate:date options:0];
            long long fDay_timestamp = [f_date timeIntervalSince1970] * 1000;

            if(fDay_timestamp <= l_end_timestamp && fDay_timestamp >= l_start_timestamp) {
                isCanAdded = YES;
            }
        }
        else {
            long long lt_start_timestamp = [tableStartTimestamp longLongValue];

            NSDate *ss_date = [NSDate dateWithTimeIntervalSince1970:l_start_timestamp/1000];
            NSDateComponents *ss_comp = [cal components:NSCalendarUnitWeekday fromDate:ss_date];
            NSInteger ss_week_day = ss_comp.weekday;

            if(isReverse) {
                if(ss_week_day < startDay) {
                    ss_week_day += 7;
                }
            }

            comps.day = startDay-ss_week_day;
            ss_date = [cal dateByAddingComponents:comps toDate:ss_date options:0];

            NSDate *ts_date = [NSDate dateWithTimeIntervalSince1970:lt_start_timestamp/1000];
            NSDateComponents *duration = [cal components:NSCalendarUnitDay fromDate:ss_date toDate:ts_date options:0];

            if((duration.day / 7) % 2 == 0) {
                comps.day = real_fDay - startDay;
                NSDate *f_date = [cal dateByAddingComponents:comps toDate:date options:0];
                long long fDay_timestamp = [f_date timeIntervalSince1970] * 1000;

                if(fDay_timestamp <= l_end_timestamp && fDay_timestamp >= l_start_timestamp) {
                    isCanAdded = YES;
                }
            }
        }
    }

    return isCanAdded;
}
```

```Java
/*  day : 수업이 있는 요일. (예시에서 월 또는 수가 해당 됨.)
    startDay : 시간표의 시작 요일. (예시에서 화.)
    isReverse : 역방향이면 true.
*/
public static int getCalendarDay(String day, int startDay, boolean isReverse) {
        String[] kDays = {"", "일", "월", "화", "수", "목", "금", "토"};
        String[] eDays = {"", "Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"};

        int real_day = 0;
        for(int i=1; i<kDays.length; i++) {
            if(kDays[i].equals(day) || eDays[i].equals(day)) {
                if(isReverse) {
                    if(i < startDay) {
                        real_day = i + 7;
                    }
                    else {
                        real_day = i;
                    }
                }
                else {
                    real_day = i;
                }
                break;
            }
        }

        return real_day;
}

/*  day : 수업이 있는 요일. (예시에서 월 또는 수가 해당 됨.)
    isReverse : 역방향이면 true.
    repeatType : 격주는 0, 매주는 1.
    start_day : 시간표의 시작 요일.
    end_day : 시간표의 시작 요일.
    table_start_timestamp : 현재 보이는 시간표의 시작 날짜를 timestamp로 표시한 것.
    lt_end_timestamp : 현재 보이는 시간표의 종료 날짜를 timestamp로 표시한 것.
    startTimestamp : 수업의 시작 날짜를 timestamp로 표시한 것.
    end_timestamp : 수업의 종료 날짜를 timestamp로 표시한 것.
*/
public static boolean canAddedInSubjectLists(Calendar cal, String day, boolean isReverse, int repeat_type,
                                          int start_day, int end_day, String table_start_timestamp,
                                                 long lt_end_timestamp, String start_timestamp, String end_timestamp) {
        boolean isCanAdded = false;

        if(start_timestamp == null || end_timestamp == null) {
            return true;
        }

        if(isReverse) {
            end_day += 7;
        }

        long l_start_timestamp = (long)Long.parseLong(start_timestamp);
        long l_end_timestamp = (long)Long.parseLong(end_timestamp);
        int real_fDay = getCalendarDay(day, start_day, isReverse);

        cal.setTimeInMillis(Long.parseLong(table_start_timestamp));
        if(real_fDay >= start_day && real_fDay <= end_day) {
            if(repeat_type == 0) {
                cal.add(Calendar.DAY_OF_MONTH, real_fDay - start_day);
                long fDay_timestamp = cal.getTimeInMillis();
                if(fDay_timestamp <= l_end_timestamp && fDay_timestamp >= l_start_timestamp) {
                    isCanAdded = true;
                }
            }
            else {
                long lt_start_timestamp = (long)Long.parseLong(table_start_timestamp);
                long temp_lt_start_timestamp = lt_start_timestamp;
                long temp_lt_end_timestamp = lt_end_timestamp;

                Calendar ss_cal = Calendar.getInstance();
                ss_cal.setTimeInMillis(l_start_timestamp);
                int ss_week_day = ss_cal.get(DAY_OF_WEEK);

                if(isReverse) {
                    if(ss_week_day < start_day) {
                        ss_week_day += 7;
                    }
                }

                ss_cal.add(Calendar.DAY_OF_MONTH, start_day - ss_week_day);

                Calendar cal2 = Calendar.getInstance();
                cal2.setTimeInMillis(temp_lt_start_timestamp);

                long duration = ClassUpUtil.getDaysFromToDate(ss_cal, cal2);
                if((duration/7)%2 == 0) {
                    cal.add(Calendar.DAY_OF_MONTH, real_fDay - start_day);
                    long fDay_timestamp = cal.getTimeInMillis();

                    if(fDay_timestamp <= l_end_timestamp && fDay_timestamp >= l_start_timestamp) {
                        isCanAdded = true;
                    }
                }
            }
        }

        return isCanAdded;
    }
```
이렇게 하면 쉽게 좌우 스크롤 할 때마다 해당되는 수업을 불러올 수 있다.

<iframe width="420" height="315" src="{{ site.baseurl }}/assets/develop_diary/calendar.MP4" frameborder="0" allowfullscreen></iframe>
