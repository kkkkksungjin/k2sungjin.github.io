---
layout: default
title: Coverage Tool
nav_order: 2
parent: Quality Management
grand_parent: Performance & Quality Management
---

## Coverage Tool

---
### **カバレッジ測定ツール**

#### **JaCoCo (Java Code Coverage)**
- **用途**: Java/Kotlinコードのカバレッジ分析。  
- **サポートするカバレッジの種類**:  
  - ライン Coverage
  - 分岐 Coverage 
  - メソッド Coverage

#### **Android Studio カバレッジレポート**
- **用途**: Android Studio上で直接カバレッジを確認可能。  
- **確認方法**:  
  - `Run > Run with Coverage`を選択するとカバレッジレポートが生成  
  - クラスやメソッドごとのカバレッジ率がIDEに表示  

<img src="../../../../assets/images/pqm/qm/setting_coverage.png" width="750" height="550"/> 
<br/>
<img src="../../../../assets/images/pqm/qm/android_studio_intellij.png" width="750" height="550"/> 

#### **SonarQube**
- **用途**: 静的コード解析とカバレッジ測定。  
- **特徴**:  
  - プロジェクトダッシュボードでカバレッジ率を確認  
  - 問題点や改善箇所を視覚化  

<img src="../../../../assets/images/pqm/qm/sonar_ide_plugin.png" width="750" height="550"/>
<img src="../../../../assets/images/pqm/qm/sonar_ide_run.png" width="750" height="550"/>

#### **SonarQubeサーバーにCI連動**
- **Docker**からSonarQubeImageをPullする
<img src="../../../../assets/images/pqm/qm/sonarQube_pull.png" width="750" height="550"/>

- SonarQube Server start
- defualt login admin/admin
- Github Action connect

<img src="../../../../assets/images/pqm/qm/sonar_set_1.png" width="750" height="550"/>
<img src="../../../../assets/images/pqm/qm/sonar_set_2.png" width="750" height="550"/>


{: .fs-6 .fw-300 }
