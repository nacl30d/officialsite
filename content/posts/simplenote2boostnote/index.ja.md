---
title: "SimplenoteからBoost Noteに移植する"
slung: "Simplenote2boostnote"
date: 2020-03-18T17:18:15+09:00
tags: ["develop", "note"]
draft: false
---

### TL;DR

完成品はこちら↓  
{{< hatena-card "https://gist.github.com/nacl30d/7f20d53083e6291014c157421f076211" "Gist" >}}

みなさんはお気に入りのMarkdownエディタはありますか？  
いま使っているMarkdownエディタに満足していますか？  
Markdownエディタの旅って結構果てしないと思うんですよ．  

私は[Boostnote](https://boostnote.io)に始まりしばらく[Simplenote](https://simplenote.com)を使い，紆余曲折の末，**今のところは**Apple純正のNotesに落ち着いています．（Markdownエディタじゃないんですけどね）  
この紆余曲折の話は長くなるので別の機会にしますが，Boostnoteを使うのをやめたのはMobileアプリがイケてなかったからなんですね．  
でもさっき見たらβ版開発を抜けて新バージョンとしてBoost Note全体が[アップデート](https://boostnote.hatenablog.com)されてたので，もう一度出直そうかなと．  

単純に乗り換えるだけならそんなに難しくないんですけど，ちゃんと前アプリに書き溜めたノートを引き継ごうとすると結構めんどくさい．  
アプリごとに独自のフォーマットで保存してやがるので，変換してやらなきゃいけないんですね．  

前置きが長くなりましたが  
と，いうわけで...  

SimplenoteからBoostnoteにノートデータを移行するスクリプトを書いた
===

最近新しくなったBoost Noteは，β版のBoostnoteのデータをImportする機能が実装されているんですね．  
つまり，Boostnoteのフォーマットに変換してやれば，それを使ってImportできるってわけ．  
Boostnoteはこんなフォーマット  

`note.cson`

```note.cson
createdAt: "2020-03-18T09:27:28.409Z"
updatedAt: "2020-03-18T04:54:12.915Z"
type: "MARKDOWN_NOTE"
title: "ノートの名前"
content: '''
  ノートの本文
'''
tags: [
  "tag"
]
```

`cson`っていうJSONのCoffeeScript版を使ってる．  

対するSimplenoteはこんなフォーマット  

`note.json`

```.json
{
  ActiveNotes: [
    {
      id: "436127901234948721",
      creationDate: "2020-03-18T09:27:28.409Z",
      lastModified: "2020-03-18T09:27:28.409Z",
      content: "ノートの本文",
      markdown: "true",
      tags: ["tag"]
    }
  ]
}
```

こっちは普通のJSON  

Simplenoteは[こんな感じ](https://simplenote.com/help/#export)でノートを簡単にExportすることができる．  
zipを解凍すると，各noteのtextファイルと`source/`に`notes.json`が入ってる．  

この`notes.json`には全ノートの情報が入っていて，こいつをゴニョゴニョすればほしいcsonファイルが手に入る!  
というスクリプトを，pythonのCLIとして作った．  
{{< gist d-salt 7f20d53083e6291014c157421f076211 "simplenote2boostnote.py" >}}  

さいごに
===

メタデータつけてるのに，最終更新日とか反映されてないっぽい...  
作成日とかって検索するときに結構ヒントになるんだけどなぁ．
個人的には，書いた日をタイトルに入れてるからそれでソートできるんだけども．  

あと，Boost Noteもまだ完全じゃないので，メインユースはもうしばらく様子見が必要かも．  
だけどナレッジの分散を防ぐために，とりあえず一箇所にはまとめておきたい．  
今一番ナレッジが溜まってるのはNotesだからそこから救出したい...  
さくっと調べたらファイルじゃなくてDB管理っぽいからExportに工夫がいるな...  
