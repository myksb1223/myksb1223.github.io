---
layout: post
title:  "아마존 SES와 Rails를 사용시 주의사항(Amazon SES with Rails error)."
date:   2018-09-28 01:33:00
author: Seungbeom Kim
categories: develop_diary
tags:	아마존 Amazon SES 레일즈 Rails
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)은 Amazon SES 서비스를 사용하는데, 갑자기 서버에 `UserMailer#Action processed outbound mail` 과 같은 에러가 발생했다.

이유를 알고보니 아래와 같다. [링크](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/smtp-connect.html)

    Amazon Elastic Compute Cloud (Amazon EC2) throttles email traffic over port 25 by default. To avoid timeouts when sending email through the SMTP endpoint from EC2, use a different port (587 or 2587) or fill out a Request to Remove Email Sending Limitations to remove the throttle.

즉, 25번 포트가 아닌 587이나 2587을 사용하는 것이 트래픽에 관계없이 보낼 수 있다.

다행히 사용자에게 연락이 와서 겨우 찾았다!
