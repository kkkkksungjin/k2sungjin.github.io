---
layout: default
title: Hello Rx
parent: Android
grand_parent: Google
nav_order: 1
permalink: /Google/Android/Rx
---

# Rx [http://reactivex.io](http://reactivex.io)

### RxJava 나온 배경?
> 자바는 동시성 처리를 하는데 번거로움이 많기 때문에 이를 해결하기 위해 넷플릭스에서 RxJava를 개발, 클라이언트 요청을 
> 처리할 때 다수의 비동기 실행 흐름(스레드 등)을 생성하고 그것의 결과를 취합하여 최종 리턴하는 방식으로 내부방식에서 변경할수 있도록 개발 

### RxJava 장점?
1. 동시성을 적극적으로 끌어안을 필요가 있다 (Embrace Concurrency)
2. 자바 Future를 조합하기 어렵다는 점을 해결한다. (Java Futures are Expensive to Compose.)
3. 콜백 방식의 문제점을 개선해야 한다 (Callback Have Their Own Problems.)

##### RxJava compile : 'io.reactivex.' + rxjava2:rxjava:2.x

<table rules="groups">
  <thead>
    <tr>
      <th style="text-align: left">리액티브 API</th>
      <th style="text-align: center">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left">RxLifecycle</td>
      <td style="text-align: left">RxJava를 사용하는 안드로이드 앱용 라이프 사이클 처리 API로서 
                   메모리 누수 관리에 사용할수 있다.</td>
    </tr>
  </tbody>

</table>