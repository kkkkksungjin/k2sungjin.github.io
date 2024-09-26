---
layout: default
title: Flavor Setting
nav_order: 1
parent: flutter
grand_parent: Google
---

# Flavor Setting
<hr/>


<table rules="groups">
  <thead>
    <tr>
      <th style="text-align: center">環境</th>
      <th style="text-align: center" colspan="3">説明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left">flutter</td>
      <td style="text-align: left">
        <img src="../../../../assets/images/flutter/flutter_builds.png" alt="1" width="150" height="350">
        <img src="../../../../assets/images/flutter/flutter_directory_tree.png" alt="1" width="150" height="350">
        <br>
        <img src="../../../../assets/images/flutter/flutter_edit_configurations.png" alt="1" width="600" height="350">
      </td>
    </tr>
    <tr>
      <td style="text-align: left">Android</td>
      <td style="text-align: left">
      <pre>
flavorDimensions "default"
productFlavors{
    prod{
        dimension "default"
        resValue "string", "app_name", "xxxx"
        versionName "-1.0"
    }
    dev{
        dimension "default"
        resValue "string", "app_name", "[DEV]xxxx"
        applicationIdSuffix ".dev"
        versionName "-1.0-dev"
    }
}
        </pre>
      </td>
    </tr>
    <tr>
      <td style="text-align: left">Ios</td>
      <td style="text-align: left">

<img src="../../../../assets/images/flutter/runner_info_add.png" alt="1" width="600" height="500">
<br/>
<img src="../../../../assets/images/flutter/ios_schema_manager.png" alt="1" width="300" height="350">
<br/>
<br/>
<img src="../../../../assets/images/flutter/ios_dev_run_set.png" alt="1" width="600" height="500">
<img src="../../../../assets/images/flutter/ios_dev_test_set.png" alt="1" width="600" height="500">
<img src="../../../../assets/images/flutter/ios_dev_profile_set.png" alt="1" width="600" height="500">
<img src="../../../../assets/images/flutter/ios_dev_analyze_set.png" alt="1" width="600" height="500">
<img src="../../../../assets/images/flutter/ios_dev_archive_set.png" alt="1" width="600" height="500">
<br/>
<br/>
<br/>
<img src="../../../../assets/images/flutter/ios_prod_run_set.png" alt="1" width="600" height="500">
<img src="../../../../assets/images/flutter/ios_prod_test_set.png" alt="1" width="600" height="500">
<img src="../../../../assets/images/flutter/ios_prod_profile_set.png" alt="1" width="600" height="500">
<img src="../../../../assets/images/flutter/ios_prod_analyze_set.png" alt="1" width="600" height="500">
<img src="../../../../assets/images/flutter/ios_prod_archive_set.png" alt="1" width="600" height="500">

<img src="../../../../assets/images/flutter/ios_dev_runner_detail.png" alt="1" width="600" height="500">      
 </td>
    </tr>
  </tbody>
</table>

<br/>
<br/>

{: .fs-6 .fw-300 }
