---
title: "WSLチートシート"
emoji: "🌟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [wsl, wsl2]
published: true
---

## コマンド編

#### 一覧表示

```
wsl -l -v
```

```:例
  NAME          STATE           VERSION
* Ubuntu        Running         2
  Debian        Stopped         1
  Ubuntu-20.04  Stopped         2
```

`wsl --list --verbose`の略`wsl --list`だとディストリビューションの名前しかでなくて使いものにならないので基本こちら。

#### wsl を終了

```:すべて
wsl --shutdown
```

```:特定のディストリビューション
wsl --terminate <ディストリビューション>
```

とりあえずバグったら使えば OK なイメージ
基本 shutdown でいいと思う
普段は閉じて時間経過で終了するのであまり使わないかも

#### ディストリビューションを追加

```
wsl -d <ディストリビューション>
```

`wsl --install --distribution`の略、↓ で確認してからするといいかも

#### インストールできるディストリビューションを確認

```
wsl --l --o
```

```:例
NAME            FRIENDLY NAME
Ubuntu          Ubuntu
Debian          Debian GNU/Linux
kali-linux      Kali Linux Rolling
openSUSE-42     openSUSE Leap 42
SLES-12         SUSE Linux Enterprise Server v12
Ubuntu-16.04    Ubuntu 16.04 LTS
Ubuntu-18.04    Ubuntu 18.04 LTS
Ubuntu-20.04    Ubuntu 20.04 LTS
```

`wsl --list --online`の略

#### ディストリビューションを削除

```
wsl --unregister <ディストリビューション>
```

#### WSL のバージョンを変える

```
wsl --set-version <ディストリビューション> <バージョン 1 or 2>
```

#### その他詳細

https://learn.microsoft.com/ja-jp/windows/wsl/basic-commands


#### コマンド以外はまた追記します