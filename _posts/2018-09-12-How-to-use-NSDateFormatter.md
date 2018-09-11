---
layout: post
title:  "NSDateFormatter를 어떻게 쓸까?(How to use NSDateFormatter?)"
date:   2018-09-12 02:15:00
author: Seungbeom Kim
categories: develop_diary
tags:	아이폰 iOS NSDateFormatter Objective-c
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에 갑자기 아랍에서 local notification이 울리지 않는다는 연락이 엄청오기 시작했다. 그래서 테스트했더니 정말 울리지 않았다. 이유는 무엇이었을까??

예전 코드는 아래와 같다.

    NSString *dateString = [NSString stringWithFormat:@"2013-08-%d %@:00", day, newTime];
    NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
    dateFormatter.locale = [[NSLocale alloc] initWithLocaleIdentifier:[NSString stringWithFormat:@"%@_%@", language, countryCode]];
    dateFormatter.dateFormat = @"yyyy-MM-dd HH:mm:ss";
    dateFormatter.timeZone = [NSTimeZone localTimeZone];

    NSDate *date = [dateFormatter dateFromString:dateString];
    [dateFormatter release];

    noti.fireDate = date;
    noti.timeZone = [NSTimeZone localTimeZone];
    NSString *msg = @"...";

    noti.alertBody = msg;
    noti.repeatInterval = NSCalendarUnitWeekOfYear;
    noti.soundName = UILocalNotificationDefaultSoundName;
    [[UIApplication sharedApplication] scheduleLocalNotification:noti];
    [noti release];

보면 알겠지만 월~금에 해당되는 요일에 정해진 시간마다 7주일 반복으로 알림이 울려야 한다. 하지만 아랍에서는 울리지 않았다.

이유는 아래의 문장때문이다.

    dateFormatter.locale = [[NSLocale alloc] initWithLocaleIdentifier:[NSString stringWithFormat:@"%@_%@", language, countryCode]];

이 문장을 보면 언어가 한국어, 지역이 미국이면 ko_EN, 지역이 미국이면 ko_KR과 같은 identifier를 주는 것이다. 그런데 아랍은 ko_AR인데 이렇게 해버리면 마땅한 locale이 없었던 것이다. 그래서 알림이 울려야 할 제대로 된 시간을 표현하지 못했던 것이다.

그러면 어떻게 해야할까? 고민했다. 답은 아래와 같다.

    NSString *dateString = [NSString stringWithFormat:@"2013-08-%d %@:00", day, newTime];
    NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
    dateFormatter.dateFormat = @"yyyy-MM-dd HH:mm:ss";

    NSDate *date = [dateFormatter dateFromString:dateString];
    [dateFormatter release];

    noti.fireDate = date;
    noti.timeZone = [NSTimeZone localTimeZone];
    NSString *msg = @"...";

    noti.alertBody = msg;
    noti.repeatInterval = NSCalendarUnitWeekOfYear;
    noti.soundName = UILocalNotificationDefaultSoundName;
    [[UIApplication sharedApplication] scheduleLocalNotification:noti];
    [noti release];

즉, locale과 timezone을 쓰지 않는 것이다. NSDateFormatter에서 locale과 timezone을 사용하면 기본적으로 현재 시스템의 locale과 시간을 따라서 알아서 변환해 준다. 즉, 지역이 아랍이면 어차피 저 시간은 아랍시간으로 표현되기에 아무런 문제가 없는 것이다. 즉, 내가 알림을 받아야할 요일이 월요일 17시라면, 그냥 알아서 2013-08-14 17:00이 되는 것이다. 그렇기 때문에 이를 7일마다 반복하면 결국 2018-09-12 17:00에 알림을 받을 수 있다.
