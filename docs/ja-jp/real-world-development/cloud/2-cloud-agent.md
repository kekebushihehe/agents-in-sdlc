---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩まされていない組織は、ほとんどありません。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト プルリクエスト (PR) を作成します。面倒な作業を任せて開発を加速し、開発者がより大きな課題に集中できるようになります。

Copilot cloud agent を使って、次の内容を確認します。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- Issue の範囲を明確にする重要性
- Issue を Copilot に割り当てる方法

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した委託先は、ドキュメントを十分に整備していません。実際、ドキュメントはほとんどありません。まず、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のタイトルを表示して、ユーザーがさらにゲームを閲覧できるようにします。現時点では洗練された実装は不要で、UX の受け入れテストに使える状態を目指します。この作業は現在ブロッカーですが、優先度がより高い Issue が他にあります。

これらは優先順位が下がりやすいタスクの例で、Copilot cloud agent に適しています。cloud agent が非同期で作業している間、開発者は別のタスクに取り組み、後から Copilot の作業をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。たとえば、[GitHub Issue を Copilot に割り当て][assign-issue]ます。割り当てると、Copilot は進行状況を追跡するドラフト プルリクエスト (PR) を作成し、環境を準備して、タスクに取り組み始めます。作業中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を適切に定めた指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく確認すると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に定めてください。AI ペア プログラマーとして扱い、段階を踏んで学び、試し、調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への指示方法を確認しました。カスタム指示の演習をすでに行った場合は `.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、cloud agent に作業を依頼する前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認してください。

リポジトリの GitHub Web UI、または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメント コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターで、指示が `**/*.test.ts` ファイルに適用されることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成されたプルリクエストに TSDoc ドキュメント コメント、TypeScript の規約、コメント ヘッダーが反映されているか確認します。後の演習で PR をレビューするときに、改めて説明します。

## Copilot cloud agent の開発環境を設定する

コードの作成には通常、環境の準備とセットアップ スクリプトの実行が必要です。ソフトウェア エンジニアと同じようにタスクを実行する Copilot に作業を割り当てる場合も同様です。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業を始める前に必要な開発ツールと依存関係を利用できるようになります。ラボの進行と学習のため、このワークフローは事前に設定されています。単一の Astro アプリ用に Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、移行およびシード済みの SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限のアクセス許可を設定します。Copilot は操作用の独自のトークンを受け取ります。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカル SQLite データベースを移行してシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じですが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーにより、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトで情報が古いままになっていたり、十分に整備されていなかったりします。この種の技術的負債は放置されやすく、生産性の低下や、コードベースの保守、新しい開発者の受け入れの難しさにつながります。Copilot はドキュメントの作成に役立つため、cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを作成し、後の演習で結果を確認します。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    関数やメソッドの動作を明確にするうえで役立つ場合は、TSDoc のドキュメント コメントを含めることが Organization の要件です。しかし、最近の更新はこの標準に従っていません。動作の説明に役立つ箇所にドキュメント コメントを追加し、既存コードを更新してください。
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエストを開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が To Do リストを作成していることを確認します。

> [!NOTE]
> Copilot の To Do リストが PR に表示されるまで数分かかる場合があります。Copilot は環境を作成し、前述のワークフローを実行して、プロジェクトを分析し、問題への最適な取り組み方を検討しています。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ブラウザーを更新してください。

16. ライブ セッションをスクロールして、Copilot がどのように問題を解決するか確認します。コードを調査して状態を把握し、計画を考えて決定し、コードを作成する様子を確認できます。

完了まで数分かかることがあります。Copilot cloud agent の主な目的は、非同期でタスクを実行して、開発者が他の作業に集中できるようにすることです。この機能を活用し、別のタスクを Copilot cloud agent に割り当ててから、アプリケーションの機能追加にも取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザイン チーム向けに関連ゲームを追加する作業に時間がかからないとしても、その時間を別の作業に使える可能性があります。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    各ゲームの詳細ページに関連ゲームを表示し、ユーザーが他のゲームを見つけられるようにしたいと考えています。デザイン チームは UX を確認し、受け入れテストを行いたいと考えています。要件は次のとおりです。

   - `src/lib/` にデータ アクセス ヘルパーを追加し、指定されたゲームと同じカテゴリに属する他のゲームを返します。指定されたゲーム自体は除外します。
   - ゲーム詳細ページに「関連ゲーム」セクションを表示し、このヘルパーを使います。
   - 関連ゲームがない場合を処理します。
   - 新しいヘルパーのユニット テストを作成します。
   - PR を作成する前に、すべてのテストが成功することを確認します。
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後、Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![Copilot が Issue の作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同様に作業するため、常に監視する必要はありません。完了後にレビューします。次は、カスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について確認しました。cloud agent に Issue を割り当てると、非同期で作業を実行できます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに利用できます。

この演習では、次の内容を確認しました。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- Issue の範囲を明確にする重要性
- Issue を Copilot に割り当てる方法

cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントを作成して使用します。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
