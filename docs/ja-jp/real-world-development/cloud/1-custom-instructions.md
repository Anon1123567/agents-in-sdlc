---
title: "演習 1 - カスタム手順（クラウドエージェント）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot クラウドエージェント →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業の進め方に決まりがある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。[手順ファイル][instruction-files]を使うと、何をしてほしいかだけでなく、どのように作業してほしいかも Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム手順とパスを指定した手順ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code および CLI のハーネスとは異なり、ここではプロンプトの変更前後を比較しません。Copilot クラウドエージェントは GitHub イシューを非同期に処理するため、リアルタイムで並べて効果を示すのは困難です。このハーネスの後半で、エージェントが作成したプルリクエストをレビューするときに、手順ファイルの影響を確認します。

## 手順ファイル

### シナリオ

優れた開発会社と同様に、Tailspin Toys には開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ層には必ずユニットテストを用意します。
- UI はダークモードにし、現代的な印象にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

手順ファイルを使って、Copilot がこれらの開発手法に沿って作業できるよう、必要な情報を提供します。

### カスタム手順

カスタム手順を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを生成できるようにします。リポジトリ全体、または特定の種類のファイルやタスクを対象に、コーディング規約、ライブラリ、コメントの書き方などを指定できます。

手順ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**要求で Copilot に送信される単一の手順ファイルです。技術スタック、開発中のものの概要、ベストプラクティス、全般的なガイダンスなど、ほとんどのチャットや CLI 要求に関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類に使用します。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの作成などを対象にガイダンスを提供できます。

> [!NOTE]
> IDE では、手順ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot クラウドエージェントは、リポジトリレベルの手順ファイルと、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルを使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の手順ファイルにも対応しています。詳細は[サポート対象の手順ファイル][custom-instructions-support]をご覧ください。

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
2. プロジェクトの簡単な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内には、**GitHub Actions Workflows** のガイダンスもあります。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの手順があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで、リポジトリルートからの相対パスで対象ファイルを指定しています。ここでは、`**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の手順を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` など、ほかの手順ファイルやプロジェクト内の既存ファイルへのリンクがあります。大きな手順を再利用可能な小さなファイルに分け、Copilot が参考にできる例を示す方法が分かります（ここでのパスはリポジトリルートではなく、手順ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメント化はまだ求めていません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーコメントのルールを追加します。

## リポジトリ標準を追加する

ここで行う編集は、コード内のドキュメントを TSDoc のドキュメントコメントとファイルレベルのヘッダーコメントで記載するという、プロジェクト全体のルールを追加することです。次の演習でイシューを割り当てると、クラウドエージェントがこのルールを参照します。

編集の前に、作業用のブランチを作成します（クラウドエージェントはイシューの対象ブランチにある手順を読み込みますが、ここでは変更をきちんとコミットします）。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に提供します。コード提案の改善に役立つよう、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` を開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在はコーディング標準が記載されていますが、コード内のドキュメント化のルールはありません。
3. 既存の標準の直後に、次の Markdown を追加し、ファイルヘッダーと TSDoc コメントを Copilot に作成させます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したように、リポジトリ全体のガイダンスには `.github/copilot-instructions.md` を、特定の言語、ファイル種別、タスクには `*.instructions.md` を使えます。ドキュメントコメントのルールのようなプロジェクト全体の標準は、リポジトリレベルのファイルに記載します。

## 手順の変更をコミット、プッシュ、マージする

クラウドエージェントは、イシューの対象ブランチにある手順ファイルを読み込みます。次の演習でイシューを Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、クラウドエージェントがこの手順を使えるように、変更を `main` にマージする必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習でクラウドエージェントがイシューに取り組むとき、この手順を読み込めるようになります。

> [!TIP]
> このワークショップでは、ブランチを使わず `main` に直接作業してもかまいません。ここでブランチを作成するのは、実際のプロジェクトでの手順を再現するためです。

## まとめと次のステップ

Copilot が手順ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの後半で、クラウドエージェントが作成するプルリクエストを通して、その標準が適用される様子を確認します。

次は、[Copilot クラウドエージェントに最初のイシューを割り当て][next-lesson]ます。

## リソース

- [GitHub Copilot のカスタマイズに使う手順ファイル][instruction-files]
- [カスタム手順を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けにより良いカスタム手順を作成する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 手順ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
