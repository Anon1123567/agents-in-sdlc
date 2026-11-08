---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

ここまでで説明したとおり、コードを書くことだけが開発ではありません。データや外部サービスを使い、Copilot で追加の自動化も利用できるようにする必要があります。MCP サーバーを使うと、Copilot CLI に組み込まれている機能を超えて、さらに多くのツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びます。
- Playwright MCP サーバーが利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

ユニットテストやエンドツーエンドテストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot にユーザーと同じように開発中の Web サイトを使わせることで、変更が期待どおりに動作する確信を高め、作業の自動化をさらに進めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部ツールやサービスとリアルタイムに通信できます。最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

ツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。各 MCP サーバーは、AI エージェントが利用できるツールとリソースを提供します。

よく使われている MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、イシュー、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を利用できます。

ほかにも多くの MCP サーバーがあります。GitHub がホストする [MCP レジストリ][mcp-registry]で、エコシステム内のサーバーを見つけたり、登録に貢献したりできます。

> [!CAUTION]
> MCP サーバーは、プロジェクトのほかの依存関係と同じように扱ってください。使用前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるように追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、セットアップを飛ばして次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認して <kbd>Enter</kbd> を押し、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは、機能が期待どおりに動くことを手動で確認しました。次は、Copilot の Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は MCP サーバーの使用を Copilot に指示しなくても、Copilot が適切に判断します。ここでは使用するサーバーが分かっているため、明示すると結果が安定し、トークンも節約できます。

2. Copilot がブラウザーで機能を確認するために実行する各手順を確認します。
3. 結果を読み、すべてが期待どおりに動作することを確認します。

Copilot がサーバーを起動し、Playwright で Web サイトを操作して、サーバーを停止した後、結果を報告します。

## まとめと次のステップ

おめでとうございます。Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。ここでは、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びました。
- Playwright MCP サーバーが利用できない場合に追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次は、スキルとブラウザーツールを専門的な役割として組み合わせる [QA カスタムエージェントを作成][next-lesson]します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
