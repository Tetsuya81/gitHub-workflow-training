# GitHub実践ガイド：OJTマニュアル

**免責事項**: このプロジェクトは、初心者がGithubを使った開発ワークフローを学ぶために作成したものです。間違ってたらごめんね。

## 目次

1. [はじめに](#はじめに)
2. [プロジェクトセットアップ](#プロジェクトセットアップ)
3. [Issue管理](#issue管理)
4. [ブランチ戦略](#ブランチ戦略)
5. [Pull Request](#pull-request)
6. [GitHub Actions](#github-actions)
7. [実践演習](#実践演習)
8. [トラブルシューティング](#トラブルシューティング)

## はじめに

このマニュアルは、GitHubを使った開発ワークフローを実践的に学ぶためのガイドです。シンプルなTODOアプリケーションを開発しながら、GitHub、Issue、GitHub Actionsの基本的な使い方とベストプラクティスを学びます。

### 前提条件

- Gitの基本的な知識（commit、push、pull）
- GitHubアカウント
- 基本的なプログラミング知識（HTML、CSS、JavaScript）

### 学習目標

- GitHubのIssue管理
- ブランチ戦略の実践
- Pull Requestによるコードレビュー
- GitHub Actionsによる自動化

## プロジェクトセットアップ

### リポジトリの作成

1. GitHubにログインし、新しいリポジトリを作成します
   - リポジトリ名: `github-workflow-training`
   - 説明: `GitHubワークフローを学ぶためのトレーニングプロジェクト`
   - 可視性: Public
   - READMEファイルの初期化: チェックを入れる
   - .gitignoreの追加: Node
   - ライセンス: MIT License

2. リポジトリをローカルにクローンします
   ```bash
   git clone https://github.com/あなたのユーザー名/github-workflow-training.git
   cd github-workflow-training
   ```

### プロジェクト構造の初期化

次のファイル構造で初期プロジェクトを作成します：

```
github-workflow-training/
├── .github/
│   ├── workflows/
│   │   ├── ci.yml
│   │   └── release.yml
│   └── ISSUE_TEMPLATE/
│       ├── bug_report.md
│       └── feature_request.md
├── src/
│   ├── index.html
│   ├── style.css
│   └── app.js
├── tests/
│   └── app.test.js
├── .gitignore
├── LICENSE
├── README.md
└── package.json
```

### 基本的なウェブアプリの設定

1. `package.json` ファイルを作成し、基本的な依存関係を設定します

```json
{
  "name": "github-workflow-training",
  "version": "0.1.0",
  "description": "GitHubワークフローを学ぶためのトレーニングプロジェクト",
  "main": "src/app.js",
  "scripts": {
    "start": "serve src",
    "lint": "eslint src",
    "test": "jest"
  },
  "keywords": [
    "github",
    "workflow",
    "training"
  ],
  "author": "Your Name",
  "license": "MIT",
  "devDependencies": {
    "eslint": "^8.42.0",
    "jest": "^29.5.0",
    "serve": "^14.2.0"
  }
}
```

2. 必要なパッケージをインストールします

```bash
npm install
```

## Issue管理

GitHubのIssueは、機能要求やバグ報告、タスク管理など、プロジェクトの作業を追跡するための機能です。

### Issueテンプレートの作成

`.github/ISSUE_TEMPLATE/` ディレクトリに次の2つのテンプレートファイルを作成します：

#### バグ報告テンプレート (bug_report.md)

```markdown
---
name: バグ報告
about: アプリケーションの問題を報告
title: '[BUG] '
labels: bug
assignees: ''
---

## バグの説明
明確かつ簡潔にバグを説明してください。

## 再現手順
バグを再現する手順:
1. '...' に移動
2. '....' をクリック
3. '....' までスクロール
4. エラーを確認

## 期待される動作
何が起こることを期待していたのかを説明してください。

## スクリーンショット
可能であれば、問題を説明するためのスクリーンショットを追加してください。

## 環境
 - OS: [例: iOS, Windows]
 - ブラウザ: [例: chrome, safari]
 - バージョン: [例: 22]

## 追加情報
他に問題に関する情報があれば、ここに追加してください。
```

#### 機能リクエストテンプレート (feature_request.md)

```markdown
---
name: 機能リクエスト
about: プロジェクトへの新機能の提案
title: '[FEATURE] '
labels: enhancement
assignees: ''
---

## 関連する問題
この機能リクエストが関連する問題の簡単な説明。例：フラストレーションを感じます [...]

## 解決策の提案
実現したいことの明確で簡潔な説明。

## 検討した代替案
検討した他の解決策や機能の明確で簡潔な説明。

## 追加情報
機能リクエストに関するその他の情報やスクリーンショットをここに追加してください。
```

### ラベルとマイルストーン

効果的なIssue管理のために、以下のラベルを作成します：

1. `bug`: バグや問題
2. `enhancement`: 新機能や改善
3. `documentation`: ドキュメントに関する作業
4. `good first issue`: 初心者向けの簡単なタスク
5. `help wanted`: 協力を求めるIssue

マイルストーンは以下のように設定します：

1. `v0.1.0`: 最初の機能リリース
2. `v0.2.0`: 機能強化

### Issue作成のベストプラクティス

1. **明確なタイトル**: Issue内容を端的に表すタイトルをつける
2. **詳細な説明**: 問題や機能を詳細に説明し、必要に応じてスクリーンショットを添付
3. **再現手順**: バグの場合は再現手順を詳しく記載
4. **ラベル付け**: 適切なラベルを付ける
5. **担当者の割り当て**: 必要に応じて担当者を割り当てる
6. **マイルストーンの設定**: リリース計画に基づいてマイルストーンを設定する

## ブランチ戦略

GitFlow に基づいたブランチ戦略を採用します：

### ブランチの種類

1. **main**: 本番環境のコード（安定版）
2. **develop**: 開発用のメインブランチ
3. **feature/**: 新機能開発用ブランチ
4. **bugfix/**: バグ修正用ブランチ
5. **release/**: リリース準備用ブランチ
6. **hotfix/**: 緊急バグ修正用ブランチ

### ブランチ命名規則

- feature/issue-番号-機能名
  例: `feature/issue-12-add-login-function`
- bugfix/issue-番号-バグ内容
  例: `bugfix/issue-15-fix-layout-issue`
- release/バージョン番号
  例: `release/v0.1.0`
- hotfix/issue-番号-修正内容
  例: `hotfix/issue-20-fix-critical-error`

### ブランチ操作の基本コマンド

新しい機能ブランチを作成:
```bash
git checkout develop
git pull origin develop
git checkout -b feature/issue-番号-機能名
```

変更をコミット:
```bash
git add .
git commit -m "feat: 機能の実装"
```

リモートリポジトリにプッシュ:
```bash
git push origin feature/issue-番号-機能名
```

## Pull Request

Pull Request（PR）は、変更をレビューし、メインブランチにマージするためのGitHubの機能です。

### PRテンプレートの作成

`.github/pull_request_template.md` ファイルを作成します：

```markdown
## 変更の概要
変更内容の簡単な説明

## 関連Issue
fixes #(issue番号)

## 変更の種類
- [ ] バグ修正
- [ ] 新機能
- [ ] 破壊的変更
- [ ] ドキュメント更新
- [ ] パフォーマンス改善
- [ ] その他

## チェックリスト
- [ ] テストを追加/修正しました
- [ ] ドキュメントを更新しました
- [ ] コードスタイルのガイドラインに従っています

## スクリーンショット（UI変更の場合）
 
## 追加情報
```

### PRのベストプラクティス

1. **小さく保つ**: 1つのPRに多くの変更を詰め込まない
2. **説明的なタイトル**: PRの内容を明確に表すタイトルを付ける
3. **詳細な説明**: 変更の背景、目的、影響を説明する
4. **Issueへのリンク**: 関連するIssueへのリンクを含める
5. **レビュアーの指定**: 適切なチームメンバーをレビュアーに指定する
6. **変更の説明**: コードの変更点をコメントで説明する

### レビュープロセス

1. **コードの確認**: 変更されたコードを確認
2. **テストの実行**: 自動テストの結果を確認
3. **フィードバックの提供**: 建設的なフィードバックを提供
4. **承認/改善要求**: レビュー後に承認または改善を要求

## GitHub Actions

GitHub Actionsは、リポジトリ内のイベントに基づいて自動化されたワークフローを実行する機能です。

### CI/CDワークフローの設定

`.github/workflows/ci.yml` ファイルを作成して、継続的インテグレーション（CI）ワークフローを設定します：

```yaml
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main, develop ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run linting
      run: npm run lint
    
    - name: Run tests
      run: npm test
```

### リリースワークフローの設定

`.github/workflows/release.yml` ファイルを作成して、リリースワークフローを設定します：

```yaml
name: Release

on:
  push:
    tags:
      - 'v*'

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Build project
      run: npm run build
    
    - name: Create GitHub Release
      uses: softprops/action-gh-release@v1
      with:
        files: |
          dist/*
        body_path: CHANGELOG.md
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### GitHub Actionsのベストプラクティス

1. **再利用可能なワークフロー**: 繰り返し使用するジョブは再利用可能なワークフローとして定義
2. **キャッシュの活用**: 依存関係やビルド結果をキャッシュして高速化
3. **マトリックステスト**: 異なる環境やバージョンでのテスト
4. **環境変数の管理**: 機密情報はシークレットとして保存
5. **エラー処理**: ワークフローの失敗時の通知設定

## 実践演習

この章では、実際にTODOアプリケーションを開発しながら、これまで説明したGitHubの機能やベストプラクティスを実践します。

### 演習1: Issueの作成と管理

1. 以下の機能に関するIssueを作成します：
   - TODOアイテムの追加機能
   - TODOアイテムの完了/未完了の切り替え機能
   - TODOアイテムの削除機能
   - TODOリストのフィルタリング機能

2. 各Issueに適切なラベルとマイルストーンを設定します。

### 演習2: ブランチの作成とコード実装

1. `develop` ブランチを作成します：
   ```bash
   git checkout -b develop
   git push origin develop
   ```

2. 「TODOアイテムの追加機能」のIssueに対応するfeatureブランチを作成し、機能を実装します：
   ```bash
   git checkout develop
   git checkout -b feature/issue-1-add-todo-item
   ```

3. HTMLファイル（src/index.html）を編集：
   ```html
   <!DOCTYPE html>
   <html lang="ja">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>TODOアプリ</title>
     <link rel="stylesheet" href="style.css">
   </head>
   <body>
     <div class="container">
       <h1>TODOリスト</h1>
       <div class="todo-input">
         <input type="text" id="new-todo" placeholder="新しいTODOを入力...">
         <button id="add-button">追加</button>
       </div>
       <ul id="todo-list"></ul>
     </div>
     <script src="app.js"></script>
   </body>
   </html>
   ```

4. CSSファイル（src/style.css）を編集：
   ```css
   * {
     margin: 0;
     padding: 0;
     box-sizing: border-box;
   }
   
   body {
     font-family: Arial, sans-serif;
     line-height: 1.6;
     background-color: #f4f4f4;
   }
   
   .container {
     max-width: 500px;
     margin: 30px auto;
     padding: 20px;
     background: white;
     border-radius: 5px;
     box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
   }
   
   h1 {
     text-align: center;
     margin-bottom: 20px;
     color: #333;
   }
   
   .todo-input {
     display: flex;
     margin-bottom: 20px;
   }
   
   #new-todo {
     flex: 1;
     padding: 10px;
     border: 1px solid #ddd;
     border-radius: 4px 0 0 4px;
   }
   
   #add-button {
     padding: 10px 15px;
     background: #4caf50;
     color: white;
     border: none;
     border-radius: 0 4px 4px 0;
     cursor: pointer;
   }
   
   #todo-list {
     list-style: none;
   }
   
   .todo-item {
     padding: 10px;
     border-bottom: 1px solid #eee;
     display: flex;
     justify-content: space-between;
     align-items: center;
   }
   
   .todo-item.completed span {
     text-decoration: line-through;
     color: #888;
   }
   
   .todo-item button {
     padding: 5px 10px;
     background: #ff5252;
     color: white;
     border: none;
     border-radius: 4px;
     cursor: pointer;
   }
   ```

5. JavaScriptファイル（src/app.js）を実装：
   ```javascript
   document.addEventListener('DOMContentLoaded', () => {
     const newTodoInput = document.getElementById('new-todo');
     const addButton = document.getElementById('add-button');
     const todoList = document.getElementById('todo-list');
     
     // TODOアイテムを追加する関数
     function addTodo() {
       const todoText = newTodoInput.value.trim();
       
       if (todoText === '') return;
       
       const todoItem = document.createElement('li');
       todoItem.className = 'todo-item';
       
       const todoSpan = document.createElement('span');
       todoSpan.textContent = todoText;
       
       const deleteButton = document.createElement('button');
       deleteButton.textContent = '削除';
       deleteButton.addEventListener('click', function() {
         todoList.removeChild(todoItem);
       });
       
       todoItem.appendChild(todoSpan);
       todoItem.appendChild(deleteButton);
       todoList.appendChild(todoItem);
       
       newTodoInput.value = '';
       newTodoInput.focus();
     }
     
     // 「追加」ボタンのクリックイベント
     addButton.addEventListener('click', addTodo);
     
     // 入力フィールドでEnterキーが押された時
     newTodoInput.addEventListener('keypress', function(e) {
       if (e.key === 'Enter') {
         addTodo();
       }
     });
   });
   ```

6. 変更をコミット&プッシュ：
   ```bash
   git add .
   git commit -m "feat: TODOアイテムの追加機能を実装"
   git push origin feature/issue-1-add-todo-item
   ```

### 演習3: Pull Requestの作成とレビュー

1. GitHubのリポジトリページで、`feature/issue-1-add-todo-item` ブランチから `develop` ブランチへのPull Requestを作成します。

2. PRテンプレートに従って、必要な情報を入力します。

3. チームメンバーにレビューを依頼し、フィードバックを得ます。

4. 必要に応じて修正を加え、PRをマージします。

### 演習4: GitHub Actionsの確認

1. CIワークフローが正常に実行されていることを確認します。

2. 簡単なテストファイル（tests/app.test.js）を作成します：
   ```javascript
   test('TODOアイテムの追加', () => {
     // ここにテストコードを実装
     expect(true).toBe(true);
   });
   ```

3. 変更をコミットしてプッシュし、CIワークフローが実行されることを確認します。

### 演習5: リリースプロセス

1. リリース用のブランチを作成します：
   ```bash
   git checkout develop
   git checkout -b release/v0.1.0
   ```

2. バージョン情報を更新し、CHANGELOGを作成します。

3. リリースブランチを `main` にマージするPRを作成します。

4. PRがマージされたら、タグを作成してリリースします：
   ```bash
   git checkout main
   git pull origin main
   git tag v0.1.0
   git push origin v0.1.0
   ```

5. GitHub Actionsのリリースワークフローが実行され、リリースが作成されることを確認します。

## トラブルシューティング

### よくある問題と解決策

1. **マージコンフリクト**:
   - 原因: 複数の開発者が同じファイルの同じ部分を編集した場合
   - 解決策: 
     ```bash
     git pull origin ブランチ名
     # コンフリクトを解決し、ファイルを編集
     git add .
     git commit -m "resolve: マージコンフリクトを解決"
     git push origin ブランチ名
     ```

2. **GitHub Actionsの失敗**:
   - 原因: テストや構文チェックでエラーが発生
   - 解決策: ワークフローのログを確認し、エラーを修正

3. **GitHubのPermission拒否**:
   - 原因: リポジトリへの権限が不足している
   - 解決策: リポジトリの設定で適切な権限を付与する
