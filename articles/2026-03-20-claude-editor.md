---
title: 'Claude Code のプロンプトを Cursor で書く'
emoji: '📝'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['claude']
published: true
---

## claudeは「ctrl+g」でエディタから入力できる

以前にも記事にしましたが、claudeにはエディタを開くショートカットが用意されており、
**「ctrl+g」** でエディタからプロンプトを入力できるようになります。
開かれるエディタは環境変数`EDITOR`に設定したものが選ばれます。

```
export EDITOR=vim
```

前回の記事：https://zenn.dev/uttnaoki/articles/98e7e7a5542c27

前回はvimから開くことができていましたが、vscodeやcursorでやろうとすると失敗していました。

## cursorでプロンプトを入力する

vimのようなcuiのエディタではなく、cursorのようなフルスクリーンのエディタを使う場合は環境変数の設定方法が異なるようです。

```
export VISUAL="cursor --wait"
```

上記のように`VISUAL`という変数で`--wait`のオプションを設定するとcursorが開けました。

## おわりに

vimもいいですが日本語を入力するならcursorやvscodeのようなフルスクリーンエディタの方が入力しやすいですね。
改行を含む長文を入力する場合はとても便利です。
