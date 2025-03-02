---
title: VPS
date: 2025-03-02T16:38:06+08:00
draft: true
tags: [Server]
---
前不久收到CloudCone的情人节促销邮件，看了下虽然阿里云/[云小站](https://www.aliyun.com/minisite/goods)上也有一些优惠，不过想获得完整体验的话可能需要各种备案什么的吧，感觉[CloudCone](https://cloudcone.com/)价格还挺合适的，就买了台机器，3 Core & 2G RAM & 240G DISK，$24.99/y，续费同价？然后折腾了一番，这里做一些相关配置步骤的记录。

<!--more-->

## 创建新用户
> 接下来已sammy作为用户名为例

### 本地登录远程服务器
在CloudCone完成订单以后，过一会就会收到相应的信息（IP&Username&Password）端口默认`22`。

```bash
ssh root@xx.xxx.xx.xxx
```

### 修改密码

```bash
passwd
```

### 更新系统(Debian)

```bash
apt update && apt full-upgrade -y && apt autoremove -y
```

### 安装vim(可选)

> 不安装也可以使用`nano`文本编辑器，可以依据个人使用习惯
```bash
apt install sudo vim -y
```

### 创建用户

```bash
adduser sammy # 根据提示填写信息
```

### 赋予 sudo 权限

```bash
usermod -aG sudo sammy
```

### 本地登录新用户

```bash
ssh sammy@xx.xxx.xx.xxx
```

### 设置 SSH 登录

```bash
# 1.本地生成钥匙对
ssh-keygen -t rsa -b 4096

# 2.将其中的公钥上传到服务器上，文件路径与第一步生成的文件一致
ssh-copy-id -i ~/.ssh/key_file_name sammy@xx.xxx.xx.xxx

# 3.修改本地ssh config文件，创建alias
vi ~/.ssh/config
```

在config文件中，添加如下格式（私钥文件路径同上）
```bash
# ~/.ssh/config
Host sammy_server                     # 别名  
    HostName xx.xxx.xx.xxx            # 替换 xx.xxx.xx.xxx 为服务器 ip 地址  
    Port xx                           # 替换 xx 为对应服务器SSH端口
    User sammy                        # 用户名  
    IdentityFile ~/.ssh/key_file_name # 私钥文件
```

接下来本地就可以使用alias登录服务器
```bash
ssh sammy_server
```

到现在为止已经在服务器上创建`sammy`新用户，并在本地生成了ssh密钥，上传到了服务器中。
接下来的命令操作，需要已`sammy`用户的身份进行

## 安全设置

编辑服务器 `/etc/ssh/sshd_config` 文件：
```bash
sudo vim /etc/ssh/sshd_config
```
### 禁止root登录

```bash
# /etc/ssh/sshd_config
# 将PermitRootLogin改为no
PermitRootLogin no
```

### 禁止密码登录

```bash
# /etc/ssh/sshd_config
# 将PasswordAuthentication改为no
PasswordAuthentication no
```

### 更改 SSH 端口

```bash
# /etc/ssh/sshd_config
# 将Port改为想设置的端口
Port xx # 替换 xx 设置为服务器SSH端口，并相应修改本地~/.ssh/config文件保持一致
```

### 重启服务
```bash
sudo systemctl restart sshd
```

OK，到这里，基本设置就完成啦～，当然后续还可以安装ufw，设置防火墙，或者选择直接安装[1Panel](https://1panel.cn/)，通过 Web 图形界面管理服务器，应用商店中也有丰富的开源工具和应用软件。