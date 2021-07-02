---
title: Amrta测试博客
date: 2018-01-25 18:30:19
tags: [hugo]
---

## hexo 
这是第一次使用hexo搭建静态博客的测试文章
我的地址：https://Aquarian-Age.github.io/

## hexo转hugo

```bash
tar xvf hugo_0.84.3_Linux-64bit.tar.gz 
sudo mv hugo /usr/local/bin/
hugo version
cd ~/
hugo new site blog
cd blog/

git clone https://github.com/flysnow-org/maupassant-hugo themes/maupassant
cp exampleSite/config.toml ../../

rm -r themes/maupassant/.git/ #避免添加git子项

hugo new post/Amrta测试博客.md

git init
git remote add origin https://github.com/Aquarian-Age/Aquarian-Age.github.io.git
git checkout -b hugo

hugo server -D
hugo server --theme=maupassant --buildDrafts --baseUrl="https://aquarian-age.github.io/"
```

可以直接在　blog/content/post/里面添加xx.md文件
```bash
hugo
git status
git add .
git commit -m "hugo test"
git push --set-upstream origin hugo
```

themes/maupassant/static/css/　添加　toc_style.css
```css
.post-toc {
  transition: all .3s ease;
}

.post-toc:hover,.post-toc:active {
  width: 200px !important;
} 
```
