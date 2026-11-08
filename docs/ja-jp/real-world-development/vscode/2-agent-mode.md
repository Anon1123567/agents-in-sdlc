---
title: "演習 2 - Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

最も簡単なアプリの更新でも、通常は複数ファイルを修正し、テストを実行するなどの作業が必要です。開発者は必要なファイルを探して変更し、テストを実行し、問題をデバッグして、修正し忘れたファイルがあればさらに更新する、といった作業を繰り返します。

ここで役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、IDE 内でより自律的に作業します。開発者と同じように、まず既存のプロジェクト構造を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使って、Web サイトに機能を追加してみましょう。

> [!NOTE]
> 名前は似ていますが、Agent Mode とクラウドエージェントは異なるユーザー体験を提供します。Agent Mode は IDE 内で作業し、すばやいフィードバックと対話を可能にします。クラウドエージェントは、イシューやプルリクエストを介してやり取りする、チームメンバーのような非同期のエージェントです。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを見つけて、複数ファイルを連携して変更する方法。
- GitHub Copilot Agent Mode が UI とデータ層にまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、利用者がカテゴリと発行者で絞り込めるようにします。前の演習では発行者のヘルパーを追加しました。次は Copilot Agent Mode を使い、残りのデータ層、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者向けテーマのボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。Web サイトは単一の Astro アプリで、ビルド時に静的 HTML を生成します。ページはフロントマターから Drizzle ORM を使ってローカルの SQLite データベースを直接参照します。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、操作性は標準的な DOM API を使う小規模な Astro `<script>` で追加します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習から `main` ブランチにいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは、Web サーバーを実行している Codespace へのプライベートなトンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明とともにボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行者、カテゴリなどの詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、さらに機能を追加します。最初にバックログを確認しましょう。テンプレートからプロジェクトを設定すると、イシューのバックログも自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。**Agent** は複数ファイルを対象に計画と実装を行い、ターミナルコマンドを実行したりツールを呼び出したりできます。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップの作成には Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高めることが目的ですが、LLM は確率的に動作するため、記載内容と異なる提案が生成されることがあります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは同様に完了できます。

3. 次のプロンプトを Copilot に送信して、バックログのイシューを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべてのイシューを一覧表示するコマンドを実行します。
5. 生成されたイシューの一覧を確認します。

Copilot が利用者にとって有益な順に優先順位も付けてくれたことに注目してください。

## 手順ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを与える手順ファイルを確認します。Web サイトの機能追加の進め方を説明する [ユーザーインターフェイス (UI)][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能を追加するときの全体的なガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle などのファイル種別に関する手順へのリンク

> [!TIP]
> 手順ファイルから別の手順ファイルやプロジェクト内のファイルを参照できます。パスは手順ファイルを基準とした相対パスです。これにより、手順を再利用しやすい小さなファイルに分け、例やテンプレートを参照させることができます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも 3 つの更新が必要です。

- データ層 (`src/lib/`) にフィルター処理を追加または調整します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

さらに、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot Agent Mode にこれらの作業を任せます。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、手順ファイルを追加します。

   ![UI 手順ファイルを選択する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログにある関連イシューに従って実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを確認し、機能に必要なファイルを探します。データ層のヘルパー、UI、テストを見つけ、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトファイルを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによっては、数分かかります。作業中、エディターに **Keep** と **Undo** ボタンが表示されることがあります。Copilot の作業中に選択する必要はありません。完了すると、すべての変更に対して **Keep** または **Undo** が表示されます。

7. Copilot に促されたら **Continue** を選択してテストを実行します。

   ![Copilot Chat がテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストに失敗したりすることがあります。問題ありません。Copilot はエラーを検出しなくなるまで、コード生成とテストの実行を繰り返します。

   ![Copilot Agent Mode の Chat セッションが完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot や他の AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを実行中のブラウザーに戻り、新しい機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加しました。次の方法を学びました。

- GitHub Copilot Agent Mode が UI とデータ層にまたがる新機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを見つけて、複数ファイルを連携して変更する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを開きます。

### ボーナスの演習 - ページネーションを実装する

ゲーム一覧が増えるにつれて、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを実装するよう依頼します。次の点を検討してください。

- 既存の手順ファイルを使うなど、ベストプラクティスに従います。
- ページサイズを利用者が選択できるようにするか、固定にするかなど、ページネーションの方法を検討します。
- 希望どおりの実装になるよう、Copilot に必要なガイダンスをプロンプトに含めます。
- Copilot に変更を依頼したりコンテキストを追加したりして、何度かやり取りすることがあります。これは Copilot と作業するときの通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
