---
title: "semantic-releaseとGitHub Actionsでnpmへいい感じに自動でリリースする"
emoji: "🌟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [npm, githubactions]
published: true
---

[semantic-release](https://github.com/semantic-release/semantic-release)を使えばコミットメッセージに基づいてリリースされます。
また、それを GitHubActions と組み合わせることで GitHub に push するだけで自動でリリースが完了します。

### semantic-release をインストールする

```shell
yarn add -D semantic-release
```

（基本的に yarn で進めていくので npm であればその都度読み替えてください）

### semantic-release のオプションを設定する

基本にオプションを指定せずデフォルトのままで問題無いです。
私は GitHub のデフォルトブランチが main なのでそれだけ設定しました。（semantic-release のデフォルトは master）

```json:package.json
{
  ...
  "release": {
    "branches": [
      "main"
    ]
  }
}
```

:::message
`package.json`の"version"はリリースするときに勝手に変わるので
"version": "0.0.0-dev"などにしておくのがおすすめ！
:::

### GitHub Actions の設定をする

```yml:.github/workflows/release.yml
name: semantic-release
on:
  push:
    branches: [main]

jobs:
  release:
    name: Release
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
        with:
          fetch-depth: 0
      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: 12
      - name: Install dependencies
        run: yarn install --frozen-lockfile
      # TypeScriptなどを使っている場合などビルドが必要ならここで
      - name: Compile TypeScript
        run: yarn build
      - name: Release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
        run: yarn run semantic-release
```

#### GitHub に環境変数を設定する

`https://github.com/name/repo/settings/secrets/actions`

`NPM_TOKEN`と`GH_TOKEN`を指定します。
詳しくは以下の URL を参考にしてください。
[CI configuration - Authentication for plugins](https://github.com/semantic-release/semantic-release/blob/master/docs/usage/ci-configuration.md#authentication-for-plugins)

### 実際にリリースする

コミットメッセージによってどのようなリリースになるか決まります。

v1.0.0 だったときからのコミットメッセージによるバージョンの変化の例をまとめました。

| コミット        | version            |
| --------------- | ------------------ |
| fix: ○□ を修正  | v1.0.1             |
| feat: ○□ を追加 | v1.1.0             |
| pref: ○□ を削除 | v2.0.0             |
| chore: ○□       | v1.0.0（変化なし） |

なので最初に何か機能を追加した場合は`feat`から始まるコミットメッセージにすることで自動的に npm と GitHub の Releases に`v1.1.0`としてリリースされます。

```shell
git commit -m 'feat: ○□ を追加'
```

:::details コミットメッセージを楽する
[commitizen](https://github.com/commitizen/cz-cli)をインストールします。

```shell
yarn add -D commitizen
commitizen init cz-conventional-changelog --yarn --dev --exact
```

commitizen を実行します。

```shell
yarn cz
```

![commitizen](https://raw.githubusercontent.com/commitizen/cz-cli/master/meta/screenshots/add-commit.png)

あとは指示に従って選択/入力をするだけです。

これで semantic-release に対応したコミットをかんたんにできます。
:::

### まとめ

semantic-release+GitHubActions でかんたんにリリースを自動化できました。
semantic-release を使うことで npm が推奨している[セマンティックバージョニング](https://docs.npmjs.com/about-semantic-versioning)をしつつ楽ができるのでいいことしかありません。
またプラグインなどを用いることで DockerHub や Chrome,VSCode の拡張機能などのリリースに応用できます、ドキュメントなどを読んで試してみてください。
