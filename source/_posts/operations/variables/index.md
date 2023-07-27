---
title: "Linux 设置环境变量"
description: "优雅的在 Linux 上设置环境变量"
date: 2022/11/04 16:20:51
abbrlink: 0e62ab6b
#image: "cover.png"
tags: [Environment Variables, Linux]
categories: Operations
---

## 现有教程存在的问题

现在的教程，不是放在 `/etc/environment`、`/etc/profile` 就是 `~/.bashrc`

- `/etc/environment` 及 `~/.config/environment.d/` 仅接受 `key=value` 格式，这将导致部分重复使用变量的写法不可用，比如：

    ```bash
    export $JAVA_HOME="/path/to/jdk"
    export $PATH="${JAVA_HOME}/bin:${PATH}"
    ```

- `~/.bashrc` 仅对 bash 中运行的程序有效，对于桌面程序无效
- `/etc/profile` 为全局变量，会造成用户 Token 泄露

## 优雅的解决方案

- 使用 `/etc/profile.d/usr_env.sh` 加载对应用户的的 `~/.local/profile.d/*.sh`
- 对应用户只会加载自己目录下的环境变量文件，**不会将一些关于密钥的环境变量泄露到其他用户**
- 环境变量在对应用户的作用域下是全局生效的，不局限于终端
- 如果要设置系统变量，创建并编辑 `/etc/profile.d/global.sh`

1. 创建并编辑 `/etc/profile.d/usr_env.sh`

    ```bash
    #!/bin/sh
    if test -d ${HOME}/.local/profile.d; then
        for profile in ${HOME}/.local/profile.d/*.sh; do
            test -r "$profile" && . "$profile"
        done
        unset profile
    fi
    ```

2. 创建 `~/.local/profile.d` 文件夹并创建编辑 `*.sh`，比如 `bin.sh`:

    ```bash
    if test -d ${HOME}/.local/bin; then
        export PATH="${HOME}/.local/bin:${PATH}"
    fi
    ```

3. 注销后重新登录生效