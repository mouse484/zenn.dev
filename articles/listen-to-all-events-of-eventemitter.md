---

title: 'EventEmitter の全てのイベントを取得する方法(Node.js)'
emoji: '👌'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['nodejs', 'eventemitter']
published: true

※ [数年前 Qiita に投稿した記事](https://qiita.com/mouse_484/items/a2f0b82a8f02dcf42404)の修正版です

Node.js の [EventEmitter](https://nodejs.org/api/events.html) は便利なんですがたくさんのイベントを用意して使っていると一旦発生したイベントをまとめて見たくなります。

ですが、標準 Node.js には全てのイベントをまとめて取得する方法はありません。

### 解決方法その 1

[EventEmitter2](https://github.com/EventEmitter2/EventEmitter2)などの対応したライブラリを使う。

例：

```js
const EventEmitter2 = require('eventemitter2');
const emitter = new EventEmitter2();

emitter.on('*', () => console.log('EventEmitter2'));

emitter.emit('event');
```

この場合はワイルドカード(\*)で全てのイベントを取得しています。

### 解決方法その 2

Node.js の EventEmitter を拡張する。

ちょっと確認のためだけだしライブラリを使うほどでもないなって時おすすめ。
そこまで機能いらないし無駄にライブラリ増やしたくないのでこちらばかり使ってます。

例:

```js
const EventEmitter = require('events');

class ExtendEventEmitter extends EventEmitter {
  emit(name, ...args) {
    return super.emit('*', name, ...args);
  }
}

const event = new ExtendEventEmitter();

event.on('*', () => console.log('ExtendEventEmitter'));

emitter.emit('event');
```

（わざわざ再 emit しなくて class 拡張するところで log にしてもいいけど、`*`に emit し直したほうが使いやすいはず）

### まとめ

それなりに全ての Event を取得したい人はいるみたいでライブラリは結構ある。
でも class 拡張するだけでもできるからわざわざライブラリ使う必要あるんかなと思ったり思わなかったり。
