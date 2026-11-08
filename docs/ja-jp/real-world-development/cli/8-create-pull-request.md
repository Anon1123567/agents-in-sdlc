---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター機能、手順、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、手順の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを開く準備ができました。最初の星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを管理させます。

> [!NOTE]
> 通常は機能、手順の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップを簡潔にするため、フィルター機能と品質ワークフローを 1 つの会話とブランチで進め、1 つの PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージまでのライフサイクルを自動化する方法を学びます。
- 機能全体の変更と検証結果を確認します。
- フィルター機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフローで、Copilot を使って計画、実装、検証を行いました。Tailspin Toys では、マージの許可は開発者が管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージ可能にするために残っている作業を自動化します。有効にすると、Copilot は PR を妨げている問題（失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースなど）に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

ここまでは、自分で **Merge pull request** を選択してきました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、作業をレビューし、マージできる状態であることを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、全体をレビューして PR を作成し、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージが実行されます。
7. Copilot から「PR #14 was squash-merged successfully.」などのメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている方法でレビュー済みのマージを手動で完了してください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、いくつかの開発プロセスを自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージまでのライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証結果を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に役立つ [Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
