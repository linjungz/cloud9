## 关于AWS Cloud9
![Cloud9](https://d1.awsstatic.com/product-marketing/Tulip/AWS_Cloud9_Asset01_R3_P.22c006faf1258710ffbdd756ec83ea97449e9da3.png)
[AWS Cloud9](https://aws.amazon.com/cn/cloud9/?nc1=h_ls) 是一种基于云的集成开发环境 (IDE)，您只需要一个浏览器，即可编写、运行和调试代码。它包括一个代码编辑器、调试程序和终端。Cloud9 预封装了适用于 JavaScript、Python、PHP 等常见编程语言的基本工具，您无需安装文件或配置开发计算机，即可开始新的项目。Cloud9 IDE 基于云，因此您可以从办公室、家中或任何地方使用已连接互联网的计算机完成项目。Cloud9 还可以为开发无服务器应用程序提供无缝体验，使您能够轻松定义资源、进行调试，并在本地和远程执行无服务器应用程序之间来回切换。借助 Cloud9，您可以与团队快速共享开发环境，从而能够将程序配对，并实时跟踪彼此的输入。

## 在AWS国内区域部署Cloud9
目前AWS Cloud9暂未落地在中国区域, 因此无法在控制台上直接启动一个Cloud9 IDE. 不过由于[Cloud9 Core](https://github.com/c9/core)是开源的，因此我们可以通过在EC2上部署Cloud9 Core来达到使用Cloud9 IDE的目的。

### 通过CloudFormation模板一键启动Cloud9

如下是CloudFormation模板的链接，可以在北京区域或是宁夏区域的默认VPC中部署一台安装了Cloud9 Core的EC2实例，模板部署完成后通过一个链接即可打开Cloud9 IDE.

AWS区域名称 | AWS区域代号|模板链接
--- | --- | ---
北京区域 | cn-north-1| [![](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://cn-northwest-1.console.amazonaws.cn/cloudformation/home?region=cn-north-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fs3.cn-northwest-1.amazonaws.com.cn%2Fcf-templates-lvjleo0xe30a-cn-northwest-1%2F2019283IJ0-cloud9-china.json&stackName=Cloud9&param_c9Password=passw0rd&param_c9UserName=aws)
宁夏区域 | cn-northwest-1 | [![](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://cn-northwest-1.console.amazonaws.cn/cloudformation/home?region=cn-northwest-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fs3.cn-northwest-1.amazonaws.com.cn%2Fcf-templates-lvjleo0xe30a-cn-northwest-1%2F2019283IJ0-cloud9-china.json&stackName=Cloud9&param_c9Password=passw0rd&param_c9UserName=aws)

#### 工作原理

整个模板的工作内容非常简单

#### 1. 创建安全组
默认打开了22端口(SSH访问)和8181端口(Cloud9 Web访问)

#### 2. 部署EC2实例
基于自定义的AMI创建EC2实例，该AMI中已经安装好Cloud9 Core并配置为systemd的service。通过User Data来进行Cloud9 Core的初始化配置。User Data中运行的命令主要将CloudFormation模板中输入的用户名密码参数值更新到systemd service文件中，并重启服务使变更生效。

### 通过AMI启动Cloud9
