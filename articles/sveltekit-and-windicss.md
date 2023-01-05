---
title: "SvelteKitでWindi CSSを使う"
emoji: "🌟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [svelte, sveltekit, windicss]
published: true
---

何かすごいらしい [Windi CSS](https://windicss.org/) を [SvelteKit](https://kit.svelte.dev/) で試してみたら、いくつか注意点があったので共有します。
（SvelteKit@1.0.0-next.359以降はほとんど癖なく Vite で使う方法をそのまま利用できます）

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

```js:vite.config.js
import { defineConfig } from 'vite';
import { sveltekit } from '@sveltejs/kit/vite';
import WindiCSS from 'vite-plugin-windicss';

export default defineConfig({
  plugins: [sveltekit(), WindiCSS()],
});
```

:::details >=SvelteKit@1.0.0-next.359

```js:svelte.config.js
import WindiCSS from 'vite-plugin-windicss';

/** @type {import('@sveltejs/kit').Config} */
const config = {
  kit: {
    vite: {
      plugins: [ WindiCSS() ]
    }
  }
};

export default config;
```

参考: https://windicss.org/integrations/vite.html#sveltekit-as-of-1-0-0-next-102
:::

# css を読み込む

```js:src/routes/+layout.svelte
<script>
  import 'virtual:windi.css';
</script>
```

### 参考

https://windicss.org/integrations/vite.html#sveltekit-as-of-1-0-0-next-100

- 一部バージョンの違いで本記事と違いがあります。（記事作成時バージョン @sveltejs/kit@1.0.0-next.107）
- 2021/06/22 更新 vite-plugin-windicss@1.1.0 @sveltejs/kit@1.0.0-next.115 現在は**ほぼ**公式ドキュメントと同じ手順で可能です
- 2022/09/23 更新 SvelteKit@1.0.0-next.359以降 vite.config.js に変わったため Vite で使う例をそのまま利用する
  - (公式の Svelte のサンプルを利用しない)方法でうまくいきます（詳細は以下の別記事）

https://zenn.dev/mouse_484/articles/sveltekit-100-next359-and-later
