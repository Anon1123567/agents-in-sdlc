---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "Copilot CLI でゲームカードを少し変更し、転送されたブラウザーでレビューして、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次は、プロジェクトに初めて変更を加えます。小さな変更から始めましょう。ゲームのデータには星評価がありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに評価を表示させ、変更をレビューし、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot の会話を始めます。
- 小さな変更を行うようエージェントに依頼します。
- `/diff` で変更をレビューします。
- 転送されたブラウザーでアプリを実行し、変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。しかし、ホームページのゲームカードには、タイトル、カテゴリ、発行者、説明だけが表示されます。最初の練習として、既存の評価を各カードに表示するようエージェントに依頼します。これは小規模で独立した変更なので、最初のセッションにぴったりです。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot アプリとは異なり、通常の CLI の会話では、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使用し、専用の worktree は作成しません。保存した会話は後で再開できますが、ファイルとブランチは通常の Git の状態としてディスク上に残ります。

会話では、プロンプトとエージェントの応答、ファイルを調べて編集するエージェントのツール操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を始め、変更を依頼する

新しい会話を始めて、機能の実装に取りかかります。

1. Codespace に戻ります。
2. まだターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだ起動していない場合は、次のコマンドを実行して Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を実行して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトを使って変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot がプロジェクトを確認し、ゲームの詳細を表示するファイルを探して、必要なコードを作成します。Copilot CLI を使って新しい機能を追加できました。

## 差分をレビューする

小さな変更でも、マージする前に AI が生成した変更をレビューしてください。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたファイルをすべて確認します。
2. 評価がある場合は各ゲームカードに数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. どちらの状態もテストで確認されていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> すべての生成 AI ツールと同様に、Copilot の応答は確率的であり、決定論的ではありません。生成されるコードは異なる場合があります。特定の実装を期待するのではなく、動作をレビューしてください。

## 変更を確認する

コードを読んだだけで動作すると判断せず、Copilot に Web サイトを起動させ、Codespaces で転送されたブラウザーから更新後の UI を確認します。

1. Copilot に次のプロンプトを送り、アプリを起動します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. ゲームカードに 5 段階評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを開いてマージする

機能を作成できました。次は、プロジェクトに変更をマージするためにプルリクエスト (PR) を作成します。

1. 既定のエージェントに変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR を作成するときに既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI が表示した URL を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して PR を開きます。
4. 変更されたファイルとチェックを確認します。
5. 問題がなければ **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` を使って Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI を使って、最初の変更をリリースしました。具体的には、次のことを行いました。

- 機能ブランチで Copilot の会話を始めました。
- ゲームカードに小さな変更を加えるようエージェントに依頼しました。
- `/diff` で変更をレビューしました。
- 転送されたブラウザーでアプリを実行し、星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は、[フィルター機能のイシューから始め、Plan と Autopilot モードを使って][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
