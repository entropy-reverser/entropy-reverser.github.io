---
layout:     post
title:      "基于SD扩散模型的生成作品展示"
subtitle:   "使用SD Web UI工作流，基于多种checkpoint大模型配合lora controlnet等微调模型"
date:       2024-05-27 12:00:00
author:     "逆熵人"
header-img: "img/home-bg.jpg"
catalog: true
tags:
    - entropy-reverser
---

> “Yeah It's on. ”


一直想有那么一个可以分享自己和表现自己的个人博客，随即花了差不多10天的时间搭建了名为**逆熵人**的个人博客网站.

[跳过废话，直接看技术实现 ](#build) 

搭建这个博客完全是在没有一点前端经验的基础下完成的，像是选择服务器，部署到仓库，git管理工具，html,JS，css,node，markdown标记语言这些技能都是在应用中学习的，
但做这一切支持我的动力就是对于学习和提升自己的巨大热情和专注，我不想放过任何一个我能学到的技术.
另外逆熵人(entropy-reverser)这个名字的来源于诺兰执导的电影<信条>，其实在这之前就对于'**熵**'这个物理概念莫名的痴迷，熵增代表着无序混乱
逆熵则代表着事物的永恒和有序， 我喜欢有序的有条理的做事.


<p id = "build"></p>

## 正文

接下来说说搭建这个博客的技术细节。

其中国内百度研发对标chatgpt的AI生成工具'文心一言'帮了我不少，尤其是在git指令和Github仓库托管的错误追踪上，表现出了惊人的解决能力
这是我第一次感受到AI带来的实实在在的流程效率巨大提升.

初次尝试是想基于[vuepress](https://vuepress.vuejs.org/zh/guide/getting-started.html)框架搭建个人博客，对于只会**markdown**的新手十分友好，
磕磕绊绊的使用git部署到本地进行搭建，第一次了解了markdown文本文件以及如何在本地项目中构建网站主题
``` javascript
// 初始博客的config.js文件内容
module.exports={
	title:'个人博客项目规划',
	description:'entropy_reverser的个人技术分享',
    base:'/Personal-blog-project/',
	themeConfig: {
        subSidebar:'auto',
        nav: [
            { text: '首页', link: '/' },
            { 
                text: 'entropy_reverser的个人博客', 
                items: [
                    { text: 'Github', link: 'https://github.com/entropy-reverser' }
                    
                ]
            }
        ],
        sidebar: [
            {
                title: '个人介绍',
                path: '/',
                collapsable: false, 
                children: [
                    { title: "个人简历", path: "/" }
                ]
            },
            {
              title: "计算机科学与技术(Computer Science and Technology)",
              path: '/handbook/Leedcode',
              collapsable: true, // 不折叠
              children: [
                { title: "Leedcode算法题目分析", path: "/handbook/Leedcode" },
                { title: "计算机系统组成", path: "/handbook/Computer_system_composition" },
                { title:"数据结构与算法",path:"/handbook/Data_Structure_and_Algorithms"},
                { title:"计算机网络",path:"/handbook/computer_network"},
                { title:"操作系统",path:"/handbook/operating_system"},
                { title:"编译原理",path:"/handbook/Compilation_principle"}
              ]
            },
            {
                title:"视觉设计作品展示(Visual design works display)",
                path:'/',
                collapsable:true
            },
            {
                title:"AIGC",
                path:'/',
                collapsable:true
            },
            {
                title:"考研经验(Graduate Exam Experience)",
                path:'/',
                collapsable:true
            }
          ]
    },
    theme:'reco'
}
```
但最终该方案放弃，第一是因为生成的主题太单调，交互性太弱，第二是我在部署到云端服务器时迟到了闭门羹，花了好长的时间都是失败了，最终
决定果断尝试其他方案.

正好之前就有关注过 [GitHub Pages](https://pages.github.com/) + [Jekyll](http://jekyllrb.com/) 快速 Building Blog 的技术方案，非常轻松时尚
最终决定使用github Page部署我的个人网站，更重要是更改博客内容时十分方便，我开始学习github仓库和 git命令管理器的协作，其中在涉及到将github上的
项目部署到本地时我遇到了不少难以追查的错误信息， 文心一言生成工具提供了巨大的帮助

```
我:
我在克隆仓库时遇到该错误信息：SSL certificate problem: unable to get local issuer certificate

文心一言:
当你尝试克隆一个Git仓库时遇到“SSL certificate problem: unable to get local issuer certificate”这个错误，
通常意味着Git客户端在尝试通过HTTPS与远程仓库通信时，无法验证远程服务器提供的SSL证书。这可能是由以下几个原因导致的：

- 系统根证书问题：你的系统可能没有安装或无法找到必要的根证书颁发机构（CA）证书。
- Git配置问题：Git可能配置为使用错误的SSL证书或没有正确配置SSL验证。
- 代理或VPN干扰：如果你通过代理或VPN连接到网络，可能会干扰SSL证书验证。
- 为了解决这个问题，你可以尝试以下几个步骤：

更新系统证书：

在你的操作系统上查找并安装或更新最新的根证书颁发机构证书。
配置Git以忽略SSL验证（注意：这通常不推荐，因为它会降低安全性）：

你可以通过配置Git来忽略SSL证书验证，但这会使你的连接容易受到中间人攻击。
git config --global http.sslVerify false

指定正确的CA证书路径：
git config --global http.sslCAInfo /path/to/ca-certificates.crt

使用SSH代替HTTPS：
如果可能的话，考虑使用SSH URL而不是HTTPS来克隆仓库。SSH通常不会遇到与SSL证书相关的问题。
........
```
这里我就不再赘述，但一些非常刁钻的问题都能给出逻辑严谨的正确的详细的解决方案，其中在将本地项目部署到github仓库时常用的指令:

```
如果你的项目还没有初始化为 Git 仓库，你需要运行以下命令来初始化它：

git init

然后，添加所有文件到 Git 跟踪：

git add .

提交你的更改并添加一条提交信息：

git commit -m "Initial commit for my VuePress blog"

你需要将你的本地仓库推送到 GitHub。首先，你需要添加远程仓库的 URL或者SSH：

git remote add origin https://github.com/username/your-repo-name.git

然后，推送你的更改到 GitHub：

git push -u origin master（注意：如果你的默认分支不是 master 而是 main，请将 master 替换为 main）
```


使用Git page搭建博客的优点非常明显：

* **Markdown** 带来的优雅写作体验
* 非常熟悉的 Git workflow ，**Git Commit 即 Blog Post**
* 利用 GitHub Pages 的域名和免费无限空间，不用自己折腾主机
	* 如果需要自定义域名，也只需要简单改改 DNS 加个 CNAME 就好了 
* Jekyll 的自定制非常容易，基本就是个模版引擎


---


大的 Jekyll 主题上直接 fork 了 Clean Blog（这个主题也相当有名，就不多赘述了。唯一的缺点大概就是没有标签支持，于是我给它补上了。）

本地调试环境需要 `gem install jekyll`，结果 rubygem 的源居然被墙了……后来手动改成了我大淘宝的镜像源才成功

Theme 的 CSS 是基于 Bootstrap 定制的，看得不爽的地方直接在 Less 里改就好了（平时更习惯 SCSS 些），**不过其实我一直觉得 Bootstrap 在移动端的体验做得相当一般，比我在淘宝参与的团队 CSS 框架差多了……**所以为了体验，也补了不少 CSS 进去

最后就进入了耗时反而最长的**做图、写字**阶段，也算是进入了**写博客**的正轨，因为是类似 Hack Day 的方式去搭这个站的，所以折腾折腾着大半夜就过去了。




## 后记


如果你恰好逛到了这里，希望你也能喜欢这个博客主题。

