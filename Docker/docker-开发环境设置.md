---
title: docker 开发环境设置
date: '2018-10-19'
tags:
  - docker
---

# 本机开发与 docker

...

## 目标


启动项目时明显优于本机项目启动,开始时媲美本机开发体验.

+ 不配置环境,快速的启动项目
+ 本机浏览器 IP 访问项目,如:`localhost:300`
+ 项目目录在本机上,可以通过编辑器编辑,如:工程目录在本机的:`/home/xxx/a-projcet/`,而非 `docker:/usr/src/app` 里面
+ 可以进入容器内部执行 Shell 命令,如:`yarn add xxx`,`yarn test`

## 环境

现在 docker 已经实现了三大平台的支持.Mac 和 Linux 的安装相对简单,Windows 相对而言比较麻烦一点.

笔者以 Centos 为例, 介绍以下 docker 的安装步骤.其余平台可参考笔者给出的两个连接选择和自己系统相符的指南,不建议使用在 Window10 折腾.

[官方文档-CentOS安装参考](https://docs.docker.com/install/linux/docker-ce/centos/#prerequisites)

[docker 入门和实践-安装参考](https://bingohuang.gitbooks.io/docker_practice/content/install/centos.html)(笔者参考的指南)

<!-- ### 添加源

```shell
$ sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/7/
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
EOF
```

### 安装 docker

```
$ sudo yum update
$ sudo yum install docker-engine
```

### 启用 dokcer

```
$ sudo systemctl enable docker
$ sudo systemctl start docker
```

### docker 用户组

因为 docker 命令只有,**root**用户和**docker**用户组才有权限执行,所有要把当前用户加入**docker**用户组.

```
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
```

>**注意:这里添加到 docker 用户组需要先登出才会生效** -->

# 示例

下面一个 Koa 的项目来演示,如何使用 docker 建立一个开发环境.

本机开发,我们必须的在预先安装好开发环境,比如这里我们需要的 NodeJS. 常常会出现的问题就是,多人开发时如果开发环境的版本不一致的话,就会出现比如:依赖安装失败,特别是相差一个大版本时(python2.x和python3.x),还有可能无法 running, 突然间迷之死掉.

这实际上就是开发环境的版本管理,往往多个项目依赖的着不同的版本,你需要文能够切换不同的版本,为此许多语言都有推出各自的版本控制工具,比如: NodeJS的`nvm`,Python的`pyenv`,Ruby 的`rvm`等等.

而使用 docker 开发就不存在这个问题,预先将所需要的开发环境打包到镜像里面,统一使用同一个镜像就不会存在开发环境不一致的问题了.

## 拉取镜像

```
$ docker pull node
```

一边等待拉取完成,补充一点,只有官方镜像不需要加前缀名,非官方的镜像需要加上镜像所有者的前缀,比如`docker pull username/my-node`,换句话说,如果没有前缀名那么就会默认加上`library/`官方库的前缀.
