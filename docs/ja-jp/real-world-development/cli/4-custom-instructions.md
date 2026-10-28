---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。作業方法に決まりがある場合は、Copilot がその指針を参照できるようにします。[指示ファイル][instruction-files]には、作成してほしいコードだけでなく、その構成方法も記載します。フィルタリング機能を実装したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示とパスで適用範囲を指定した指示ファイルがエージェントに読み込まれる方法を確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに及ぼす影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。

- コメントでは、コードを言い換えるのではなく、目的や自明でない決定を説明します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値、および該当する場合は注入可能な `db` 引数を説明する TSDoc/JSDoc を記載します。
- 再利用可能な Astro コンポーネントには `Props` 契約を記載し、関連コードが変更されたときにコメントも最新の状態にします。
- 既存のフォーマットと lint の指針を維持します。

指示ファイルを使い、Copilot がこれらの開発手法に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝えられます。規約、ライブラリ、コードに含めるコメントの種類などを指定できます。リポジトリ全体、または特定の種類のファイルに対して指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に渡される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、タスクに関する指針を記載します。

> [!NOTE]
> 他の指示形式への対応状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すでに複数の指示ファイルがあります。変更前に内容を確認して、指示が作業にどう影響するかを把握します。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要と指針を確認します。この指示は、リポジトリ内で Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを glob で指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存プロジェクト ファイルへのリンクを確認します。指示を再利用できる小さなファイルに分け、Copilot が参考にできる例を示せます。ここに記載されたパスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting guidance** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの規則はありません。次の手順で追加します。

## チームのガイダンスに合わせて指示ファイルを更新する

`.github/copilot-instructions.md` に新しい標準を追加し、これから実装するフィルタリング機能のコードにも適用します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します。35 行目付近にあります。
2. そのセクションの最後に、次の Markdown 行を追加します。

   ```markdown
   - 新しい TypeScript コードには、ドキュメントとして TSDoc コメントを含めてください。
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は、会話の開始時にリポジトリの指示を読み込みます。更新した指示を読み込ませるため、フィルタリングの会話を終了して再開します。機能のコンテキストはそのまま保持します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで、`game-filters-cli` ブランチから保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトを Copilot に送信して、更新したガイダンスを適用します。

   ```plaintext
   指示とコードのガイダンスを更新しました。生成したコードをこのガイダンスに合わせて更新してください。
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい標準をフィルタリング機能に適用しました。このレッスンでは、次の操作を行いました。

- リポジトリの指示とパスで適用範囲を指定した指示ファイルがエージェントに読み込まれる方法を確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに及ぼす影響を確認しました。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行します][next-lesson]。lint とテストを一貫して実行できます。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示の作成に関するベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
