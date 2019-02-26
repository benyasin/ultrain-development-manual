Ultrain平台环境可以分为线下开发环境、线上测试网环境与线上主网环境。其中，线上主网环境需要购买资源套餐后方可使用，线上测试网环境可经水龙头程序自行充值后使用，线下开发环境是指在本地自行构建的网络共识环境。以下篇幅重点介绍线下开发环境的搭建流程。

我们推荐首选的操作系统为MacOS，其次是Linux与Windows。

Ultrain线下开发环境借助于Docker来构建，所以需要你在本机上提前安装并启动Docker。关于Docker安装及使用可以参考阮一峰的[Docker入门教程](http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)。

![](/assets/longclaw.png)

我们使用集成工具Longclaw来构建本地共识网络环境。首先在开发者网站上下载相应系统的[Longclaw](https://developer.ultrain.io/tools)，安装并启动，Longclaw第一次初始化环境可能要花费几分钟，需要你耐心等待，当出现以下界面，则说明Longclaw已经成功帮你构建了共识网络环境，也就是本地开发环境。

![](/assets/longclaw1.png)

Longclaw为开发者默认创建了八个测试账号，这些账号拥有无限的资源使用权。当然如果你通过程序接口自行创建了账号，则默认是没有可用资源的，需要调用购买资源套餐的接口购买资源。

注意：如果在Linux与Window下，Longclaw因不兼容问题,导致不能正常启动，建议直接连接线上测试网环境进行开发，相关配置可参考教程[测试公链开发配置指南](https://developer.ultrain.io/tutorial/testnet_guide)。

