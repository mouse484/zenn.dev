---
title: 'SvelteKitでWindi CSSを使う'
emoji: '🌟'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: [svelte, sveltekit, windicss]
published: true
---

何かすごいらしい [Windi CSS](https://windicss.org/) を [SvelteKit](https://kit.svelte.dev/) で試してみたら、いくつか注意点があったので共有します。

# インストール

SvelteKit は Vite 使ってるし Framework としては Svelte だし...と思ったけど Vite を選択するのが良いみたい。

> ![image](https://user-images.githubusercontent.com/38714187/118356010-2c790e00-b5ae-11eb-92da-74b6e546e8ba.png) > https://windicss.org/guide/installation.html

:::details 参考
[svelte-add/windicss #1](https://github.com/svelte-add/windicss/issues/1)

[windicss/svelte-windicss-preprocess #65](https://github.com/windicss/svelte-windicss-preprocess/issues/65)
:::

なので[vite-plugin-windicss](https://windicss.org/integrations/vite.html)をインストールします。

```shell
npm i -D vite-plugin-windicss windicss
```

# 設定

```js:svelte.config.js
import WindiCSS from 'vite-plugin-windicss';

/** @type {import('@sveltejs/kit').Config} */
const config = {
  kit: {
    vite: {
      plugins: [ WindiCSS.default() ]
    }
  }
};

export default config;
```

:::message

~~`import WindiCSS from 'vite-plugin-windicss';`だとモジュールの解決周りの影響で cjs が読み込まれてしまうので直接 mjs をインポートしています。~~
v1 から必要なくなりました。しかし.default が必要です。
:::

# css を読み込む

```js:src/routes/__layout.svelte
<script>
  import 'virtual:windi.css';
</script>
```

### 参考

https://windicss.org/integrations/vite.html#sveltekit-as-of-1-0-0-next-100

- 一部バージョンの違いで本記事と違いがあります。（記事作成時バージョン @sveltejs/kit@1.0.0-next.107）
- 2021/06/22 更新 vite-plugin-windicss@1.1.0 @sveltejs/kit@1.0.0-next.115 現在は**ほぼ**公式と同じです
