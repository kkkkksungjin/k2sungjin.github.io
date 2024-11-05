---
layout: default
title: Event using CI
nav_order: 2
parent: github action
grand_parent: CICD
---

## Event

push - コードが特定のブランチにプッシュされたときにトリガー

~~~java
on:
  push:
    branches:
      - main  # mainブランチにプッシュされたときに実行
    tags:
      - 'v*'  # タグが「v」で始まる場合に実行（例：v1.0）
    paths:
      - 'src/**'  # srcフォルダ内のファイルが変更されたときのみ実行
~~~ 

pull_request - 特定のブランチにPRが作成されたときにトリガー

~~~java
on:
  pull_request:
    branches:
      - main  # mainブランチへのPRが作成されたときに実行
    paths:
      - 'docs/**'  # docsフォルダ内の変更を含むPRのみ実行
    types:
      - opened      # PRが作成されたときに実行
      - synchronize # PRが更新されたときに実行
      - reopened    # PRが再オープンされたときに実行
~~~

schedule - スケジュールに従ってトリガー（cron形式）

~~~java
on:
  schedule:
    - cron: '0 0 * * *'  # 毎日午前0時に実行（UTC基準）
    - cron: '30 9 * * 1' # 毎週月曜日午前9時30分に実行
~~~

workflow_dispatch - 手動でワークフローを実行する際にトリガー

~~~java
on:
  workflow_dispatch:
    inputs:
      log_level:
        description: 'ログレベル'
        required: true
        default: 'warning'
      deploy_env:
        description: 'デプロイ環境'
        required: false
        default: 'production'
~~~

repository_dispatch - 外部アプリケーションからリクエストされたときにトリガー

~~~java
on:
  repository_dispatch:
    types:
      - deploy # deployというイベントが発生したときに実行
~~~

issue_comment - イシューにコメントが追加されたときにトリガー

~~~java
on:
  issue_comment:
    types:
      - created  # コメントが作成されたときに実行
      - edited   # コメントが編集されたときに実行
      - deleted  # コメントが削除されたときに実行
~~~

issues - イシューに関するイベントが発生したときにトリガー

~~~java
on:
  issues:
    types:
      - opened    # イシューが作成されたときに実行
      - edited    # イシューが編集されたときに実行
      - closed    # イシューがクローズされたときに実行
      - reopened  # イシューが再オープンされたときに実行
~~~

release - リリースが作成、編集、削除されたときにトリガー

~~~java
on:
  release:
    types:
      - published   # リリースが公開されたときに実行
      - edited      # リリースが編集されたときに実行
      - created     # リリースが作成されたときに実行
      - deleted     # リリースが削除されたときに実行
      - prereleased # プリリリースが公開されたときに実行
~~~

fork - リポジトリがフォークされたときにトリガー

~~~java
on:
  fork:
    types: [created]  # リポジトリがフォークされたときに実行
~~~

delete - ブランチやタグが削除されたときにトリガー

~~~java
on:
  delete:
    branches:
      - 'feature/*'  # featureで始まるブランチが削除されたときに実行
    tags:
      - 'v*'  # 'v'で始まるタグが削除されたときに実行
~~~

create - ブランチやタグが作成されたときにトリガー

~~~java
on:
  create:
    branches:
      - 'release/*'  # releaseで始まるブランチが作成されたときに実行
    tags:
      - 'v*'  # 'v'で始まるタグが作成されたときに実行
~~~

watch - リポジトリがウォッチされたときにトリガー

~~~java
on:
  watch:
    types: [started]  # ユーザーがリポジトリをWatchに設定したときに実行
~~~

member - リポジトリにメンバーが追加されたときにトリガー

~~~java
on:
  member:
    types: [added, edited, removed]  # メンバーが追加、編集、削除されたときに実行
~~~

workflow_run - 他のワークフローが完了したときにトリガー

~~~java
on:
  workflow_run:
    workflows: ["Another Workflow"]
    types:
      - completed  # 指定されたワークフローが完了したときに実行
~~~

workflow_call - 他のワークフローから呼び出されたときにトリガー

~~~java
on:
  workflow_call:
    inputs:
      log_level:
        description: 'ログレベル'
        required: true
        type: string
    secrets:
      MY_SECRET:
        required: true
~~~

{: .fs-6 .fw-300 }
