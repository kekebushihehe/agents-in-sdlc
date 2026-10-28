---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローを進めました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、シード済みのフィルタリング Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認し、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリング Issue から始め、Plan モードで方針を定め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーの証拠を評価しました。
- フィルタリング全体の変更をレビューし、フィルタリング PR で Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュ コマンドを確認しました。

## リリースしたもの

このワークショップには、それぞれ最新の `main` から作成した別ブランチを使う 2 つの PR の節目があります。

1. **星評価:** 既存の `starRating` と未評価であることを示す状態をゲーム カードに表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して、関連するテストを含めます。

フィルタリングの計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡略化するため、これらの作業を 1 つの PR にまとめました。

## 複数の方法で検証する

自動テスト、自分でのブラウザー確認、MCP を使った Copilot のブラウザー確認など、複数の方法でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、カスタマイズした形式で結果を報告しました。QA は要件とテスト カバレッジのレビューをまとめ、これらの結果と照合しました。

追加するテストは、実際の不足を解消するものにします。QA の実行で追加テストが不要と判断される場合もあります。マージを許可する前に、コードと根拠をレビューしてください。変更があった場合は、影響を受ける検証結果を更新します。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズに合わせて調整します。指示は標準を設定し、スキルは繰り返し実行できるタスクを記述し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに応じて**モードとモデル**を選びます。実装前に方針を考えるには **Plan**、範囲の小さい変更を進めながら確認するには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を作るか、なぜ必要か、どのように作るかを明確に説明すると、結果が大きく変わります。

## さらに確認する

主要なワークフローは完了しました。Copilot CLI には、他にも次のような機能があります。

- `/review` でコードレビュー エージェントに変更を分析させる。
- `/rubber-duck` で問題について会話し、別の視点を得る。
- `/fleet` で独立したサブタスクを並列に調整する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送る。

## 次のステップ

ツールの使い方を上達させるには、使い続けるのが一番です。本番コード、趣味のコード、長年作りたいと思っていた小さなアプリなどで活用してください。チームと知識を共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
