---
layout: default
title: Understanding GitHub Actions
nav_order: 1
parent: github action
grand_parent: CICD
---
**基本順番**：Workflow > Job > Step > Action 

## 用語
__Workflow（ワークフロー）__{: style="color: #00c0ef"}
自動化された一連の作業。.ymlファイルで定義され、イベント発生時に実行されます。

__Job（ジョブ）__{: style="color: #00c0ef"}
ワークフロー内の実行単位で、複数のStepで構成されます。

__Step（ステップ）__{: style="color: #00c0ef"}
Job内の各作業。コードのチェックアウトやビルドなどを含みます。

__Action（アクション）__{: style="color: #00c0ef"}
Step内で使用される再利用可能な作業単位。GitHubが公式アクションを提供

__Runner（ランナー）__{: style="color: #00c0ef"}
Jobを実行する環境。GitHubのホストランナーやセルフホストランナーを利用可能。

__Event（イベント）__{: style="color: #00c0ef"}
ワークフローが実行されるトリガー条件。例：pushやpull_request。

__YAML（YAMLファイル）__{: style="color: #00c0ef"}
ワークフローファイルの記述形式（.yml）

__Secret（シークレット変数）__{: style="color: #00c0ef"}
機密データを安全に管理する変数。例：APIキー。

__Environment（環境）__{: style="color: #00c0ef"}
開発や本番環境ごとに設定できる実行環境。

__Matrix（マトリックス）__{: style="color: #00c0ef"}
複数の設定を組み合わせ、並行して複数のJobを実行する機能。

~~~java
//yaml
strategy:
  matrix:
    os: [ubuntu-latest, macos-latest, windows-latest]
    node: [12, 14, 16]
~~~

__Artifact（アーティファクト）__{: style="color: #00c0ef"}
Artifactは、ワークフローで生成されたファイルやビルド成果物などの結果ファイルを保存・共有する機能です。後続のStepや他のJobでこのファイルを使用できます。例：ビルド成果物をアーティファクトとして保存し、後続のJobでダウンロードして使用できます。

__Cache（キャッシュ）__{: style="color: #00c0ef"}
Cacheは、ワークフローの実行時に依存関係ファイルやビルド成果物をキャッシュし、次回の実行時にビルド時間を短縮するための機能です。主にパッケージ管理ツールで使用されます。例：Node.jsのnode_modulesフォルダをキャッシュしてビルド時間を短縮します。

~~~java
name: CI/CD Pipeline Example

# 6. Event - ワークフローが実行されるトリガー
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
  schedule:
    - cron: "0 0 * * *" # 毎日午前0時に実行

# 1. Workflow - CI/CDパイプラインの定義
jobs:
  # 2. Job - ビルドとテストの作業定義
  build-and-test:
    # 5. Runner - GitHubが提供するUbuntu環境を使用
    runs-on: ubuntu-latest

    # 10. Matrix - 複数のNode.jsバージョンとOSでのテスト
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest]
        node: [14, 16]

    steps:
      # 3. Step - コードのチェックアウト
      - name: Checkout code
        uses: actions/checkout@v2

      # Node.jsの設定とキャッシュ管理
      - name: Set up Node.js ${{ matrix.node }}
        uses: actions/setup-node@v2
        with:
          node-version: ${{ matrix.node }}

      # 12. Cache - node_modulesキャッシュ
      - name: Cache node modules
        uses: actions/cache@v2
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ matrix.node }}-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-${{ matrix.node }}-

      # 依存関係のインストール
      - name: Install dependencies
        run: npm install

      # テストの実行
      - name: Run tests
        run: npm test

      # ビルドの実行
      - name: Run build
        run: npm run build

      # 11. Artifact - ビルド成果物の保存
      - name: Upload build artifacts
        uses: actions/upload-artifact@v2
        with:
          name: build
          path: build/

  # 2. Job - デプロイ作業の定義
  deploy:
    # 9. Environment - デプロイ環境の設定（例：production）
    environment: production
    runs-on: ubuntu-latest
    needs: build-and-test # build-and-test作業が完了後に実行

    steps:
      # 3. Step - コードのチェックアウト
      - name: Checkout code
        uses: actions/checkout@v2

      # 8. Secret - デプロイに必要なシークレット環境変数
      - name: Set up environment variables
        env:
          DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}

      # 7. YAML - デプロイ作業で生成されたアーティファクトのダウンロードとデプロイ
      - name: Download build artifacts
        uses: actions/download-artifact@v2
        with:
          name: build
          path: ./build

      # デプロイの実行（例：S3にアップロードやサーバーへの転送）
      - name: Deploy to production
        run: |
          echo "Deploying to production with DEPLOY_KEY=${{ secrets.DEPLOY_KEY }}..."
          # デプロイスクリプトをここに追加（例：AWS CLIの使用など）

~~~

1. Workflow: .ymlファイルで構成されたパイプライン定義。
2. Job: build-and-testとdeployの2つの作業を含む。
3. Step: コードのチェックアウト、Node.jsの設定、テスト、ビルド、デプロイの各段階を定義。
4. Action: GitHubが提供する基本的なアクションを使用（actions/checkout、actions/cacheなど）。
5. Runner: GitHubが提供するubuntu-latest環境で実行。
6. Event: コードのプッシュ、PR、スケジュール条件でパイプラインを実行。
7. YAML: .ymlファイル形式で記述。
8. Secret: DEPLOY_KEYというシークレットを使用してデプロイのセキュリティを保持。
9. Environment: デプロイ環境をproductionに設定。
10. Matrix: 複数のNode.jsバージョンとOSで同時にテスト。
11. Artifact: ビルド結果を保存し、次の段階で使用。
12. Cache: node_modulesをキャッシュしてビルド時間を短縮。

{: .fs-6 .fw-300 }
