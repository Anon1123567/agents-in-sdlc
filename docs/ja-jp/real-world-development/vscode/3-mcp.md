---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent Mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

Agent Mode でフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーで機能をテストさせます。その後、ブランチを公開して PR を開きます。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルター機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルター機能のプルリクエストを開きます。

## Model Context Protocol (MCP) とは

Model Context Protocol (MCP) を使うと、エージェントはエディターの外にあるツールにもアクセスできるため、Agent Mode の機能が大きく広がります。MCP は、Copilot が外部ツールやサービスと通信するための標準的な方法です。

![Agent Mode の内部構成と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信する手段を提供します。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムに通信し、リソースを使って最新情報にアクセスしたり、ツールを使って利用者に代わる操作を実行したりできます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。MCP サーバーは、AI エージェントと外部ツール（既存の API や NPM パッケージなどのローカルツール）との通信を管理します。MCP サーバーごとに、AI エージェントが利用できるツールとリソースが異なります。

よく使われている MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。新しいリポジトリの作成、既存リポジトリの更新、イシューやプルリクエストの管理などをエージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などをエージェントが実行できます。

ほかにも多くの MCP サーバーがあります。GitHub がホストする [MCP レジストリ](https://github.com/mcp)で、エコシステム内のサーバーを見つけたり、登録に貢献したりできます。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクトのほかの依存関係と同じように扱ってください。使用前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトのテンプレートに含まれています。そのため、前の演習で Copilot がバックログのイシューを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が最初にツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次は、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと Copilot がブラウザーを操作できるため、作成した機能をテストできます。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code がマシン上で `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、表示ウィンドウなしで Playwright を実行します。デスクトップ環境がない Codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンドテストに Playwright をすでに使用しているため、必要なブラウザーもインストール済みの場合がほとんどです。Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行して、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。初めて Copilot がサーバーのツールを使うときに VS Code が起動し、サーバーを信頼するかどうかを尋ねます。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. サーバーを信頼するよう VS Code に求められた場合は、構成を確認して信頼します。

手動で起動する必要はありません。スキップした場合は、次の手順で Copilot が初めてツールを使用するときに VS Code がサーバーを起動し、信頼するかどうかを確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して確認する代わりに、エージェントにページを開かせ、移動してフィルターを適用し、結果を読み取らせて、確認内容をまとめてもらえます。会話を離れずに機能を確認する手軽な方法です。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使います。エージェントは、支援技術と同じように、ボタン、リンク、リスト項目などの構造化されてラベル付けされた要素を基に動作します。そのため、簡単な機能確認がアクセシビリティの基本確認にもなります。

サーバーを接続してアプリを起動したら、次のプロンプトを Copilot に送り、フィルター機能を試させます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、手順に沿って確認した結果を報告します。結果をイシューの受け入れ条件と照らし合わせます。問題がある場合は、プルリクエストを開く前に質問したり、コードを修正させたりしてください。

> [!NOTE]
> テストするには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときにブラウザーのダウンロードが必要になる場合があります。ブラウザーがないと報告された場合は `npx playwright install chromium` を実行して、もう一度試してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したら、チームでレビューするためにプルリクエスト (PR) を作成します。最初に `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージングします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してリポジトリにブランチをプッシュします。

## プルリクエストを作成する

プルリクエストは github.com や GitHub コマンドラインインターフェイス (CLI) などで作成できます。ここでは、GitHub Copilot に作成を依頼します。関連するイシューを探し、そのイシューと関連付けて PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを始めます。
2. エージェントのドロップダウンで、GitHub ツールを使えるように **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の取得や操作を実行できるようにします。GitHub ツールを初めて使うときに認証を求められた場合は、画面の指示に従います。
5. Copilot がイシューを検索して適切なものを見つけ、PR を作成したことを確認します。
6. Copilot が生成したリンクを選択して PR をレビューします。まだマージしないでください。

## まとめと次のステップ

おめでとうございます。この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルター機能を手動テストするために使用しました。
- ブランチを公開し、フィルター機能のプルリクエストを開きました。

MCP サーバーを使って機能をテストしましたが、Copilot を拡張する方法は MCP だけではありません。次は、アクセシビリティレビューなどの作業に役立つ[カスタムエージェントを作成][next-lesson]します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP レジストリ][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
