---
title: CourseSpace
date: 2020-03-03
thumbnail: "images/cs-main_view.en.png"
description: "Visualization tool of the various course-taking patterns on interdisciplinary departments."
tags: ["application", "academic"]
draft: false
---

This project is graduation research on undergraduate.  

Background and Overview
---

Interdisciplinary departments offers diverse course-taking patterns with highly free choises because they dealt wide academic filelds.  
It is difficult for students not only teachers to grasp course-taking patterns in those departments.  
I tried to visualize the course-taking patterns using network science approach on this project.  

System Architecture
---

{{< figure src="./images/cs-system_architecture.en.png" >}}

All processes, from data processing to visualizations, runs on a browser.  
MySQL server is wrapped by API written in PHP that is able to issue any SQL statements.  
Getting data from DB are processed synchronous by async/await.  
Use Crossfilter.js for filtering features, and D3.js for visualization of network graphs.  

### Technologies

- Javascript
  - jQuery
  - D3.js
  - Crossfilter.js
- MySQL

### Source Codes

Source codes are not published since the system is a part of graduation research.  

### Conferences

- Daiki Shiozawa，Yoshiaki Matsuzawa："Development of The Observatory of Course Taken Models in Interdisciplinary Departments", The 11th Forum on Data Engineering and Information Management（DEIM2019）,2019. (in Japanese) [PDF](https://db-event.jpn.org/deim2019/post/papers/344.pdf)
- Daiki Shiozawa，Yoshiaki Matsuzawa："Development and Evaluation: The Observatory of Course Taken Models in Interdisciplinary Departments", Summer Symposium in Sesera（SSS2019）, 2019. (in Japanese) [PDF](https://ipsj.ixsq.nii.ac.jp/ej/index.php?action=pages_view_main&active_action=repository_action_common_download&item_id=198651&item_no=1&attribute_id=1&file_no=1&page_id=13&block_id=8)（Student Encouragement Award）
- Daiki Shiozawa, David F. Hoenigman, Yoshiaki, Matsuzawa: "CourseSpace: The Observatory of Course Taken Models in Interdisciplinary Departments", Open Conference on Computers in Education, 2020
