---
layout: default
title: Building a CMS
nav_order: 1
parent: web-nodejs
grand_parent: play-labs
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
      <td style="text-align: left">Server Develop / Release </td>
      <td style="text-align: left">MAMP / Amazon EC2</td>
    </tr>
    <tr>
      <td style="text-align: left">language</td>
      <td style="text-align: left">javascript</td>
    </tr>
    <tr>
      <td style="text-align: left">作業日</td>
      <td style="text-align: left">1m 14days</td>
    </tr>
    <tr>
      <td style="text-align: left">サービス</td>
      <td style="text-align: left">店舗計算システム</td>
    </tr>
    <tr>
      <td style="text-align: left">詳細機能</td>
      <td style="text-align: left">新規会員, 会員管理, データ管理（アップデート, Excelダウンロード, データ変更、）, 精算合計の導出</td>
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
      <td style="text-align: center">ログイン画面</td>
      <td style="text-align: left"><img src="../../../../assets/images/web-node/login_page.png" alt="1" width="500" height="600"></td>
    </tr>
    <tr>
      <td style="text-align: center">データ登録画面</td>
      <td style="text-align: left"><img src="../../../../assets/images/web-node/data_update.png" alt="1" width="500" height="600"></td>
    </tr>
    <tr>
      <td style="text-align: center">データ管理画面</td>
      <td style="text-align: left"><img src="../../../../assets/images/web-node/data_list_page.png" alt="1" width="500" height="600"></td>
    </tr>
  </tbody>
</table>


~~~ java
//dependencies
    "async": "2.6.0",
    "body-parser": "~1.12.0",
    "cookie-parser": "~1.3.4",
    "cookie-session": "2.0.0-beta.3",
    "ejs": "~2.5.9",
    "excel": "^1.0.0",
    "express": "~4.16.3",
    "moment": "~2.22.1",
    "mysql": "~2.15.0",
    "xlsx": "^0.12.12"
~~~ 
<br/>


{: .fs-6 .fw-300 }
