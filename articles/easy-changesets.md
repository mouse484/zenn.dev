---
title: 'Changesetsで頑張らないリリース（モノレポ対応）'
emoji: '🚀'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['changesets', 'npm', 'monorepo']
published: true
---

バージョン管理、リリースの管理に[Changesets](https://github.com/changesets/changesets)がいいらしいと聞いたけどなんか取っつきにくい感じだったので簡単に使う方法のメモ。
モノレポ対応しています。

このあたりでおすすめされている
https://turbo.build/repo/docs/handbook/publishing-packages/versioning-and-publishing
https://pnpm.io/ja/using-changesets

## 以下の説明をこのまま使うための前提条件

- GitHub を使う
- npm にリリースする
- monorepo でも可

各説明に関連のドキュメントを貼っておくので違う場合はドキュメントを見て参考にしてください。

### 知っておくといいこと

Changesets は変更に対してフラグをつけてそのフラグをまとめてリリースしたり、チェンジログを作ったりします。
このフラグの概念がちょっと分かりにくいので頭の片隅においておいてください。

## 1. インストール

```
yarn add -D @changesets/cli
```

```
yarn changeset init
```

https://github.com/changesets/changesets/blob/main/docs/intro-to-using-changesets.md

## 2. GitHub App をインストール

https://github.com/apps/changeset-bot

リンクから GitHub に追加してください。
この Bot は PR に以下のようなメッセージをつけて Changesets で変更がされているかされていないかを返信するので必要なリポジトリのみにすることをおすすめします。

![](https://user-images.githubusercontent.com/38714187/209133509-98a1c71d-08c1-49b4-a25b-e2283fb8a180.png)

> Changesets 側の更新フラグが無いよっていう画像

## 2. GitHub Actions をセットアップ

こちらの`With Publishing`をコピーすれば問題ないです、自分の場合下の方の Slack 通知は消してます。
https://github.com/changesets/action/blob/main/README.md#with-publishing

:::message
GitHub Actions の方で`yarn release`というコマンドを使っているので`package.json`に`release`スクリプトを追加しておきましょう。
基本は`"release": "changeset publish"`で問題ないですが、ビルドが必要な場合はここで。
:::

## 3. GitHub に npm の TOKEN を設定

npm の TOKEN を取得 (`https://www.npmjs.com/settings/<name>/tokens`)

GitHub のリポジトリ設定から actions のところに`NPM_TOKEN`という名前で TOKEN を設定します。

(設定: `https://github.com/<org>/<repo>/settings/secrets/actions`)

## 4. (optional) チェンジログに GitHub の PR リンクやユーザー名を表示

追加でインストールしてから設定の`changelog`を書き換える。

```
yarn add -D @changesets/changelog-github
```

.changeset/config.json

```json
{
  "changelog": ["@changesets/changelog-github", { "repo": "<org>/<repo>" }]
}
```

https://github.com/changesets/changesets/blob/main/docs/config-file-options.md#changelog-false-or-a-path

# 実際に使う

ここまでで設定は終わりです。
実際のリリースの手順です。

- 1. コードを書く
- 2. `yarn changeset`を実行する
- 3. monorepo なら更新してるものを選択
- 4. semver の`major`,`minor`,`patch`を選択
- 5. コードと`.changeset/*`に生成されるマークダウンをまとめてコミット
- 6. PR をマージ。このとき Bot のメッセージで`🦋 Changeset detected`となってることを確認してください。
- 7. まだリリースはされていません、リリースするためにはリリース用の PR(Version Packages)をマージします。

これでリリースまで完成です、npm と GitHub の release に追加されているはずです。
:::message
**7** の PR をマージする前に **6** までを繰り返してから **7** の PR をマージすると一気にリリースが可能です。
:::

#### 他人のコードまたは先にコミットしてしまった場合

- まだ PR まではしてない場合
  - そのまま`.changeset/*`にマークダウンを作成してコミットすればいいです。(自分の環境なら `yarn changeset` でいい)
- PR までできている場合
  - changeset-bot のメッセージの一番下にあるリンクから GitHub 上で作成できる。
  - (`Click here if you're a maintainer who wants to add a changeset to this PR`)

# 最後に

semantic-release のモノレポ対応が悪くて探しているなかで turborepo がおすすめしていたので使ってみたら案外便利でした。
見やすいドキュメントが無いので手間取りましたが一度設定してしまえば楽に使えます。
（semantic-release みたいにコミットするだけじゃないのは手間だけど自由度は高い）
モノレポじゃなくても便利ですが、特にモノレポでいいリリースツール探してたら使ってみてください。
