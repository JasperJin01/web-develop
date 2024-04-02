

感谢Github Pages[博客模版](https://sighingnow.github.io/jekyll-gitbook)。



### 博客渊源与搭建过程

这已经是第二个Github Pages了，第一个Github Pages（[`timaxthu.github.io`](https://timaxthu.github.io)）不出意外的话也不会再更新。

**搭建博客**是考虑到，内容充实的博客可以写在简历中，处于升学/找工作岔路口的我深深感受了写简历时没有内容的无力感，因此要多积累经历，扩充简历的内容。同时，更新博客也能促进我更加主动学习。

**换博客域名**是因为之前的`timaxthu`和我本人名字并无太大关系。取了英文名Jasper Jin作为新的域名前缀（jasperjin被注册过了，只能在后面加一个01）。



在**本地部署**jekyll环境竟然成功了。这样调试博客方便太多了！之前用博客部署的时候一直失败，可能官方对m系列芯片优化了，有了arm64版本的环境，也把本地环境跑起来了（JekyII，你让我感到陌生）。

* 安装Ruby网站：https://jekyllrb.com/docs/installation/macos/ （MacOS安装Ruby，照着命令行做就可以了。🔵 **编译.zshrc命令`source ~/.zshrc`**）
* 本地部署jekyll：https://docs.github.com/zh/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll （🔵 **`bundle install` 、`bundle exec jekyll serve`。注意文件夹里的.gitignore文件，加入到其中路径的文件或文件夹会在git中被忽略**）

```shell
bundle exec jekyll serve
```

**模版与一些部署注意事项：**

* 🔵 **Github Pages的域名必须和注册用户名一致（也就是URL中github.com/后面的那一串）**，而不是Profile中的Name属性。例如：我的Github账号的URL为[http://github.com/jasperjin01](http://github.com/jasperjin01)，就不能使用`jasperjin.github.io`作为Pages的域名。
* 模版是GitBook主题的，fork来自[https://github.com/sighingnow/jekyll-gitbook](https://github.com/sighingnow/jekyll-gitbook)。更多模版主题：[http://jekyllthemes.org/](http://jekyllthemes.org/)。如果你也使用的是该模版，🔵 fork后要把**配置文件`_config.yml`中的url调整为自己的Pages URL，把baseurl调整为空**，否则jekyll会部署失败无法正常显示博客页面。



 
