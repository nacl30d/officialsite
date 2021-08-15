---
date: 2020-04-20T18:48:48+09:00
title: "EmacsでJavasctipt Semistandardを使う"
tags: ["develop", "javascript", "emacs"]
draft: false
---

Emacs x FlyCheck x Semistandardを使おうとしたら苦労したのでメモ．

Semistandardとは
===

[JavaScript Standard Style](https://standardjs.com)という，GitHubやらElectronやらが採用しているCoding Styleがある．  
設定不要で自動フォーマットまでしてくれる便利なやつ．  
ただし，標準のルールでは**セミコロンは省略**が採用されている．  
このあたりは宗教論争なので，**セミコロンありバージョン**が用意されている．  
それがSemistandardだ．  

FlyCheckでSemistandardを使う
===

最終的な設定はこんな感じ．  
ちなみにEmacsの設定には[straight.el](https://github.com/raxod502/straight.el)を使っている．  

```init.el
(use-package exec-path-from-shell
  :init
  (exec-path-from-shell-initialize))

(use-package flycheck
  :init
  (global-flycheck-mode)
  :config
  (setq flycheck-display-errors-delay 0.1)
  (setq-default flycheck-disabled-checkers '(javascript-jshint))
  (setq flycheck-javascript-standard-executable "semistandard")
  (flycheck-add-mode 'javascript-standard 'web-mode)
  (flycheck-add-mode 'javascript-standard 'js2-mode))

```

ポイントは2つ  

1. 標準ではstandardがexecutableになっているので，そいつを変更しなければならない  
1. executableを変更するときにpathが読み込まれている必要があるので，`exec-path-from-shell`を設定する  

1のexecutableの変更は，ドキュメントには一言も書いてなかった．不親切極まりない．（参考：[Supported Languages - Flycheck](https://www.flycheck.org/en/latest/languages.html#javascript)）  

ソースコードにはちゃんと書いてあって，それを見てこの設定にたどり着いた．

```el
This checker works with 'standard' and 'semistandard', defaulting
to the former.  To use it with the latter, set
'flycheck-javascript-standard-executable' to 'semistandard'.
```

2は，`(setq flycheck-javascript-standard-executable "semistandard")`の'semistandard'の部分をフルパスで書けば別に設定しなくても良い．  


さいごに
===

なんでstandardじゃなくてsemistandardを使おうかと思ったかといえば，天下のGoogleさまがそう言っていたからです．（参考：[Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html#formatting-semicolons-are-required)）  
