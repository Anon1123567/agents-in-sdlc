---
title: "演習 3 - カスタムエージェント"
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

この演習では、次のことを行います。

- カスタムエージェントの定義方法を確認します。
- イシューをカスタムエージェントに割り当てます。

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

## イシューを作成して割り当てる

ミッションコントロールは、環境内のエージェントと作業を管理する場所です。Copilot クラウドエージェントにタスクを割り当てたり、タスクを監視したり、方針を変更したりできます。まず、Copilot に高コントラストモードを作成するタスクを割り当てましょう。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しいイシューのダイアログを開きます。
4. **Blank issue** を選択して、空のイシューを作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次のテキストを入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択してイシューを作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てのダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![クラウドエージェントの割り当て画面で、カスタムエージェントとアクセシビリティエージェントを選択したスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、Copilot が特定の種類の作業を効果的に行えるようにします。

ここでは、次のことを行いました。

- カスタムエージェントの定義方法を確認しました。
- エージェントにタスクを割り当てました。

Copilot が高コントラストモードの実装に取り組んでいる間に、次はミッションコントロールから[エージェントセッションを監視して指示を追加][next-lesson]します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使用する準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot クラウドエージェント][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
