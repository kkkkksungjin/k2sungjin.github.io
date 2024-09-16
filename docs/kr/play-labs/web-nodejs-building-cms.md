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
      <td style="text-align: left">javascript</td>
    </tr>
    <tr>
      <td style="text-align: left">작업일</td>
      <td style="text-align: left">1m 14days</td>
    </tr>
    <tr>
      <td style="text-align: left">서비스</td>
      <td style="text-align: left">점포 정산 시스템</td>
    </tr>
    <tr>
      <td style="text-align: left">상세 기능</td>
      <td style="text-align: left">회원가입, 회원관리, 데이터 업로드, 데이터 다운로드, 정산 태그 추가, 정산 합계 도출</td>
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
      <td style="text-align: center">로그인</td>
      <td style="text-align: left"><img src="../../../../assets/images/web-node/login_page.png" alt="1" width="500" height="600"></td>
    </tr>
    <tr>
      <td style="text-align: center">데이터 등록</td>
      <td style="text-align: left"><img src="../../../../assets/images/web-node/data_update.png" alt="1" width="500" height="600"></td>
    </tr>
    <tr>
      <td style="text-align: center">데이터 확인</td>
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
