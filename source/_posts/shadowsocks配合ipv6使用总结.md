---
title: shadowsocks配合ipv6使用总结
date: 2020-02-04 20:32:13
tags:
    - linux
---

ip被*的情况下，通过配置ipv6隧道方式，达到更换ip的效果（也可以通过ipv6地址使用ssh远程连接服务器，亲测）
<!-- more -->

#### 前提条件
搬瓦工vps kvm，centOS 8

####一、申请ipv6隧道地质
1. 访问 https://www.tunnelbroker.net，注册并登录，本人使用chrome浏览器进行注册时，总是提示资料错误，注册失败，后来改用safari后，注册成功
2. 点击左侧 create regular tunnel，输入你的服务器ip地址，并选择服务器位置（貌似可以随意选择，本人未曾尝试）
3. 生成后，先点击 Example Configurations，然后选择 Linux net tools 选项（根据操作系统选择，本人操作系统为centOS 8），生成配置代码
    
####二、配置服务器
1. 连接服务器，若ip被*，可使用搬瓦工 kiviVM 管理面板进行登录
2. 将上面步骤3生成的配置代码，输入并执行
3. 执行后，验证是否配置成功，输入 ping6 ipv6.google.com，如果可以ping通，说明ipv6配置成功

####三、设置ipv6开机启动
1. 输入 ``vi /root/ipv6.sh`` 进入ipv6配置文件
2. 对文件进行编辑
    ```
    #!/bin/bash
   
    // 再次将上面的配置代码复制到此处
    ```
3. 给文件可执行权限 ``chmod +x /root/ipv6.sh``
4. 编辑 ``rc.local`` 文件
    ```
   vi /etc/rc.d/rc.local
   ```
5. 加入一行代码 ``sh /root/ipv6.sh``
6. 结束后，重启系统也可以正常使用了

####四、服务器安装 shadowsocks 客户端
1. 安装教程参考本人之前的文章即可。
2. 安装完成后，需要将 shadowsocks 配置文件的 server 属性值改为 ::，并重启 shadowsocks
```
{
    "server":"::",
    "server_port":17406,
    "local_port":1080,
    "password":"your password",
    "method":"aes-256-cfb",
    "timeout":300
}
```

####五、客户端配置 shadowsocks
1. 将地址改为 ipv6 的地址即可（后面的 /64 不需要携带）

####结束。

