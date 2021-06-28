---
title: '文字列の解析に正規表現をやめてパーサーコンビネーター使う (JavaScript/TypeScript)'
emoji: '📐'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: [javascript, typescript, parser, 正規表現]
published: false
---

正規表現はキモい、ダルい、無駄に難しい、型が付かないなど便利ではあるけどちょっとでも手を込んだことをしようとすると色々と嫌なところが出てきます。
それに比べたらとっても簡単に同じ処理ができるパーサーコンビネーターが最強なので布教する記事です。

今回は計算しそうな文字列を解析するのを例に進めていきます。

`1 + 1`が以下のようになるのが目標です。

```yml
1: number
+: plus
2: number
```

# 正規表現で`1 + 1`を解析する

```js
/(?<loperand>[0-9]) (?<plus>\+) (?<roperand>[0-9])/.exec('1 + 1'); //
```
