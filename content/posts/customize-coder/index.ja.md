---
title: "hugoのテーマcoderを自分好みにカスタマイズ"
date: 2019-01-02T15:56:08+09:00
tags: ["develop", "hugo"]
draft: true
---


フォント
---

まず，フォント日本語対応のフォントじゃない．  
フォントの設定を探すと，themeのscssで変数化されていた．  
本当はあんまり触りたくないのだけれど，自分でcssを書くのも面倒なので上書いてしまった．

`hugo-coder/assets/scss/_variables.scss`

``` scss
$text-font-family: "游ゴシック", "Yu Gothic", YuGothic, Merriweather, Georgia, serif;
$heading-font-family: "游ゴシック", "Yu Gothic", YuGothic, Lato, Helvetica, sans-serif;
```


スタイル
---

[coder](https://themes.gohugo.io/hugo-coder/)では`config.toml`に任意のスタイルシートを設定する項目がある．  
（参考：[Configurations - hugo-coder wiki](https://github.com/luizdepra/hugo-coder/wiki/Configurations)）

`config.toml`

``` toml
[params]
    custom_css = ["css/style.css"]
```

### aタグ

aタグのスタイルがデフォルトに近くてあまり美しくない
[テキストリンクのホバー時に使えるエフェクト・デザインサンプル 15 - NxWorld](https://www.nxworld.net/tips/15-text-link-hover-effect-and-design.html)を参考にモダンな感じに追記．

aタグは色々なところに使われているので適宜cssをキャンセルする必要がある．
今のところプロジェクトページでのリンクとの相性が悪いので，プロジェクトページで`a::after`を表示させないようにした．  

`static/css/style.css`

``` css
article a {
    position: relative;
    display: inline-block;
    transition: .3s;
    color: #3498db;
}
article a:hover {
    color: #2ecc71;
    text-decoration: none;
}
article a::after {
    position: absolute;
    bottom: .3em;
    left: 0;
    content: '';
    width: 100%;
    height: 1px;
    background-color: #2ecc71;
    opacity: 0;
    transition: .3s;
}
article a:hover::after {
    bottom: 0;
    opacity: 1;
}
.navigation a,
.list ul li a {
    position: relative;
}
.navigation a:hover,
.list ul li a:hover {
    color: #212121;
    text-decoration: none;
}
.navigation a::after,
.list ul li a::after {
    position: absolute;
    bottom: -0.3em;
    left: 50%;
    content: '';
    width: 0;
    height: 3px;
    background-color: #2ecc71;
    transition: .5s;
    -webkit-transform: translateX(-50%);
    transform: translateX(-50%);
}
.navigation a:hover::after,
.list ul li a:hover::after {
    width: 100%;
}
.project a::after {
    width: 0;
    height: 0;
}
```

プロジェクト
---

プロジェクトページのためにShortcodeを作成した．  
[portfolio.html - hugo-coder-portfolio](https://github.com/naro143/hugo-coder-portfolio/blob/master/layouts/shortcodes/portfolio.html)を参考にさせてもらった．  
外部リンクを設定したのが大きな違いだ．自分のプロダクトはおそらく殆どがwebなので．  
今更だが，aタグのスタイルを変えたのもこのForkにかなり影響されている．  
しかし，フォントなどいくつか趣味に合わなかったのでこのテーマは採用しなかった．  

`layouts/shortcodes/projects.html`

``` html
{{ if .IsNamedParams }}
  {{ $.Scratch.Set "src" ( .Get "src") }}
  {{ $.Scratch.Set "alt" ( .Get "alt") }}
{{ else }}
  {{ $.Scratch.Set "src" "" }}
  {{ $.Scratch.Set "alt" "" }}
{{ end }}

{{ $src := $.Scratch.Get "src" }}
{{ $alt := $.Scratch.Get "alt" }}

<div class="project">
  <div class="project-inner">
    <div class="project-image">
        <img src="{{ $src }}" alt="{{ $alt }}">
    </div>
    <div class="project-content">
      {{ .Inner }}
    </div>
    {{ with .Get "href" }}<a href="{{ . }}" target="_blank"></a>{{ end }}
  </div>
</div>

```

