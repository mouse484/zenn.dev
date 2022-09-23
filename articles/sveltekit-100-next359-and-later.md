---
title: "SvelteKit@1.0.0-next.359,374,405のアップデートメモ"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [sveltekit, svelte, vite]
published: true
---

SvelteKit@1.0.0-next.359で`vire.config.js`が必要になった
https://github.com/sveltejs/kit/blob/master/packages/kit/CHANGELOG.md#100-next359

> [breaking] require vite.config.js
> 破壊変更: vite.config.js を必要にする

https://github.com/sveltejs/kit/blob/master/packages/kit/CHANGELOG.md?#100-next374

> removed vite key from config definition
> svelte config から vite 設定を削除

## 解決

### 1. vite を追加する

これまでは svelte-kit コマンドを使っていたが dev/build などが vite のコマンドに変わるのでパッケージを追加する必要があります。
`yarn add -D vite`
`npm install -D vite`

### 2. svelte.config.js から vite 設定を削除 (^next374)

以下は自分が使ってたのですが中の`vite`を消します

```js:svelte.config.js
import vercel from '@sveltejs/adapter-vercel';
import preprocess from 'svelte-preprocess';

/** @type {import('@sveltejs/kit').Config} */
const config = {
  preprocess: preprocess(),
  kit: {
    vite: {...} // <-これを消す
    adapter: vercel(),
  },
};

export default config;
```

### 3. vite.config.js を追加する (^next359)

```js:vite.config.js
  import { defineConfig } from 'vite';
  import { sveltekit } from '@sveltejs/kit/vite';

  export default defineConfig({
    plugins: [sveltekit()],
  });
```

参考: https://github.com/sveltejs/kit/issues/5184
:::message
正確なバージョンは追えていないですが next.359<499 のどこかのバージョンから`@sveltejs/kit/experimental/vite`ではなく`@sveltejs/kit/vite`から sveltekit を取得します
:::

### 4. routes ディレクトリなどを含むマイグレーション (^next.405)

`npx svelte-migrate routes`と打つことで自動でマイグレーションされます。
あとはコンソールに出るのですが自動修正できない部分については GitHub の情報と照らし合わせて変更します。（自分の場合は少しだけしか出ていないし、急に修正する必要がある部分はありませんでした）

参考: https://github.com/sveltejs/kit/discussions/5774


## 色々混ざって大変な目に合う前にこまめに依存関係の更新をしよう！！！
