---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義すると、特定の種類の作業を効果的に行うための手順、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に提供できます。チームの専門知識を再利用可能なエージェントとしてまとめることもできます。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティエージェント、安全なコーディングを行うセキュリティエージェント、一貫したテストパターンを守るテストエージェントを作成できます。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダーか、`~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、手順を定義する Markdown のプロンプトを記述します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割の重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一部としてスキルやほかのエージェントを調整できます。このラボでは、アクセシビリティのカスタムエージェントがアクセシビリティガイドラインに沿ってサイトをレビューして更新します。その作業の一環として、プルリクエストのワークフローやテストの実行・管理を行うスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI のほかの活用方法と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタムエージェントを使って次のことを行います。

- カスタムエージェントの定義方法を確認します。
- カスタムエージェントにタスクを割り当てます。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディングプラットフォームを使えるようにしたいと考えています。最近の利用者のフィードバックによると、文字と背景のコントラストが不十分なため、現在のダークテーマを読みづらいと感じる人がいます。このアクセシビリティの課題に対処するため、デザインチームから、利用者が切り替えられる高コントラストモードの実装が求められました。

アクセシビリティは重要なため、この機能をできるだけ早く実装します。カスタムエージェントを使って機能を生成します。

## アクセシビリティのカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、あらかじめ作成されています。内容を確認して、Copilot のガイド方法を理解しましょう。

Codespace に戻り、アクセシビリティのカスタムエージェントファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドが含まれる YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` は、カスタムエージェントに必須のフロントマターです。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主な役割
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題と避けるべきこと

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビューの下部にあるエージェントのドロップダウンには、`.github/agents` に定義されたすべてのカスタムエージェントが表示されます。カスタムエージェントを選ぶと、そのエージェントの手順とツールに合わせてチャットセッションが設定されます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェントのドロップダウンで、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にあるエージェントのドロップダウンを選択します。現在アクティブなエージェントが表示されます。既定では **default** が選択されています。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティエージェントに次のプロンプトを送ります。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat が作業を開始し、コードベースを調べて、変更を提案してプロジェクトファイルに適用します。各編集はファイルパスとレビューできる差分とともにチャット内に表示されます。
6. 編集が適用されると、チャットの **Files changed** の表示が更新され、このセッションで変更されたファイルが確認できます。

> [!NOTE]
> この作業には数分かかります。Copilot は Astro コンポーネント、CSS、関連テストなどのファイルを実際に変更します。

次の演習では、作業中のセッションをレビューし、追加の指示を出します。

## まとめと次のステップ

この演習では、特定のタスクや分野に合わせた GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、Copilot が特定の種類の作業を効果的に行えるようにします。

ここでは、次のことを確認しました。

- カスタムエージェントの定義方法。
- Copilot Chat の Agent Mode でカスタムエージェントを使う方法。

次は、[エージェントの作業を監視して指示を追加し][next-lesson]、変更をレビューしながら同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントを監視、管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
