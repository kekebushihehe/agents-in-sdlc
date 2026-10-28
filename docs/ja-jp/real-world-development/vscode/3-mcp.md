---
title: "演習 3 - Playwright Model Context Protocol (MCP) サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードでフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで動作を確認してください。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) と、MCP サーバーで Copilot にツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP を使わせて、ブラウザーでフィルタリング機能を手動テストします。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外部にあるツールを利用すると、さらに便利になります。Copilot がそのために使うのが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準的な方法です。

![エージェント モードの内部と、コンテキスト、大規模言語モデル (LLM)、MCP サーバーや VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP を使うことで、リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作を実行したりできます。

ツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じてアクセスします。MCP サーバーは、既存の API や npm パッケージなどのローカル ツールとの通信を管理します。各 MCP サーバーは、それぞれ異なるツールやリソースを提供します。

よく使われる MCP サーバーの例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスし、新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

他にも多くの MCP サーバーがあり、それぞれ異なるツールやリソースにアクセスできます。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献できるように[MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内の他の依存関係と同様に扱ってください。利用前にソース コードを慎重に確認し、発行元を検証し、セキュリティへの影響を考慮します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のような `github` サーバーがあらかじめ設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使用するため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供し、機能のテストに使えます。

1. `.vscode/mcp.json` で `github` と並ぶ位置に `playwright` エントリを追加し、ファイルを次の内容にします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、ブラウザー ウィンドウを表示せずに実行します。デスクトップ環境がない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys はエンドツーエンド テストに Playwright を使っているため、通常、必要なブラウザーはすでにインストールされています。Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行するよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて使うときに、VS Code がサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に表示される **Start** 操作を選択し、サーバーを起動して構成を確認します。
2. VS Code からサーバーを信頼するか確認された場合は、構成を確認して、サーバーの起動を許可します。

手動で起動する必要はありません。ここで起動しない場合は、次の手順で Copilot が初めてツールを使うときに VS Code が起動し、信頼の確認を表示します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot に実際のブラウザーを操作させることができます。開発者がアプリを操作しなくても、エージェントがページを開き、移動し、フィルターを適用して、結果を確認し、内容をまとめます。会話を続けながら機能の動作をすばやく確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントは、支援技術と同じように、ボタン、リンク、リスト項目などの構造化されたラベル付き要素を扱います。簡単な機能確認を通じて、アクセシビリティも確認できます。

サーバーを接続し、アプリを起動したら、次のプロンプトを Copilot に送信して、作成したフィルタリング機能をテストします。

```text
Playwright MCP サーバーを使って、実行中のアプリ `http://localhost:4321` をブラウザーで開き、新しいゲームのフィルタリング機能を確認してください。

1. ゲーム ページに移動し、一覧に表示されているゲームの数を記録します。
2. カテゴリ フィルターを適用し、そのカテゴリのゲームだけが一覧に表示されることを確認します。
3. フィルターを解除してから発行元フィルターを適用し、該当する発行元のゲームだけが表示されることを確認します。
4. カテゴリと発行元のフィルターを組み合わせ、両方が結果に反映されることを確認します。

各手順で確認したことを報告し、想定どおりに動作しない点があれば指摘してください。
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ条件に照らして報告を確認します。問題がある場合は追加で質問するか、プルリクエストを作成する前にコードの修正を依頼します。

> [!NOTE]
> テストするには、アプリを `http://localhost:4321` で起動しておく必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときにブラウザーのダウンロードが必要になる場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームでレビューするプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルに移動し、Copilot が作成した変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミット メッセージを生成します。

   ![変更が表示された Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、複数の方法でプルリクエストを作成できます。GitHub Copilot を使用中なので、PR の作成も依頼します。Copilot は関連 Issue を見つけ、その Issue に関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェント ドロップダウンで **Agent** が選択され、Copilot が GitHub ツールを使えることを確認します。
3. 次のプロンプトを Copilot に送信します。

   ```plaintext
   リポジトリでカテゴリと発行元によるフィルタリングに関する Issue を見つけてください。現在のブランチのプルリクエストを新しく作成し、該当する Issue に関連付けてください。
   ```

4. 必要に応じて **Continue** を選択し、情報の取得や操作を許可します。GitHub ツールを初めて使うときに GitHub の認証を求められた場合は、画面の指示に従います。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成することを確認します。
6. Copilot が生成したリンクを選択し、プルリクエストをレビューします。まだマージしないでください。

## まとめと次のステップ

この演習では、次の操作を行いました。

- Model Context Protocol (MCP) と、MCP サーバーで Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成しました。

MCP サーバーを使って機能をテストしました。MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューなど、対象を絞ったタスクを効率化する[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
