---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "イシューの要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` スキルを使って自動チェックを実行し、Playwright MCP を使ってブラウザーでフィルター機能を確認しました。次は、これらの機能を明確な QA プロセスを持つカスタムエージェントにまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが手順、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選び、フィルター機能のイシューに基づく結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザー上の動作を一貫して確認したいと考えています。カスタムエージェントを使うと、この QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の特化版です。プロファイルにはエージェントの目的、手順、使用可能なツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまで使用したカスタマイズには、それぞれ異なる役割があります。リポジトリの手順はチームの標準を説明します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、Copilot がこれらの機能を要件の評価や結果の報告にどう使うかを示します。これらの機能に代わるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルでは、QA が実行するチェックと、その作業の制約を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA の手順と制約を反映したことを確認します。これにより、機能を検証するだけのつもりなのに、不完全または範囲の広すぎるエージェントに変更されることを防げます。

1. `/diff` を入力して `.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は任意ですが、含めるとエージェントの表示名が明確になります。
3. プロファイルの手順を読み、QA が要件を起点とし、リポジトリの手順に従って `quality-checks` スキルを実行し、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更をコミットしたりプルリクエストを開いたりしないことを確認します。
5. これらの役割や制約が不足している場合は、続行する前に既定のエージェントに修正を依頼します。

## イシューに対して QA を実行する

Copilot CLI は、会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルター機能の会話を再開し、QA を選択すると、すでにコンテキストに含まれるイシューと計画上の決定を利用できます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、利用可能なエージェントの一覧にまだ表示されない場合があります。次のコマンドで有効にできます。

2. 次のプロンプトで QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、生成されたレポートを確認します。

## まとめと次のステップ

再利用可能な専門家の役割を追加し、その作業をレビューしました。このレッスンでは、次のことを行いました。

- カスタムエージェントが手順、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選び、フィルター機能のイシューに基づく結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートの準備ができました。次は、[機能 PR を作成し、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
