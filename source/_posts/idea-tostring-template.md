---
title: idea tostring template
date: 2018-08-28 11:57:02
tags: [idea, toString, template]
categories: "idea 配置相关"
---
idea 是一款强大的ide, 在编写java 代码是可大大提高我们的效率，不仅可以一键生成 getter setter 方法,
还能生成 toString 方法，下面介绍一些如何生成 json 风格的 toString方法

##### 在代码处右键选择 generate
![generate](https://s1.ax1x.com/2018/08/30/PjVwx1.jpg)

##### 然后选择 toString
![toString](https://s1.ax1x.com/2018/08/30/PjZlJH.png)

##### 下拉 template 可以看到 idea 自带的各种模版
![PjZrSs.md.png](https://s1.ax1x.com/2018/08/30/PjZrSs.md.png)

点击 setting , 然后点击加号 输入模版名称，比如json 然后在右边的编辑框输入文章最下面下面的代码

![Pjmfz9.md.png](https://s1.ax1x.com/2018/08/30/Pjmfz9.md.png)

注意，下面的代码粘贴进去的话，idea 会自动格式化，导致生成的代码中有很多空格
用鼠标选中每一行前面的(红框选中的)空格，删除，直接用退格删除的话会跑到上一行去，再敲回车又变回来了
删到和图片下面代码显示的格式一样
![PjnUw6.md.png](https://s1.ax1x.com/2018/08/30/PjnUw6.md.png)
```
public java.lang.String toString() {
#if ( $members.size() > 0 )
#set ( $i = 0 )
return "{" +
#foreach( $member in $members )
#if ( $i == 0 )
    "##
#else
    ",##
#end
#if ( $member.objectArray )
#if ($java_version < 5)
$member.name=" + ($member.accessor == null ? null : java.util.Arrays.asList($member.accessor)) +
#else
$member.name=" + java.util.Arrays.toString($member.accessor) +
#end
#elseif ( $member.primitiveArray && $java_version >= 5)
$member.name=" + java.util.Arrays.toString($member.accessor) +
#elseif ( $member.string )
\"$member.name\":\"" + $member.accessor + '\"' +
#else
\"$member.name\":" + $member.accessor +
#end
#set ( $i = $i + 1 )
#end
"}";
#else
return "$classname{}";
#end
}
```
##### 使用
按照刚才的方法设置好模版之后，下次生成toString 方法的时选择 json 的模版就可以了
![Pjue9e.md.gif](https://s1.ax1x.com/2018/08/30/Pjue9e.gif)
