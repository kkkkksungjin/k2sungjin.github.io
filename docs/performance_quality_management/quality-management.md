---
layout: default
title: Quality Management
nav_order: 2
parent: Performance & Quality Management
has_children: true
---

# Quality Management

<hr/>

## __自動テストの導入__{: style="color: #00c0ef"}

### __単体テスト（Unit Test）__{: style="color: #e26716"}
- 目的: 個々の関数やメソッドが正しく動作することを検証。
- ツール: JUnit、Mockito

### __統合テスト（Integration Test）__{: style="color: #e26716"}
- 目的: 複数のモジュールが連携して正しく動作することを検証。
- ツール: Hilt Test、TestContainers。

### __UIテスト（UI Test）__{: style="color: #e26716"}
- 目的: ユーザーインターフェースの操作やフローを検証。
- ツール: Espresso

### __パフォーマンステスト（Performance Test）__{: style="color: #e26716"}
- 目的: ロード時間、応答速度、リソース使用量を評価。
- ツール: Firebase Performance Monitoring、Android Profiler。

### __回帰テスト（Regression Test）__{: style="color: #e26716"}
- 目的: 新しいコードが既存の機能に影響を与えていないかを確認。

---

## __CI/CDパイプラインの構築__{: style="color: #00c0ef"}
- 継続的インテグレーション（CI）: コード変更ごとに自動でビルドとテストを実行。
- 継続的デリバリー（CD）: テストを通過したコードを自動でデプロイ。
- ツール:Jenkins、GitHub Actions、GitLab CI、CircleCI。

---


## __コード品質分析ツールの使用__{: style="color: #00c0ef"}

### __Lintツール__{: style="color: #e26716"}
- コードスタイル、バグ、セキュリティ脆弱性を検出。
- Android: Android Lint

### __静的解析ツール__{: style="color: #e26716"}
- コード品質とセキュリティを分析。
- ツール: SonarQube

---


## __ユーザーフィードバックの収集と分析__{: style="color: #00c0ef"}
- Crashlytics: Firebaseが提供するクラッシュレポート収集ツール。
- Instabug: ユーザーフィードバックとバグレポート収集ツール。
- アプリレビュー分析: ユーザーレビューをモニタリングし、改善点を特定。

---


## __セキュリティテストと脆弱性診断__{: style="color: #00c0ef"}

{: .fs-6 .fw-300 }
