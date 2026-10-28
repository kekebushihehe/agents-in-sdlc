---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業方法に決まりがある場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報にアクセスできるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。特に重要なのが、コード生成の指針を伝える[指示ファイル][instruction-files]です。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスで適用範囲を指定した指示ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示を使って、フィルタリング用の最初のデータ スライス (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従うことを確認します。
- 指示の更新とフィルタリング スライスを `main` にコミットし、次の演習で Copilot が更新済みの指針を使用できるようにします。

> [!CAUTION]
> 生成コードが、設定した標準の一部に従わない場合があります。Copilot の結果は確率的です。この演習では、出力を一字一句同じにするのではなく、指示の更新による傾向の変化を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。

- データ層には必ずユニット テストを用意します。
- ユーザー インターフェイス (UI) はダーク モードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc のドキュメント コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発手法に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝えられます。規約、ライブラリ、コードに含めるコメントの種類などを指定でき、より適切な提案やコードを得るのに役立ちます。リポジトリ全体、または特定の種類のファイルに対して指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に渡される単一の指示ファイルです。技術スタック、開発対象の概要、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に指示を適用するためのファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニット テストの追加などに使えます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと、`applyTo` フロントマターで適用範囲を指定した `*.instructions.md` ファイルの両方をコード生成に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、他の標準形式の指示ファイルにも対応しています。[対応する指示ファイル形式][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルを作成する際は、次の点を参考にしてください。

- `copilot-instructions.md` には、開発対象の説明、プロジェクト構成、全体的なコーディング標準など、プロジェクト レベルの指針を記載します。
- `*.instructions.md` には、ユニット テスト、Astro コンポーネント、データ層など、特定のファイル形式やタスクに関する指針を記載します。
- 自然な言葉を使い、明確なガイダンスを記載します。コードの良い例と避けるべき例も示します。

指示ファイルの作成方法は 1 つではありません。AI と同じく、プロジェクトの環境やシナリオに合わせて試行し、調整してください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新用][ui-instructions]や [Astro 用][astro-instructions]など、さまざまなタスクに対応する指示ファイルがあります。
>
> Copilot に指示ファイルを作成させることもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、サーフェスごとに操作方法が異なります。該当するサーフェスのレッスンで、関連する操作を説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタム エージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトに含まれる指示ファイルを確認してから変更し、結果の違いを見られるようにします。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要と指針を確認します。これらの指示は、リポジトリ内で Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドが、適用対象のファイルを glob で指定していることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存プロジェクト ファイルへのリンクを確認します。指示を再利用できる小さなファイルに分け、Copilot が参考にできる例を示せます。ここに記載されたパスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次の手順で TSDoc ドキュメント コメントとファイル ヘッダーの規則を追加します。

## 変更用ブランチを作成する

変更用のブランチを作成します。

1. 前の演習で使用した Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. Copilot にヘルパーの配置場所を示すため、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   `src/lib/publishers.ts` を作成または更新し、すべての発行元について名前と ID の一覧を返すデータ アクセス ヘルパーを追加してください。ファイルに変更を適用してください。
   ```

5. Copilot はプロジェクトを調べて、ヘルパー ファイルとテストなどのコードを更新します。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、発行元の型付き配列を返す型付き関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約によるものです。
7. 提案されたコードに TSDoc ドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の結果は確率的なため、指示がなくてもドキュメント コメントを追加する可能性があります。その場合も問題ありません。指示の更新後に一貫性が高まることを確認するのが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` にプロジェクト レベルの情報を記載し、コード提案の改善に役立てます。

1. `.github/copilot-instructions.md` を再度開きます。
2. 35 行目付近にある **Code formatting guidance** セクションを探します。
3. 最後の項目として、次の Markdown 行を追加します。

   ```markdown
   - 新しい TypeScript コードには、ドキュメントとして TSDoc コメントを含めてください。
   - import 文またはコードの前に、ファイルの目的を説明するコメント ブロックを追加してください。
   ```

ファイルは自動的に保存されます。

## プロンプトを再実行して変更を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   `src/lib/publishers.ts` を作成または更新し、すべての発行元について名前と ID の一覧を返すデータ アクセス ヘルパーを追加してください。ファイルに変更を適用してください。
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディング プラットフォーム用の発行元データ アクセス ヘルパーです。
    * データベースから発行元情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * すべての発行元について ID と名前の一覧を返します。
    *
    * @param db - Drizzle データベース クライアントです。
    * @returns 発行元オブジェクトの配列を解決する Promise です。
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で使うコードに適用しました。

## 指示をコミットしてブランチをプッシュする

指示ファイルは他のリポジトリのファイルと同様に、ソース管理で管理します。指示の更新をブランチにコミットして、リポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. 次のコマンドを実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新とヘルパーの基盤に関して Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなど追加の関連ファイルが表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準 Issue に関連付けたプルリクエストを作成します。Copilot に Issue を検索させ、Issue を完了する PR を作成してマージします。

> [!NOTE]
> この演習では初めて Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートには GitHub ツールがあらかじめ設定されています。初回の利用時に VS Code から GitHub へのサインインを求められた場合は、画面の指示に従って許可します。後の演習で、この接続の仕組み (Model Context Protocol) を学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 次のプロンプトで、指示ファイルの更新に関する Issue を検索し、現在のブランチから PR を作成するよう Copilot に依頼します。PR には指示の更新と、今後のフィルタリング作業に備えた発行元ヘルパーの基盤を含めます。

   ```
   指示ファイルの更新に関する Issue を見つけてください。現在のブランチから新しい PR を作成し、指示の更新と新しい発行元ヘルパーの基盤の両方を説明してください。また、この PR でその Issue を完了することを明記してください。
   ```

4. Copilot が Issue を検索して PR を作成します。
5. Copilot が GitHub 操作の許可を求めたら、コマンドを確認し、問題がなければ **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージしてローカル ブランチを `main` に戻すよう依頼します。

   ```
   PR を `main` にマージしてください。その後、ローカルで `main` に戻り、最新のコードをプルして更新してください。
   ```

7. Copilot が GitHub 操作とシェル コマンドの許可を求めたら、コマンドを確認し、問題がなければ **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次の操作を行いました。

- 既存の指示を使ってコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再送信し、提案されたコードが新しい標準に従うことを確認しました。
- 指示の更新とヘルパーの基盤をコミットし、`main` に反映しました。次の演習で利用できます。

次は、[エージェント モード][next-lesson]を使って、Copilot にコードベース全体で新機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使用する指示ファイル][instruction-files]
- [カスタム指示の作成に関するベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
