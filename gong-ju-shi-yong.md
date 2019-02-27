当有了可用的开发环境后，接下来就可以使用Robin framework创建一个Dapp了。Robin framework是一个NodeJS编写的全局命令行程序接口。

它提供以下服务：

* 一键式合约初始化、编译与部署；

* 自动化合约测试与开发；

* 友好的代码审查与错误提示；

* 大量的合约模板与示例参考；

* 脚本式与可配置化部署流程；

* 交互式合约日志控制台输出；

**要求**

NodeJS 8.0+

Linux、MacOSX、Windows

**安装**

`sudo npm install -g robin-framework`

**创建工程**

执行`robin`or`robin -h`来查看所有的robin子命令

要启动项目，首先需要创建一个新的空目录，然后进入目录：

`mkdir testing    
 cd testing`

然后初始化一个项目。使用`-c`or`--contract`来指定名称。此时，你有多个模板可以选择，默认的是纯合约项目，其余的是带界面的DAPP框架。

`robin init`![](/assets/图片 1 %281%29.png)

