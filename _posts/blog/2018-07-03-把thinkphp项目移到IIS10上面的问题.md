---
layout: post
title: 把thinkph项目移到IIS10上面出现的问题
data: 2018-07-03 11:10:00 +0800
categories: PHP
tag: VIM , 笔记
---
* content
{:toc}

## 原先在项目中设置的路由失效的问题

由于IIS10没有URL重写的模块，所以原先在router.php配置的路由不能用，点击会报404找不到文件错误，按照tp的手册在web.config中system.server节点中配置如下的代码：
```
<rewrite>
 <rules>
 <rule name="OrgPage" stopProcessing="true">
 <match url="^(.*)$" />
 <conditions logicalGrouping="MatchAll">
 <add input="{HTTP_HOST}" pattern="^(.*)$" />
 <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
 <add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" />
 </conditions>
 <action type="Rewrite" url="index.php/{R:1}" />
 </rule>
 </rules>
 </rewrite>
```

这个时候会发现打开网站提示500.19错误，里面说明提示配置文件web.config错误。这时候需要安装URL重写模块，去官网[下载地址](https://www.microsoft.com/zh-cn/download/confirmation.aspx?id=7435) 下载IIS7(32位，64位)Microsoft URL重写模块2.0. 下载后安装需要注意由于IIS版本较高，需要更改注册表：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\InetStp 中把MajorVersion改为9，安装完成后改回原值即可。安装完成后重启项目即可正常浏览

- 参考
1. https://yq.aliyun.com/wenzhang/show_166717
2. https://www.jy14.com/server/4507.html
