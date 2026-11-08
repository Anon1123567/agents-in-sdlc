---
title: "演習 2 - GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム手順][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要求などがあります。GitHub Copilot のクラウドエージェントは、コードの更新や機能の追加などのタスクを自律的に実行できます。エージェントが作業を終えると、人間の開発者がレビューできるドラフト PR を作成します。こうした手間のかかるタスクを委任することで、開発プロセスを加速し、開発者はより大きな課題に集中できます。

Copilot クラウドエージェントを使って、次のことを学びます。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- 明確な範囲のイシューが重要な理由を理解する。
- イシューを Copilot に割り当てる。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のバージョンのサイトを開発した委託業者が、ドキュメントを適切に整備しませんでした。実際には、ドキュメントがまったくありません。最初の作業として、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのゲームを表示し、利用者がさらにゲームを見つけられるようにします。まだ完成度の高い実装は必要ありませんが、UX の受け入れテストに使える状態にしたいと考えています。この作業は現在ブロッカーですが、優先度の高い別のイシューもあります。

これらは、優先順位が下がりやすいタスクの例です。Copilot クラウドエージェントに割り当てると、非同期に作業を進めてもらえます。開発者はほかの作業に集中し、後から戻って Copilot の作業をレビューできます。

## GitHub Copilot クラウドエージェントについて

[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。たとえば、[GitHub イシューを Copilot に割り当てる][assign-issue]ことができます。割り当てられると、Copilot は進捗を記録するドラフトのプルリクエストを作成し、環境を準備して、タスクに取り組みます。作業中でも、完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法のような力はありません。考える作業を省略できるわけではありません。一見簡単そうな作業でも、詳しく調べると複雑な場合があります。[Copilot クラウドエージェントに依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、改善します。生成 AI が加わっても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム手順

これまでの演習では、カスタム手順と、それを使って Copilot にガイダンスを提供する方法を確認しました。カスタム手順の演習をすでに行っている場合は、`.github/copilot-instructions.md` が機能することを見たはずです。まだの場合は、ここで簡単に確認しましょう。Copilot クラウドエージェントに作業を割り当てる前に、このリポジトリに含まれる手順ファイルを読み取り専用で確認し、後でその影響を調べられるようにします。

リポジトリの GitHub Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターを確認します。これは `**/*.test.ts` ファイルに適用される手順です。

次のセクションで `Code lacks documentation` イシューをクラウドエージェントに割り当てたら、作成されたプルリクエストで TSDoc コメント、TypeScript の規約、コメントヘッダーを確認します。これらの内容は手順ファイルから取得されます。後の演習で PR をレビューするときに、改めて確認します。

## Copilot クラウドエージェントの開発環境を設定する

コードを作成する場合、通常は特定の開発環境やセットアップスクリプトが必要です。クラウドエージェントにタスクを割り当てる場合も同様で、エージェントはソフトウェアエンジニアのように作業します。

クラウドエージェントは、作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` ファイルで設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールと依存関係を準備できます。このラボのために、すでに設定してあります。このワークフローは、Copilot が Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一の Astro アプリ用にマイグレーションと初期データ投入が済んだ SQLite データベースを利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*の実行に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストに必要なデータを用意するため、ローカル SQLite データベースをマイグレーションして初期データを投入します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルと同じですが、重要な点がいくつかあります。

- `copilot-setup-steps` という 1 つのジョブが含まれています。このジョブは、Copilot がプルリクエストに取り組む前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するのを待たずに、正常に実行されるかテストできます。

## ドキュメントを追加する

ドキュメントが重要であることは誰もが理解していますが、多くのプロジェクトでは情報が古かったり、まったくなかったりします。このような技術的負債は放置されやすく、生産性を低下させ、コードベースの保守や新しい開発者の受け入れを困難にします。Copilot はドキュメント作成が得意なため、クラウドエージェントに割り当てるのに適したイシューです。バックグラウンドで必要なドキュメントを作成してもらい、後の演習でその作業をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しいイシューのダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択してイシューを作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

  ![Copilot をイシューに割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成された、タイトルが `[WIP]: Code lacks documentation` などのプルリクエスト (PR) を開きます。一覧に PR が見つからない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が To Do リストを作成したことを確認できます。

> [!NOTE]
> Copilot の To Do リストが PR に表示されるまで数分かかることがあります。Copilot は環境を作成（前のセクションで説明したワークフローを実行）し、プロジェクトを分析して、問題に取り組む最適な方法を検討しています。

13. リストを確認し、Copilot が実行する予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot がイシューに取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 最新の状態を確認するには、ブラウザーを更新する必要がある場合があります。

16. ライブセッションをスクロールして、Copilot が問題を解決する様子を確認します。コードを調べ、現状を理解し、方針を考えて、コードを作成する流れを確認できます。

作業には数分かかる場合があります。Copilot クラウドエージェントの主な利点は、ほかのタスクに集中している間に非同期で作業を進められることです。この利点を活かすため、もう 1 つのタスクをクラウドエージェントに割り当ててから、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

Copilot クラウドエージェントの大きな利点の 1 つは、複数の作業を分担できることです。デザインチーム向けに関連ゲームのセクションを作成する作業は長くかからないかもしれませんが、その時間を別の作業に使えます。クラウドエージェントに依頼しましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しいイシューのダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択してイシューを作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

  ![Copilot をイシューに割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後、イシューの最初のコメントに 👀 が表示され、Copilot が作業中であることを示します。

![イシューの作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の要求にも取り組んでいます。Copilot クラウドエージェントはソフトウェアエンジニアのように作業するため、常時監視する必要はありません。作業が完了してからレビューできます。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot クラウドエージェント][copilot-agents]を確認しました。クラウドエージェントにイシューを割り当て、非同期で作業させることができます。技術的負債の解消、新しい機能の作成、あるフレームワークから別のフレームワークへのコード移行などに利用できます。

次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 明確な範囲のイシューが重要な理由を確認しました。
- イシューを Copilot に割り当てました。

クラウドエージェントがバックグラウンドで作業している間に、次はカスタムエージェントの作成と使用に取り組みます。[Copilot クラウドエージェントでも MCP サーバー][cloud-agent-mcp]を使用できます。また、前のモジュールで学んだカスタム手順も利用できます。

## リソース

- [Copilot クラウドエージェントについて][copilot-agents]
- [GitHub イシューを Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
