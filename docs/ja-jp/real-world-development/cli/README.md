---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、Model Context Protocol (MCP) ツール、pull request の自動化を活用し、Tailspin Toys に2つの変更を加えて検証し、公開します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** は、GitHub Copilot をエージェント型コーディングアシスタントとしてターミナルで利用できるようにします。コードベースを調査し、コードを生成し、コマンドを実行し、外部ツールに接続できます。コマンドラインから作業できるため、グラフィカルエディターに切り替えずに流れを保てます。

このワークショップでは、Tailspin Toys を使った一連の開発ワークフローに沿って進めます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、基本を確認する。
2. 星評価の小さな変更を加え、ブラウザーでレビューして、最初の pull request (PR) を手動でマージする。
3. フィルター機能の Issue から開始し、Plan モードでアプローチを定義し、Autopilot モードで構築してから、Interactive モードでレビューする。
4. リポジトリの指示を更新し、フィルター機能に適用する。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行する。
6. Playwright の Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルター機能を確認する。
7. quality assurance (QA) カスタムエージェントを作成し、要件、テスト範囲、検証の証拠を確認する。
8. フィルター機能全体をレビューし、Agent Merge を使って PR をマージする。
9. コンテキスト、モデル、共有、任意のクラウド委任に役立つスラッシュコマンドを確認する。

ワークショップでは、星評価の変更と、指示の更新、スキルの更新、QA プロファイル、テストを含むフィルター機能の変更という、2つの PR を作成します。各ツールを学ぶ間、フィルター機能と品質ワークフローでは同じ会話とブランチを使い、作業を積み重ねていきます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、基本を確認する |
| [2. 星評価の追加で小さな成果を得る][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、最初の PR をマージする |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューする |
| [4. カスタム指示による Copilot のガイド][ex4] | コンテキスト | 指示を確認および更新し、フィルター機能に適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存のスキルを確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を構成し、フィルター機能の動作を確認する |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門プロファイルを作成して選択し、最終検証の証拠を集める |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI のスラッシュコマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、クラウドエージェントへの任意の委任を確認する |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、追加リソースを確認する |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイして、Web サイトに接続する |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンドライン操作の基本的な知識

> [!TIP]
> 有料プランをお持ちでない場合、認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の各機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI の使用を有効にしていることを確認してください。

## はじめに

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
