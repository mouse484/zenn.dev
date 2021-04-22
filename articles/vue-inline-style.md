---
title: '[Vue.js] インラインスタイルを使用する'
emoji: '🔖'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: [vue, css]
published: true
---

[クラスとスタイルのバインディング | Vue.js](https://v3.ja.vuejs.org/guide/class-and-style.html#%E3%82%A4%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB%E3%81%AE%E3%83%8F%E3%82%99%E3%82%A4%E3%83%B3%E3%83%86%E3%82%99%E3%82%A3%E3%83%B3%E3%82%AF%E3%82%99) を少し噛み砕いた記事です、詳細が知りたい方はドキュメントを読みに行ってください。

一番下にサンプルを載せているのでわからなければ参考にしてください。

## インラインスタイルの使い方

```vue
<div :style="{ color: 'red' }">サンプル</div>
```

:::message
`:`は`v-bind:`の省略で`v-for`などと同じ Vue の構文です。
:::

### ハイフンが入る場合

```vue
<div
  :style="{ 'background-color': 'pink', fontSize: '20px' }"
>ハイフンが入る場合</div>
```

css のプロパティ名にハイフン`-`が含まれる場合はクオート`'`で囲むか`camelCase`（つなぎ目を大文字にする）にする必要があります。
どちらにすべきということはありませんが`camelCase`の方を多く見かけるため`camelCase`のがいいかもしれません。

### 変数を使う

```vue
<div :style="{ color: textColor }">変数を使う</div>

data: { textColor: 'blue' }
```

### 変数をまとめる

```vue
<div :style="styleObject">変数をまとめる</div>

data: { styleObject: { color: 'yellow', backgroundColor: 'green' } }
```

## サンプル

@[stackblitz](https://stackblitz.com/edit/vue-inline-style?embed=1&file=src/App.vue&hideExplorer=1&hideNavigation=1)

## おわりに

Vue.js でインラインスタイルを使う時、に使ういくつかのポイントをドキュメントから抜き出してまとめました。
もっと詳しくって人は[Vue.js](https://jp.vuejs.org/index.html)のドキュメントを見に行ってください。

わからないことなどあればコメントや Twitter などで受け付けています。
