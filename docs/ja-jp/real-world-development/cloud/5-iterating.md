---
title: "演習 5: GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善や機能追加を目的に、GitHub Copilot に複数のタスクを依頼しました。コードへのドキュメント追加、デザイン チームが改善できる関連ゲーム機能の作成、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。ここでは、コードの変更を確認し、必要に応じて Copilot にフィードバックを送ります。

### シナリオ

生成 AI を利用しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体にレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関して GitHub Copilot が提案した変更を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は、監視を受けずに非同期でタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みできるのは作業に使うブランチだけです。
- Cloud agent は GitHub Actions 内に一時的な専用環境を作成して実行されます。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されます](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。Model Context Protocol (MCP) サーバーも対象です。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は MCP サーバーへの外部接続を含め、既定で外部リソースへのアクセスが制限されているため、これは想定内です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** が表示されます。作業が完了するまで待ってください。数分かかる場合があります。その間、休憩を取ったり、これまでに学んだことを振り返ったりできます。

4. 準備ができたら **Files changed** タブを選択し、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに追加された TSDoc のドキュメント コメントなど、更新されたコードを確認します。実際の変更内容は状況によって異なります。

   TSDoc ドキュメント コメント、TypeScript の規約、コメント ヘッダーを確認します。これらは演習 2 の最初に確認したカスタム指示ファイルから反映されています。

6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローの承認が必要な場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)
9. プルリクエストのチェック欄にワークフローが追加されます。すべて問題なければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot とやり取りする方法は、レビューだけではありません。チーム メンバーに依頼するように、プルリクエストやコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するセッションを開始します。結果は確定的ではないため、必ずしも同じプロンプトを指定できるわけではありません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、内容を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc ドキュメント コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. **@copilot** をユーザーと同じようにメンションして、生成されたドキュメントの変更をコメントで依頼します。上記の例、またはコードベースに追加したいドキュメントについて、別の提案を使ってください。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更を確認します。

満足するまで改善を繰り返せます。問題がなければ、ドラフト PR をレビュー可能な状態にして、`main` ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローの承認が必要な場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが追加されます。すべて問題なければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、cloud agent とのやり取りを繰り返して変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーと TSDoc ドキュメント コメントを追加したりできます。この作業はカスタム指示を更新する前に割り当てたため、指示が反映されていない場合があります。前と同じように **Conversation** タブでコメントを追加すると、Copilot はそれを確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 のミッション コントロールで追加を依頼したライト モードが含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot が作業中の場合、プルリクエストに **[WIP]** が表示されます。作業が完了するまで待ってください。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の項目に注意します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保存するためのローカル ストレージの使用
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更
   - アクセシビリティ属性 (Accessible Rich Internet Applications (ARIA) ラベル、キーボード ナビゲーションなど)
   - モード切り替えを管理する JavaScript/TypeScript コード
6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローの承認が必要な場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されます。すべて問題なければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。ターミナルを開き、Copilot が作成したブランチ名に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次に、アプリケーションを起動し、ブラウザーでハイ コントラスト モードとライト モードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタム エージェントが、アクセシビリティのベスト プラクティスに従って Copilot の機能実装を導いたことを確認します。アクセシビリティに関する懸念や改善案があれば、前の PR と同様にコメントで **@copilot** をメンションして修正を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent は、バックログのタスクを任せて別の作業に集中するときに役立ちます。いくつか Issue を作成して Copilot に割り当て、この習慣を身に付けてください。次のようなタスクを試せます。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。このレッスンを通して、次の操作を行いました。

- このリポジトリに含まれるカスタム指示ファイルを確認し、cloud agent の出力に及ぼす影響を確認しました。
- Issue を Copilot cloud agent に割り当て、非同期で環境を準備し、計画を立て、実行する様子を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使用し、ハイ コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページをミッション コントロールとして使用し、アクセシビリティ セッションを監視して方向付けました。
- cloud agent のプルリクエストをレビューし、`@copilot` に変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。これまでの内容をさらに学ぶ場合は、他のハーネスも確認してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat のエージェント モードと MCP 統合を体験します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、QA エージェント、Playwright MCP、Agent Merge、および cloud agent に戻る任意の `/delegate` ブリッジを使い、レビュー済みの変更を 2 つターミナルからリリースします。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てます。
- 他の分野のカスタム エージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
