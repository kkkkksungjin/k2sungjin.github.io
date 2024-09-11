---
layout: default
title: Flavor Setting
nav_order: 1
parent: flutter
grand_parent: cross-platform
---

# Flavor Setting
<hr/>

Flutter Build List
<br/>
![](../../../../assets/images/flutter/flutter_builds.png)

Flutter Edit Configurations
<br/>
![](../../../../assets/images/flutter/flutter_edit_configurations.png)
<br/>

Flutter Directory Add
<br/>
![](../../../../assets/images/flutter/flutter_directory_tree.png)


<br/>
<br/>
<hr/>

Android app.gradle
~~~ java
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
~~~ 

<br/>
<br/>
<hr/>

Ios setting

Runner Info Add
![](../../../../assets/images/flutter/runner_info_add.png)

Build List
<br/>
![](../../../../assets/images/flutter/ios_schema_manager.png)
<br/>

Debug set
<br/>
![](../../../../assets/images/flutter/ios_dev_run_set.png)
![](../../../../assets/images/flutter/ios_dev_test_set.png)
![](../../../../assets/images/flutter/ios_dev_profile_set.png)
![](../../../../assets/images/flutter/ios_dev_analyze_set.png)
![](../../../../assets/images/flutter/ios_dev_archive_set.png)

<br/>
<br/>
Release set
<br/>
![](../../../../assets/images/flutter/ios_prod_run_set.png)
![](../../../../assets/images/flutter/ios_prod_test_set.png)
![](../../../../assets/images/flutter/ios_prod_profile_set.png)
![](../../../../assets/images/flutter/ios_prod_analyze_set.png)
![](../../../../assets/images/flutter/ios_prod_archive_set.png)

Runner infomation change data
![](../../../../assets/images/flutter/ios_dev_runner_detail.png)

<br/>
<br/>

{: .fs-6 .fw-300 }
