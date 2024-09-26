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
      <th style="text-align: center">環境</th>
      <th style="text-align: center">説明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left">言語</td>
      <td style="text-align: left">kotlin</td>
    </tr>
    <tr>
      <td style="text-align: left">作業日</td>
      <td style="text-align: left">7days</td>
    </tr>
    <tr>
      <td style="text-align: left">サービス</td>
      <td style="text-align: left">天気予報</td>
    </tr>
    <tr>
      <td style="text-align: left">詳細機能</td>
      <td style="text-align: left">一日天気予報、６時間・12時間ずつ更新機能</td>
    </tr>
    <tr>
      <td style="text-align: left">設定</td>
      <td style="text-align: left">mvvm, hilt, compose, retrofit</td>
    </tr>
  </tbody>
</table>

<table rules="groups">
  <thead>
    <tr>
      <th style="text-align: center">環境</th>
      <th style="text-align: center">説明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: center">画面</td>
      <td style="text-align: left">
      <img src="../../../../assets/images/compose/weather/permission_screen.png" alt="1" width="200" height="350">
      <img src="../../../../assets/images/compose/weather/main_screen.png" alt="1" width="200" height="350">
      </td>
    </tr>
  </tbody>
</table>

<!-- 
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
-->


Weather API Server
~~~ java
weatherapi.com
~~~ 

無料プランだと、時間代の天気情報が来ない問題がある


<br/>
