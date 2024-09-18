---
layout: default
title: WeatherApp test
parent: android
grand_parent: play-labs
nav_order: 7
---


<table rules="groups">
  <thead>
    <tr>
      <th style="text-align: center">환경</th>
      <th style="text-align: center">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left">Server Develop / Release </td>
      <td style="text-align: left">MAMP / Amazon EC2</td>
    </tr>
    <tr>
      <td style="text-align: left">language</td>
      <td style="text-align: left">kotlin</td>
    </tr>
    <tr>
      <td style="text-align: left">작업일</td>
      <td style="text-align: left">7days</td>
    </tr>
    <tr>
      <td style="text-align: left">서비스</td>
      <td style="text-align: left">날씨 정보</td>
    </tr>
    <tr>
      <td style="text-align: left">상세 기능</td>
      <td style="text-align: left">일일 날씨 정보</td>
    </tr>
  </tbody>
</table>

<table rules="groups">
  <thead>
    <tr>
      <th style="text-align: center">환경</th>
      <th style="text-align: center">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: center">화면</td>
      <td style="text-align: left">
      <img src="../../../../assets/images/compose/weather/permission_screen.png" alt="1" width="200" height="350">
      <img src="../../../../assets/images/compose/weather/main_screen.png" alt="1" width="200" height="350">
      </td>
    </tr>
  </tbody>
</table>
~~~ java
//Add lib
#test
junit = "4.13.2"
junitVersion = "1.2.1"
espressoCore = "3.6.1"
mockito-core = "3.11.2"
kotlinx-coroutines-test = "1.5.2"
mockito-kotlin = "4.0.0"

#compose
lifecycleRuntimeKtx = "2.8.5"
activityCompose = "1.9.2"
composeBom = "2024.04.01"

#hilt
hilt = "2.44"
hilt-navigation = "1.2.0"

#other
play-services-location = "21.0.1"
coil-compose = "1.4.0"
retrofit = "2.9.0"

#logger
timber= "5.0.1"

[libraries]
...

[plugins]
...
~~~


Weather API
~~~ java
weatherapi.com
~~~ 
무료 계정이면 시간단위의 날씨 정보는 들어오지 않는다.

<br/>
