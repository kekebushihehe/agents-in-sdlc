---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は、関連ファイルを見つけて変更し、テストを実行し、問題をデバッグして、変更漏れがあればさらに修正します。作業は続きます。

Copilot Agent Mode を使うと、統合開発環境 (IDE) の中でより自律的に作業できます。開発者と同様に、プロジェクト構成を調べ、必要なファイルを更新し、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使い、サイトに新しい機能を追加しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途に使います。エージェント モードは IDE で作業し、すばやいフィードバックと対話を可能にします。Cloud agent はチームの一員のように非同期で作業し、Issue やプルリクエストを通じてやり取りします。

この演習では、次の方法を学びます。

- Copilot Agent Mode を使ってプロジェクトを調べ、関連ファイルを見つけ、複数ファイルにまたがる変更を行います。
- GitHub Copilot Agent Mode でユーザー インターフェイス (UI) とデータ層にまたがる新機能を実装します。
- Copilot Agent Mode が生成した変更とテストをレビューしてから、コードベースにマージします。

## シナリオ

ゲーム一覧が増えてきたため、ユーザーがカテゴリや発行元で絞り込めるようにします。前の演習で発行元ヘルパーを追加しました。残りのデータ層、UI、テストを Copilot Agent Mode で完成させます。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握します。

この Web サイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ゲーム一覧と詳細を表示します。単一の Astro アプリで、ビルド時にページを静的な HyperText Markup Language (HTML) としてレンダリングします。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接クエリします。別個のバックエンド アプリケーション プログラミング インターフェイス (API) やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、対話機能は標準のドキュメント オブジェクト モデル (DOM) API を使った小規模な Astro `<script>` で追加します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクを用意しています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の続きで、`main` にいることを確認します。
2. Codespace で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Codespace の Web サーバーをホストするためのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、次の主な機能を確認します。

- **ホーム ページ**: タイトル、画像、説明を含むボード ゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、そのゲームのタイトル、説明、発行元、カテゴリなどの詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると、Issue のバックログが自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** は複数ファイルにまたがる変更を自律的に計画・実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成には Claude Sonnet 4.5 を使ったため、手順に含めています。コード提案を比較的そろえる意図がありますが、大規模言語モデル (LLM) の出力は確率的です。ワークショップの例と異なる提案が表示されても、正常な動作です。

> [!NOTE]
> 出力の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、Issue のバックログを確認します。

   ```plaintext
   GitHub リポジトリのバックログを表示してください。ユーザーに最も役立つ項目を基準に優先順位を付けるのを手伝ってください。
   ```

4. **Continue** を選択し、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が、ユーザーに最も役立つと思われる項目に優先順位を付けたことを確認します。

## 指示ファイルを確認する

コード生成をエージェントに依頼する前に、作業のコンテキストを提供する指示ファイルを確認します。Web サイトの機能追加方法に関する指針を記載する [user interface (UI)][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle などのファイル形式に関する指示へのリンク

> [!TIP]
> 指示ファイルからは、他の指示ファイルとプロジェクト内のファイルの両方を参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用できる小さな単位に分け、例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の 3 つを更新する必要があります。

- データ層 (`src/lib/`) の残りのフィルタリング ロジックを追加または調整します。
- フィルタリング動作のテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

コードベースにマージする前に、テストを実行して成功させる必要があります。Copilot Agent Mode にこれらのタスクを依頼します。

1. Copilot の現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** を選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 Issue に基づき、次のプロンプトで実装を依頼します。

   ```plaintext
   バックログにある関連 GitHub Issue の要件に基づいて、発行元とカテゴリによるフィルタリングをサイトに追加してください。前の演習で発行元ヘルパーを作成済みなので、必要に応じてその作業を維持・改善しながら、残りのデータ層、UI、テストを完成させてください。完了前にすべてのテストが成功することを確認してください。サーバーはすでに実行中なので、起動する必要はありません。
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データ層のヘルパー、UI、テストを見つけてファイルを変更し、テストを実行します。

   ![Copilot がプロジェクト ファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テスト実行などを行うため、タスクによっては数分かかります。作業中、コード エディターに **Keep** と **Undo** が表示されることがあります。Copilot が完了すると、すべての変更に **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択し、テストを実行します。

   ![Copilot Chat ペインに表示されたテスト実行の確認ダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストに失敗したりする場合があります。Copilot はコード生成とテストを繰り返し、タスクが完了してエラーが見つからなくなるまで作業します。

   ![Copilot Agent Mode の完了済みチャット セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新しい機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode で Tailspin Toys Web サイトに機能を追加する方法を確認しました。次の操作を行いました。

- GitHub Copilot Agent Mode で UI とデータ層にまたがる新機能を実装しました。
- Copilot Agent Mode を使ってプロジェクトを調べ、関連ファイルを見つけ、複数ファイルにまたがる変更を行いました。
- Copilot Agent Mode が生成した変更とテストをレビューしました。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 — ページ分割を実装する

ゲーム一覧が増えるにつれて、ページ分割が必要になります。この演習で学んだことを使い、Copilot にサイトのページ分割を依頼してください。次の点を検討します。

- 既存の指示ファイルを使うなど、既存のベスト プラクティスに従います。
- ページ サイズをユーザーが選べるようにするか、固定にするかなど、ページ分割方法を決めます。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- GitHub Copilot にコンテキストを伝えて変更を依頼し、反復が必要な場合があります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
