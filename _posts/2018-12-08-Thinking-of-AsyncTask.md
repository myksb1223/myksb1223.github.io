---
layout: post
title:  "AsyncTask에 대한 고찰(Thinking of AsyncTask)."
date:   2018-12-08 04:36:00
author: Seungbeom Kim
categories: develop_diary
tags:	안드로이드 Android AsyncTask
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에서 HttpUrlConnection을 사용할 때는 보통 AsyncTask나 Thread 중에서 그 때 그 때 내키는 것을 이용해서 구현하였다.

나는 AsyncTask를 사용할 때면 여러 AsyncTask 객체가 있을 경우 그리고 그 객체들이 같은 것을 수행할 경우, 그리고 무엇인가에 의해 수행이 멈춰있는 경우 등의 여러가지 상황을 생각하게 된다. 그래서 이를 관리하기 위한 Class 따로 생성하여 관리하게끔 구현하였다.

간단하게 이야기하면 Queue라는 것에 집어넣으면서, 먼저 들어온 AsyncTask 객체가 뒤에 들어온 객체와 같은 수행인지 아닌지 등의 판단 기준에 따라서 앞에 객체를 스킵하는 등의 작업을 하도록 구현한 것이다.

AsyncTask와 관련된 수행중 [HttpUrlConnection에서 원인불명의 통신이 끊기지 않는 경우](https://myksb1223.github.io/develop_diary/2018/12/08/2018-12-08-Timeout-in-HttpUrlConnection.html)가 발생하였는데, 이로 인해 후에 수행되는 AsyncTask들이 아예 수행자체가 되지 않았다.

지금에야 원인을 알았지만 처음에는 나의 Queue 관련 작업이 잘못되었다고 판단하였다. 그래서 여러가지 알아본 결과 여러 AsyncTask를 사용할 때는 `executeOnExecutor(AsyncTask.THREAD_POOL_EXECUTOR)` 를 사용하라고 되어 있었다. 해당 메소드를 사용하면 병렬 작업이 가능하다고 한다.

또한 AsyncTask를 사용할 때 Thread의 갯수에 제한이 있다는 것이다. 정확히 얼마나 되는지 구하는 법은 찾아보지 않았지만 제한이 있다는 것은 확실한 것 같다.

이 두가지를 더 알게되어 AsyncTask를 사용할 때 조금 더 주의하며 개발할 수 있을 것 같다.
