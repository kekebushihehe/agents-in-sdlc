---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業の進め方に決まりがある場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報にアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何を実行するかだけでなく、どのように実行するかも伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスで適用範囲を指定した指示ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、影響をリアルタイムで並べて示すのは困難です。このハーネスの後半で、cloud agent が作成するプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。

- データ層には必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc のドキュメント コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発手法に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝えられます。規約、ライブラリ、コードに含めるコメントの種類などを指定でき、より適切な提案やコード スニペットを得るのに役立ちます。リポジトリ全体、または特定の種類のファイルに対して指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に渡される単一の指示ファイルです。技術スタック、開発対象の概要、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に指示を適用するためのファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニット テストの追加などに使えます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと、`applyTo` フロントマターで適用範囲を指定した `*.instructions.md` ファイルの両方をコード生成に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、他の標準形式の指示ファイルにも対応しています。[対応する指示ファイル形式][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

このワークショップでは、指示ファイルの作成について詳しく扱いません。サンプル プロジェクトには、代表的な構成例が含まれています。

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

このリポジトリに用意されている指示ファイルを確認します。中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針もあります。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示ファイルがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルをリポジトリ ルートからの glob で指定しています。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルに適用されます。
5. このプロジェクトでユニット テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、最後までスクロールします。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用可能な小さなファイルに分け、コード生成の参考例を示せます。ここに記載されたパスはリポジトリ ルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント化はまだ必須ではありません。次の手順で、TSDoc のドキュメント コメントとファイル ヘッダーの規則を追加します。

## リポジトリ全体に適用する標準を追加する

ここでは、コード内に TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーを追加するというプロジェクト全体の規則を設定します。後の演習で cloud agent が Issue に取り組むとき、この指示を使います。

編集する前に、作業用のブランチを作成します。cloud agent は Issue の対象ブランチにある指示を読み取りますが、変更を整理してコミットするため、ここではブランチを使います。

1. Codespace のターミナルで、次のコマンドを実行してブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は、Copilot にプロジェクト レベルの情報を提供するファイルです。コード提案の改善に役立つよう、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` を開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントに関する規則はありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準の直下に、次の Markdown 行を追加します。Copilot にファイル ヘッダーと TSDoc ドキュメント コメントを追加するよう指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ レベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、言語、ファイル形式、タスクに応じた `*.instructions.md` ファイルとして作成することもできます。ドキュメント コメントの規則のようなプロジェクト全体の標準は、リポジトリ レベルのファイルに記載します。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は、Issue が対象とするブランチの指示ファイルを読み取ります。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。変更した指示を使わせるには、`main` に反映しておく必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。次の演習で割り当てる Issue では、cloud agent がこの指示を読み取ります。

> [!TIP]
> このワークショップではブランチを使い、実際のプロジェクトに近い手順を体験します。ワークショップ用に `main` へ直接コミットしてもかまいません。その場合はブランチを作成する手順を省略できます。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。このハーネスの後半で、cloud agent が作成するプルリクエストをレビューするときに、この標準が使われていることを確認します。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使用する指示ファイル][instruction-files]
- [カスタム指示の作成に関するベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
