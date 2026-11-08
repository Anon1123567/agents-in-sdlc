---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウド上で非同期に作業できます。GitHub で作業を割り当てると、クラウドエージェントはバックグラウンドでリポジトリを調べ、変更を行い、プルリクエストを作成します。その間、ほかの作業を続けられます。

各演習では、まずクラウドエージェントが従うカスタム手順を追加し、次に GitHub イシューを割り当てて実装を依頼します。カスタムエージェントを使って方針を整え、エージェントのダッシュボードからセッションの進捗を監視して指示を追加し、最後にプルリクエストをレビューして結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタム手順][ex1] | コンテキスト | クラウドエージェントが従うカスタム手順を追加します |
| [2. クラウドエージェント][ex2] | 非同期エージェント | イシューを Copilot クラウドエージェントに割り当てます |
| [3. カスタムエージェント][ex3] | 特化エージェント | カスタムエージェントを確認して使用します |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視して指示します |
| [5. 反復改善][ex5] | レビュー | PR をレビューし、Copilot の作業を改善して次の手順を選びます |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランがある GitHub アカウント
- [ ] アカウントまたは組織で Copilot クラウドエージェントが有効になっていること

> [!TIP]
> 有料プランをお持ちでない場合、認定された学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、どのハーネスも最後まで完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> クラウドエージェントは **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free は対象外です。Copilot Business または Enterprise の場合、組織の管理者が有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス][cli]または [VS Code ハーネス][vscode]をご覧ください。

## 開始する

**[演習 0: 前提条件から開始する →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli]: ../cli/
[vscode]: ../vscode/
