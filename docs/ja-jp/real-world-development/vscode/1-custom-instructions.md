---
title: "演習 1 - カスタム手順（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent Mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業の進め方に決まりがある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot にコード生成の指針を伝えるには、[手順ファイル][instruction-files]が重要です。

この演習では、次のことを行います。

- リポジトリのカスタム手順とパスを指定した手順ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 既存の手順を使い、フィルター機能の最初のデータ部分（発行者ヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度実行し、生成コードが新しい標準に従う様子を確認します。
- 手順の更新とフィルター機能の最初の変更を `main` にコミットし、次の演習で Copilot が更新されたガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが標準の一部に従わない場合があります。Copilot は確率的に動作します。この演習では、更新後の手順による行動の「傾向」を確認します。出力が文字単位で同じになることは期待しないでください。

## 手順ファイル

### シナリオ

優れた開発会社と同様に、Tailspin Toys には開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ層には必ずユニットテストを用意します。
- UI はダークモードにし、現代的な印象にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

手順ファイルを使って、Copilot がこれらの開発手法に沿って作業できるよう、必要な情報を提供します。

### カスタム手順

カスタム手順を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを生成できるようにします。プロジェクト全体、または特定の種類のファイルやタスクを対象に、コーディング規約、ライブラリ、コメントの書き方などを指定できます。

手順ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**要求で Copilot に送信される単一の手順ファイルです。技術スタック、開発中のものの概要、ベストプラクティス、全般的なガイダンスなど、ほとんどのチャットや CLI 要求に関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類に使用します。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの作成などを対象にガイダンスを提供できます。

> [!NOTE]
> IDE では、手順ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot クラウドエージェントは、リポジトリレベルの手順ファイルと、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルを使用します。
>
> AGENTS.md や CLAUDE.md など、ほかの標準の手順ファイルにも対応しています。詳細は[カスタム手順のサポート][custom-instructions-support]をご覧ください。

### 手順ファイルを管理するためのベストプラクティス

手順ファイルの作成について詳しく説明することは、この演習の範囲外です。ここでは、サンプルプロジェクトの例から、基本的な方法を確認します。

- `copilot-instructions.md` には、構築するものの説明、プロジェクトの構造、全体的なコーディング標準など、プロジェクト全体に関するガイダンスを記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層などのファイル種別や、特定のタスクに関する指示を記載します。
- 自然な言葉を使い、明確なガイダンスと、コードの例や反例を示します。

AI の使い方に唯一の正解はありません。プロジェクトで試しながら、最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトには、十分な手順ファイルを用意することをお勧めします。このプロジェクトのファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けの手順が見つかります。
>
> Copilot に手順ファイルの生成を依頼することもできます。手順の生成方法は利用する環境によって異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init`）。必要な場合は、利用中の環境向けのレッスンで説明します。
>
> テンプレートや出発点をお探しですか。[awesome-copilot][awesome-copilot]では、手順ファイル、カスタムエージェント、その他のリソースを確認できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム手順ファイルを確認する

このリポジトリに含まれる手順ファイルを確認します。中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内には **GitHub Actions Workflows** のガイダンスもあります。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データ層、テストなどの手順があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで、リポジトリルートからの相対パスで対象ファイルを指定しています。ここでは、`**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. ユニットテストを作成する際の手順を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` など、ほかの手順ファイルやプロジェクト内の既存ファイルへのリンクがあります。大きな手順を再利用可能な小さなファイルに分け、Copilot が参考にできる例を示す方法が分かります（ここでのパスはリポジトリルートではなく、手順ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメント化はまだ求めていません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーコメントのルールを追加します。

## 変更用ブランチを作成する

ここでは、ベストプラクティスに従って変更用ブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドを実行し、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 手順を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

手順ファイルの影響を確認するため、まず現在の手順ファイルを使ってプロンプトを送信します。その後、手順を更新して、同じプロンプトをもう一度送信します。

1. 前の演習で開いたエディターのタブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置するファイルを Copilot に知らせます。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、Copilot がファイルを編集できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを確認して変更を適用します。ヘルパーファイルとテストなど、複数のファイルに変更が及ぶ場合があります。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを取り、型付きの発行者の配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層に関する規約に従っています。
7. 生成されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。ここでは、手順を更新した後に動作の一貫性が改善されることを確認します。

## リポジトリ標準を追加する

前述のとおり、`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に提供します。コード提案の品質を高めるため、リポジトリのコーディング標準を更新します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在はコーディング標準が記載されていますが、コード内のドキュメント化のルールはありません。
3. 既存の標準の直後に、次の Markdown を追加し、ファイルヘッダーと TSDoc コメントの追加を Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したように、リポジトリ全体のガイダンスには `.github/copilot-instructions.md` を、特定の言語、ファイル種別、タスクには `*.instructions.md` を使えます。ドキュメントコメントのルールのようなプロジェクト全体の標準は、リポジトリレベルのファイルに記載します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して新しい会話を始めます。
2. `src/lib/publishers.ts` を選択し直し、Copilot が対象ファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で使う機能の基礎となるコードに適用しました。

## 手順をコミットしてブランチをプッシュする

手順ファイルも、ほかの資産と同じようにソース管理で管理します。変更した手順とヘルパーのコードをコミットして、リポジトリにブランチをプッシュします。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 次のコマンドを実行し、手順とヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. 変更された手順とヘルパーの基礎コードをステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの関連ファイルが追加で表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. リポジトリにブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準のイシューに関連付けてプルリクエストを作成します。手動で行う代わりに、Copilot にイシューを探してもらい、解決する PR を作成して、マージを依頼します。

1. Codespace 内の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントのドロップダウンから **Agent** が選択されていることを確認します。
3. 手順ファイルの更新と発行者ヘルパーの基礎コードの両方を説明する PR を現在のブランチから作成するよう、Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot に GitHub 上の操作を許可するよう求められた場合は、コマンドを確認して必要なものを **Allow** します。
5. PR が作成されたら、次のプロンプトでマージを依頼し、ローカルのブランチを `main` に戻します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub の操作やシェルコマンドを許可するよう求められた場合は、コマンドを確認して必要なものを **Allow** します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が手順ファイルからコンテキストを取得する方法を確認し、Copilot Chat を使って次の作業を行いました。

- 既存の手順を使ってコード生成を依頼し、Copilot の提案を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトをもう一度実行し、新しい標準に従ったコードが生成されることを確認しました。
- 次の演習で機能を実装できるよう、手順とヘルパーの基礎コードを `main` にコミットしました。

次は、[Agent Mode][next-lesson]で手順を活用し、Copilot にコードベース全体にまたがる機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使う手順ファイル][instruction-files]
- [カスタム手順を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けにより良いカスタム手順を作成する 5 つのヒント][copilot-instructions-five-tips]
- [個人用の GitHub Copilot カスタム手順][personal-instructions]
- [Awesome Copilot — 手順ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
