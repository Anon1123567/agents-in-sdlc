---
title: "演習 5: GitHub Copilot の作業を反復改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、GitHub Copilot を使ってユーザー体験の改善と機能追加を行う複数のタスクに取り組みました。コードにドキュメントを追加し、デザインチームが改善できる関連ゲーム機能を構築し、高コントラストとライトモードの切り替えなどのアクセシビリティ機能を実装しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善してから、チームでのレビューを有効にします。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。GitHub Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能をレビューしてから、チームのレビューを有効にします。

## GitHub Copilot クラウドエージェントのセキュリティ

Copilot クラウドエージェントは非同期で監督なしに作業するため、安全性を維持するためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセスと、作業用のブランチに対する**書き込みアクセスのみ**を持ちます。
- クラウドエージェントは GitHub Actions 内で実行され、作業用に分離された一時的な環境を作成します。
- GitHub Actions のワークフローを実行するには、人間の承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot クラウドエージェントが作成した、コードへのドキュメント追加に関するプルリクエスト (PR) を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot クラウドエージェントのファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに更新されたコードと、追加された TSDoc のドキュメントコメントなどを確認します。具体的な変更内容は異なる場合があります。

   TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーに注目してください。これらは演習 2 の最初に確認したカスタム手順ファイルから取得されています。

6. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す通知を確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションでワークフローがキューに追加されたことを確認します。単一の Astro アプリのプロジェクトチェックがすべて成功するはずです。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業する方法は、一方的なものではありません。チームメンバーと同じように、プルリクエストやコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確率的に変わるため、具体的な依頼文は指定できません。Copilot に依頼できる変更の例は次のとおりです。

- 各コードファイルの先頭に、役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 生成されたドキュメントへの変更を依頼するコメントを追加し、他の利用者と同様に **@copilot** をメンションします。上記の例や、コードベースのドキュメントで更新したい内容を指定してください。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更をレビューします。

満足するまで改善を繰り返してください。準備ができたら、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示するイシューに対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す通知を確認します。
7. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェックセクションでワークフローがキューに追加されたことを確認します。単一の Astro アプリのプロジェクトチェックがすべて成功するはずです。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動テストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します（`<branch-name>` を Copilot が作成したブランチ名（たとえば **copilot/fix-8**）に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot クラウドエージェントに変更を依頼して反復改善できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc のドキュメントコメントが追加されていることを確認したりできます（このイシューはカスタム手順を更新する**前**に割り当てています）。**Conversation** タブに新しいコメントを追加すると、Copilot がコメントを確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼した高コントラストモードと、演習 4 でミッションコントロールから依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。その場合は、Copilot が作業を完了するまで数分待ちます。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モード切り替え用の UI コンポーネント
   - 利用者の設定を保存するためのローカルストレージの使用
   - 高コントラストとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す通知を確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションでワークフローがキューに追加されたことを確認します。単一の Astro アプリのプロジェクトチェックがすべて成功するはずです。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動テストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します（`<branch-name>` を Copilot が作成したブランチ名に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動してブラウザーで高コントラストとライトモードの切り替えをテストし、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに従って機能を実装したことを確認してください。アクセシビリティ上の問題や改善点があれば、前の PR と同様にコメントで **@copilot** をメンションして変更を依頼できます。

## 任意の演習 — 委任を続ける

クラウドエージェントには、実際のバックログのタスクを任せ、その間に別の作業に取り組む方法が適しています。この習慣を身に付けるために、リポジトリにいくつかイシューを登録して Copilot に割り当ててください。たとえば次のようなタスクがあります。

- ゲーム詳細ページに支援者向けの関心フォームを追加する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

クラウドエージェントのハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタム手順ファイルを確認し、後でクラウドエージェントの出力に反映される様子を確認しました。**
- **Copilot クラウドエージェントにイシューを割り当て、環境のセットアップ、計画、非同期の実行を確認しました。**
- **アクセシビリティ用のカスタムエージェントを作成して使い、高コントラストとライトモードの切り替えを追加しました。**
- **Copilot エージェントのページをミッションコントロールとして使い、アクセシビリティセッションを監視して途中で指示を追加しました。**
- **Copilot のプルリクエストをレビューして改善し、@copilot をメンションして変更を依頼し、ワークフローを承認しました。**

## レビューと次のステップ

クラウドエージェントのハーネスを完了しました。ワークショップはここで終了できます。

Copilot エージェントの機能についてさらに学ぶ場合は、別のハーネスで異なる環境のシナリオを確認してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat の Agent Mode と MCP 統合を利用します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、クラウドエージェントへの任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つ実装します。

自分のリポジトリで、次の作業を試すこともできます。

- リファクタリングやテストカバレッジのイシューをクラウドエージェントに割り当てる。
- 別の分野を対象にしたカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub イシューを Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot クラウドエージェントのファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
