---
layout: post
title: "First octopress"
date: 2014-03-05 13:30:30 +0800
comments: true
categories: octopress
---
記錄一下自己使用octopress

#### 安裝
```
 git clone https://github.com/imathis/octopress.git octopress
 cd octopress
 gem install bundler
 bundle install
 rake install 
```
 
設定octopress

	rake setup_github_pages

接著會出現詢問要對應的github，那就填入自己的```git@github.com:your_name/your_name.github.io```


#### 寫文章
新增一篇文章
	
	rake new_post["文章名稱"]


接著在source/_post內就可以看到新增副檔名爲.markdown的文章

文章寫完後要轉換成html

	rake generate


都沒問題後，最後就進行文章的發佈

	rake deploy


官網上有提到`rake deploy`後會將檔案複製到_deploy/ 之中，並且add、commit最後push到master branch之中



