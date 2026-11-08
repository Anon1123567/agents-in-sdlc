---
title: "レッスン 4 - カスタム手順で Copilot を導く"
description: "リポジトリの手順を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。作業の進め方に決まりがある場合は、そのガイダンスを Copilot が参照できるようにします。[手順ファイル][instruction-files]では、どのようなコードを作成するかだけでなく、どのような構成にするかも指定できます。フィルター機能を実装したので、Copilot が参照した手順を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの手順とパスを指定した手順ファイルがエージェントに適用される方法を確認します。
- コーディング標準に従うよう、手順ファイルを更新します。
- 手順ファイルがコードに与える影響を確認します。

## シナリオ

優れた開発会社と同様に、Tailspin Toys には開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- コメントにはコードの内容を繰り返すのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記載します。該当する場合は、注入可能な `db` 引数についても記載します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連するコードの変更に合わせてコメントを最新の状態に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

手順ファイルを使って、Copilot がこれらの開発手法に沿って作業できるよう、必要な情報を提供します。

## 手順ファイル

カスタム手順を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを生成できるようにします。リポジトリ全体、または特定の種類のファイルやタスクを対象に、コーディング規約、ライブラリ、コメントの書き方などを指定できます。

手順ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**要求で Copilot に送信される単一の手順ファイルです。ほとんどの要求に関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定の言語、ファイル種別、タスクに関するガイダンスを提供します。

> [!NOTE]
> ほかの形式の手順ファイルへの対応は、環境によって異なります。特定の形式を使用する前に、[カスタム手順のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム手順ファイルを確認する

スタータープロジェクトには、すぐに使えるように手順ファイルが含まれています。変更前に既存の内容を確認し、手順の影響を見てみましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングガイダンスを確認します。この手順は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの手順があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで、手順が適用されるファイルを指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの手順ファイルや既存プロジェクトファイルへのリンクを確認します。大きな手順を再利用可能な小さなファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて手順ファイルを更新する

既存のファイルは良い出発点ですが、追加したばかりの TypeScript コードには TSDoc コメントが必要です。中核となる `copilot-instructions.md` ファイルを変更して、このルールを追加しましょう。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は、会話を開始したときにリポジトリの手順を読み込みます。更新した手順を保存したら、フィルター機能の会話を終了して再開します。これにより、機能に関するコンテキストを保ったまま、新しいガイダンスを読み込めます。

1. フィルター機能の会話を開いたままの場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新後のガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを読みます。新しく追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が手順ファイルからコンテキストを取得する方法を確認し、機能に新しい標準を適用しました。具体的には、次のことを行いました。

- リポジトリの手順とパスを指定した手順ファイルがエージェントに適用される方法を確認しました。
- コーディング標準に従うよう、手順ファイルを更新しました。
- 手順ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行][next-lesson]し、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI のカスタム手順を追加する][instruction-files]
- [カスタム手順のサポート][custom-instructions-support]
- [カスタム手順を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 手順ファイルやその他のリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
