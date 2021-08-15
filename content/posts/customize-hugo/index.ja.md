---
title: "hugoの設定を自分好みにカスタマイズ"
date: 2019-01-02T18:35:45+09:00
tags: ["develop", "hugo"]
draft: true
---


リンクの画面遷移
---

外部リンクは別タブ (`target="_blank"`) 内部リンクは同じタブで遷移させたい．

HUGOが採用しているMarkdownエンジン[Blackfriday]()の設定をいくつかいじることができる．  `config.toml`に以下のように記述すればよい．  
（参考：[Configure Blackfriday - HUGO](https://gohugo.io/getting-started/configuration/#configure-blackfriday)）

``` toml
[blackfriday]
hrefTargetBlank = true
```

新記事作成時のエディタ
---

CLIから記事を作成するコマンドは`hugo new [path]`だが，この際`--editor [string]`を指定すると任意のエディタでファイルを開くことができる．  
自分は，そのままEmacsで編集するので`hugo new [path] --editor emacs`とコマンドを流す．  
都度エディタを指定するのはいささか面倒である．
調べてみると，Configuresのパラメタに`NewContentEditor`というものがある．  
`config.toml`に`newcontenteditor = "emacs"`と指定すればオプションの指定は端折れるということだ．

しかし，サイト本体に関わる設定ではないものがサイトの設定ファイルに記述されるというのは少し気持ちが悪い．  
よく見てみると，環境変数での設定という項目がある．  
どうやら，`config.toml`に記述する代わりに環境変数として設定することができるらしい．  
というわけで，環境変数に設定した．  
参考：[Configure with Environment Variables](https://gohugo.io/getting-started/configuration/#configure-with-environment-variables)  
