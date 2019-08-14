---
title: hexo搭建事项
date: 2019-08-14 20:46:30
tags:
	-hexo
	-实战踩坑
---

### hexo的备份问题

hexo本身的搭建并不困难，可以根据hexo的官方文档，亦或是其他人的搭建经验，可以比较轻松地搭建。而其中的具体细节，例如主题，头像，访问统计等后续优化内容可以在必要时临时查看相关文档，毕竟不是想成为hexo专家。

搭建完以后有一个很困扰的问题，如果某天我的电脑突然炸裂，我该怎么办？重新配置将又是一场恶战，费时费力。因而各种搜索关于“hexo“备份的解决方案。
### 详细的步骤
最终决定利用git中的分支功能进行管理，有如下步骤。以下步骤建立在已经将hexo部署到本地，并关联上GitHub的`xxx.github.io`仓库的基础上，对于尚未在本地搭建hexo的情况不适用（最后有补充说明）。

分别用默认的master分支管理静态文件和用自建的hexo分支管理配置文件。第一步，GitHub中创建hexo分支，并设为默认分支；第二步，clone `xxxx.github.io`（这个为GitHub的个人主页仓库）到本地，将文件夹中的`.git`文件复制到hexo文件夹（本地搭建hexo的文件夹）当中；第三步，查看hexo文件夹下的`_config.yml`中deploy配置是否为master分支；第四步，`git branch`，查看当前分支，应当为hexo，依次执行`git add .、git commit -m "..."、git push origin hexo`提交网站相关的文件；第五步，执行`hexo g -d`生成网站并部署到GitHub上。

对步骤进行说明，第二步的作用是为了将本地的整个hexo文件夹关联到`xxx.github.io`仓库，如果本地文件夹没有`.git`文件夹，当前的本地文件夹没有关联到任何仓库，第四步也就无法进行；第四步将整个本地的hexo文件夹里所有内容存储到`xxx.github.io`中的hexo分支（hexo被设置为默认分支），但由于本地文件夹hexo中的`.gitignore`文件的存在--该文件指定了上传时需忽略的文件--最终被存储到hexo分支的只有最相关的配置文件；注意区分hexo分支和本地hexo文件夹。
### 日常的改动
关于日常的改动流程在本地对博客进行修改（添加新博文、修改样式等等）后，通过下面的流程进行管理。1. 依次执行`git add .、git commit -m "..."、git push origin hexo`指令将改动推送到GitHub（此时当前分支应为hexo）；2. 然后才执行hexo g -d发布网站到master分支上。虽然两个过程顺序调转一般不会有问题，不过逻辑上这样的顺序是绝对没问题的（例如突然死机要重装了，悲催....的情况，调转顺序就有问题了）。
### 本地资料丢失后的流程
当重装电脑之后，或者想在其他电脑上修改博客，可以使用下列步骤：1. 使用`git clone git@github.com:xxx/xxx.github.io.git拷贝仓库（默认分支为hexo）`；2. 在本地新拷贝的xxx.github.io文件夹下通过Git bash依次执行下列指令：`npm install hexo、npm install、npm install hexo-deployer-git`（记得，不需要hexo init这条指令）。

对于仅仅在GitHub中创建了一个`xxx.github.io`仓库的情况，可以参考[知乎中的高票回答](https://www.zhihu.com/question/21193762/answer/79109280)。本文也主要参照了该回答的步骤和方法
（如有侵权，联系修改、删除）。
