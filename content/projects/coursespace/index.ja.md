---
title: CourseSpace
date: 2020-03-03
slug: coursespace
thumbnail: "images/cs-main_view.png"
description: "文理融合系学部における多様な履修パターンを可視化するツール"
tags: ["application", "academic"]
draft: false
---

本プロジェクトは、学部卒業研究として取り組んだものである。  

概要
---

文理融合系学部においては、扱う分野が多岐に渡ることから履修選択における自由度が高いことが多い。  
そのような学部においては、教員はおろか学生同士でも誰がどのように科目を履修しているかを把握することは困難であった。
本プロジェクトでは、ネットワーク科学のアプローチから学生の履修を可視化することを試みた。  

システム構成
---

{{< figure src="./images/cs-system_architecture.png" >}}

データの加工から可視化まで全ての処理をブラウザ上で行う。  
MySQLサーバは任意のSQLを発行できるPHP製のAPIでラッパーされている。  
DBからのデータ取得はasync/awaitによって同期的に処理される。  
Crossfilter.jsによってフィルタ処理を実装し、ネットワークとして可視化するためにD3.jsを用いた。  

### 使用技術

- Javascript
  - jQuery
  - D3.js
  - Crossfilter.js
- MySQL

成果物
---

### ソースコード

本システムは卒業研究の一環でありソースコードは非公開である。  

### 学会発表

- 塩澤大輝，松澤芳昭："文理融合系学部における履修モデル可視化システムの開発", 第11回データ工学と情報マネジメントに関するフォーラム（DEIM2019）,2019. [PDF](https://db-event.jpn.org/deim2019/post/papers/344.pdf)
- 塩澤大輝，松澤芳昭："文理融合系学部における履修モデル可視化システムの開発と評価", 情報教育シンポジウム（SSS2019）, 2019 [PDF](https://ipsj.ixsq.nii.ac.jp/ej/index.php?action=pages_view_main&active_action=repository_action_common_download&item_id=198651&item_no=1&attribute_id=1&file_no=1&page_id=13&block_id=8)（学生奨励賞）
- Daiki Shiozawa, David F. Hoenigman, Yoshiaki, Matsuzawa: "CourseSpace: The Observatory of Course Taken Models in Interdisciplinary Departments", Open Conference on Computers in Education, 2020

