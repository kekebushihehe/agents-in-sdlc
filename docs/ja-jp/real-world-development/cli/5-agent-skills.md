---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能の検証に使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことは、単にコードを書くことだけではありません。これまでに、手動でコードの動作を確認し、指示ファイルを使って標準に従わせました。では、テストや lint、その他の継続的インテグレーション (CI) の作業はどうでしょうか。

このようなタスクには、**エージェントスキル**が適しています。スキルは、こうした操作を適切に実行する方法を Copilot が理解するのに役立ちます。

このレッスンでは、次の作業を行います。

- 既存の `quality-checks` スキルを確認する。
- 結果の書式をカスタマイズする。
- スキルを再読み込みして実行する。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使用します。チームは、結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルには、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースがまとめられており、エージェントが必要に応じて読み込みます。基本的には、スキル名のフォルダーと、その中の `SKILL.md` という Markdown ファイルで構成されます。Markdown ファイルには、スキルの名前と説明を定義する frontmatter、機能の概要、呼び出すべき状況についてのガイダンスが含まれます。スキルのフォルダーには、使用するスクリプトやその他のリソースを格納するサブフォルダーを追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトに既存のコマンドを使用するため、`SKILL.md` だけで構成されています。

スキルは、プロジェクトの `.github/skills` フォルダーに配置してチームで共有・再利用できるリポジトリアセットにするか、ユーザーのスキルフォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明によって、Copilot がスキルを呼び出す状況を理解しやすくなる点を確認します。
3. 指示を読み、テストと lint の手順をどのように案内しているかを確認します。
4. スキルに **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

Copilot CLI から直接、または自然言語を使ってスキルを呼び出せます。まず、Copilot にスキルを使って3つのチェックを実行するよう依頼します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行したチェック、成功したかどうか、ツールが実際に報告した内容がわかる、より優れたレポートにします。スキルを更新して、この形式のレポートを生成するようにします。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. 次のセクションをファイルの末尾に追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルが自動的に保存されたことを確認します。

## 更新したスキルを実行する

変更したスキルを使ったレポートを確認します。Copilot CLI は、会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使用しました。このレッスンでは、次の作業を行いました。

- 単体テスト、lint、型チェック用の `quality-checks` スキルを確認した。
- 結果の書式をカスタマイズした。
- スキルを再読み込みして実行した。

この変更は、フィルター機能とともに機能の PR に含めます。次は、[Playwright MCP サーバーを使ってサイトを直接操作する][next-lesson]方法を確認します。

## スキルの例

以下のコミュニティ提供の例は参考資料であり、追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とバンドルされたエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
