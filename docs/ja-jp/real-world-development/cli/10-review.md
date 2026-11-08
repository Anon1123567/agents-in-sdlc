---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されているフィルター機能のイシューを見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能のイシューから始めて、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム手順でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) のカスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーの根拠を評価しました。
- フィルター機能全体の変更をレビューし、フィルター機能の PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認しました。

## 作成したもの

このワークショップでは、更新済みの `main` からそれぞれブランチを作成し、2 つの PR を完了しました。

1. **星評価:** 既存の `starRating` と、評価がない場合の状態をゲームカードに表示します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、手順を更新して機能に適用します。`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使用しました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめました。

## さまざまな方法で検証する

自動テスト、自分で行うブラウザー上の確認、MCP 経由で Copilot に行わせるブラウザー上の確認など、複数の方法でコードを確認しました。`quality-checks` スキルではユニットテスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果に要件とテストカバレッジのレビューを組み合わせ、PR の前に確認しました。

追加するテストは、実際の不足を解消する必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと検証結果をレビューし、変更後は関連する根拠を再確認してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールは、作業結果に影響します。このワークショップでは、手順を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整してください。手順は標準を設定し、スキルは繰り返し可能なタスクを説明し、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築の前に方針を考えるには **Plan**、範囲の絞られた変更に取り組みながら状況を確認するには **Interactive**、明確に定義されたタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。*何を*、*なぜ*、*どのように*作るのかを明確に説明すると、結果が大きく変わります。

## さらに確認する

主要なワークフローを学び終えました。Copilot CLI の次の機能も確認してみてください。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を整理し、別の視点から検討します。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot クラウドエージェントにタスクを送ります。

## 次のステップ

どのツールも、使い続けることで上達します。実際のコード、趣味のコード、何年も作りたいと思っていた小さなアプリなどで活用してください。チームに学んだことを共有し、ほかの人からも学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境での作業を比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリのワークショップ][app]、または[Copilot クラウドエージェントのワークショップ][cloud]をご覧ください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
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
