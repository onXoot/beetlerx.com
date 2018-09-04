---
title: 阿里云创建 maven 私有仓库并上传 jar 包
date: 2018-09-04
tags: maven
categories: linux 工具
---

### 创建账号
已有的可以跳过这一步
#### 打开 maven.aliyun.com 
点击 maven 私有仓库服务

![ippEYd.md.png](https://s1.ax1x.com/2018/09/04/ippEYd.md.png)

#### 登录
账号,为了方便，我这里直接选择支付宝扫码登录

![ippMm8.md.png](https://s1.ax1x.com/2018/09/04/ippMm8.md.png)

#### 创建企业
已有的可以跳过这一步
打开如果没有企业，会提示创建企业，点击新建第一家企业
![iSxzN9.md.png](https://s1.ax1x.com/2018/09/04/iSxzN9.md.png)
然后输入企业名称,名字随便写
![iSzijK.md.png](https://s1.ax1x.com/2018/09/04/iSzijK.md.png)

这里点击立即创建之后可能出错，不过不要紧，直接略过就行，到下一步
重新打开 maven.aliyun.com,点击私有仓库服务，就有了，可以看到下面样子
![ippOnf.png](https://s1.ax1x.com/2018/09/04/ippOnf.png)

### 本地使用
#### 配置 maven
点击上图中的 仓库配置按钮，按照提示中的说明分别添加 servers 和 profiles， 
下面是我的配置，红框内是新加的配置，注意我改了id名字，后面要用这个
![ip9Shj.png](https://s1.ax1x.com/2018/09/04/ip9Shj.png)
#### 项目配置
在本地的项目pom配置中添加要发布的舱库 id 和 url,注意红框里的id 是刚才配置的 server id 
![ip9ABT.png](https://s1.ax1x.com/2018/09/04/ip9ABT.png)

#### 发布 jar 包
在项目的pom文件中添加下面的代码，指定要打包的类型为 jar 包
```
<packaging>jar</packaging>
```
推荐加上 jar 包原码插件，这样在 idea 里面可以直接下载 jar 包的原码，在 build 的 plugins 里面加上下面的代码
```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-source-plugin</artifactId>
    <version>3.0.1</version>
    <executions>
        <execution>
            <id>attach-sources</id>
                <goals>
                <goal>jar</goal>
                </goals>
            </execution>
        </executions>
</plugin>
```
然后执行 `mvn clean package deploy` 就能上传到自己的仓库了
如果出现
` Return code is: 409, ReasonPhrase: Overwriting released artifacts is not allowed..`
这样的错误，说明已经上传成功了，刷新一下网页就能看到了

上传后自己就能用了，下面是我上传的，复制红框里的坐标就能用了
![ip9Qjx.png](https://s1.ax1x.com/2018/09/04/ip9Qjx.png)

### 示例代码
文中 hello-world 的代码
github: https://github.com/onXoot/jar-hello-world
打不开github的可以访问 oschina
oschina: https://gitee.com/onXoot/jar-hello-world
