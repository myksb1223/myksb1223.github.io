---
layout: post
title:  "전 세계 숫자 형식 지원하는 방법은?(How to support the whole world number format)."
date:   2018-09-27 02:48:00
author: Seungbeom Kim
categories: develop_diary
tags:	아이폰 iOS 안드로이드 Android 숫자형식 NumberFormat
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에서 아랍언어를 지원하면서 몇 가지 고민할 부분이 생겼다.

날짜와 시간, 요일 같은 경우는 자동적으로 바꿀 수 있다지만... 따로 사용한 0-9 사의 숫자는 어떻게 해당 국가의 숫자 형식으로 변경해 줄 수 있을까? 였다.

각각 다행히 쉬운 방법이 있었다.

Objective-c는 아래와 같다.

```Objective-c
+ (NSString *)convertCountryNumber:(NSString *)str {
    NSNumberFormatter *formatter = [[NSNumberFormatter alloc] init];
    NSLocale *locale = [NSLocale localeWithLocaleIdentifier:[[NSLocale preferredLanguages] objectAtIndex:0]];
    [formatter setLocale:locale];
    for (NSInteger i = 0; i < 10; i++) {
        NSNumber *num = @(i);
        str = [str stringByReplacingOccurrencesOfString:num.stringValue withString:[formatter stringFromNumber:num]];
    }

    return str;
}
```


```Java
public static String convertToLocale(String value)  {
      String[] numberFormats = new String[10];
      NumberFormat nf = NumberFormat.getInstance();
      for (int i = 0; i < 10; ++i) {
          numberFormats[i] = nf.format(i);
      }

      String newValue = (((((((((((value)
                .replaceAll("1", numberFormats[1])).replaceAll("2", numberFormats[2]))
                .replaceAll("3", numberFormats[3])).replaceAll("4", numberFormats[4]))
                .replaceAll("5", numberFormats[5])).replaceAll("6", numberFormats[6]))
                .replaceAll("7", numberFormats[7])).replaceAll("8", numberFormats[8]))
                .replaceAll("9", numberFormats[9])).replaceAll("0", numberFormats[0]));
      return newValue;
}
```

위의 코드 둘 다 0-9의 값을 해당 지역의 값을 NumberFormat으로 구한 후 변경해주는 방법이다.

그러면 모든 텍스트 및 숫자 관련 데이터를 해당 로컬 지역으로 변경시켜 표현시켜 줄 수 있다.
