---
layout: post
title:  "안드로이드 알람과 잠자기 모드 및 배터리 최적화.(AlarmManager with Doze mode and Battery Optimization in Android)."
date:   2018-12-27 02:30:00
author: Seungbeom Kim
categories: develop_diary
tags:	안드로이드 Android 알람 Alarm AlarmManager
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에서 수업 및 노트 알림을 위해서 AlarmManager를 이용하였는데 잠자기(Doze) 모드 및 Battery Optimization이 설정되어 있으면 울리지 않는 문제가 있다.

일단, 알람은 잠자기(Doze) 모드에 들어가면 어떻게 설정했는가에 따라 울리지 않는 경우가 있다.

잠자기 모드에 들어가면...

- 네트워크 접근이 정지된다.
- 시스템은 wake locks를 무시한다.
- AlarmManager의 알람(`setExact()` 또는 `setWindow()`)은 다음 maintenance window로 지연된다.
  - `setAndAllowWhileIdle()` 또는 `setExactAndAllowWhileIdle()`를 사용하면 잠자기 모드에서도 알림을 울릴 수 있다.
  - `setAlarmClock()`을 사용하면 잠자기 모드와 상관없이 알람이 울린다.
- Wi-Fi 스캔을 하지 않는다.
- sync adapters의 작동을 허가하지 않는다.
- JobScheduler의 작동을 허가하지 않는다.

위와 같은 제약조건이 있다.

위의 제약조건과 함께 Battery Optimization을 켜놓고 테스트 해보았다.

`setExact()`를 사용하는 대신에 `setExactAndAllowWhileIdle()`를 사용했지만 앱에서 알람이 울린 후, 5분내로 다시 알람이 울려야 하면 울리지 않는다. 약 10분 간격으로 알람을 설정해야 울리는 것 같다. (클래스업(ClassUp)은 시간 단위가 5분 간격이기 때문에 테스트는 최저 5분, 10분 이렇게 테스트를 했다.)

그래서 마지막으로 'setAlarmClock()'도 써보았지만 정확한 시간에 울리지 않았다. 제대로 울리지 않았다. 혹시 지연이 있을 수도 있는데... 10분이 지나도 알람은 울리지 않았다.

일단 배터리 최적화를 켜놓은 상태에서는 알람이 정상적으로 작동하지 않는 것으로 생각된다.

`ACTION_REQUEST_IGNORE_BATTERY_OPTIMIZATIONS`을 이용하여 앱을 배터리 최적화에 포함시키지 않도록 사용할 수 있지만 이는 권장하는 방법이 아니기에 사용하지는 않았다.

어쨌든 결론은
1. 아예 ACTION_REQUEST_IGNORE_BATTERY_OPTIMIZATIONS을 사용하거나,
2. `setExactAndAllowWhileIdle()`을 사용하고 짧은 단위의 알람 반복은 포기하는 정도로 생각해 볼 수 있을 것 같다.
