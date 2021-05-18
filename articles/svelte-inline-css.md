---
title: 'SvelteでVue.jsみたいにインラインCSSを使うライブラリを作った'
emoji: '🐷'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: [svelte, css]
published: true
---

# 作ったもの

https://github.com/mouse484/svelte-inline-css

# なんで作ったか

- 標準でいい感じにインライン CSS が書けなかった
  - 動的に CSS を変更するとしたら結構面倒くさい
- [svelte-css-vars](https://github.com/kaisermann/svelte-css-vars)というライブラリがあったが css の var が楽に使えるだけで別に追加で css を書かないといけなくてめんどくさかった
- [Vue.js のインライン CSS](https://zenn.dev/mouse_484/articles/vue-inline-style) が使いやすいからほぼそれと同じ使い方ができるようにしたかった

[作った経緯のメモ](https://zenn.dev/mouse_484/scraps/b5306933034466)

# 使い方

```
yarn add svelte-inline-css
```

```vue:.svelte
<script lang="ts">
  import style from 'svelte-inline-css';
  export let height: `${number}px` = '0px';
</script>

<div use:style={{ height, backgroundColor: 'pink' }} />
```

Vue.js とほとんど一緒で CSS プロパティは camelCase を使用します。

# これから

- Vue.js と挙動をほぼ一致させる
  - Array Syntax
  - Auto-prefixing
  - Multiple Values
- インラインに展開するんじゃなくて CSS として出力できるようにしたい
  - SvelteKit とかビルドツールによって色々ありそうだからできたら程度
