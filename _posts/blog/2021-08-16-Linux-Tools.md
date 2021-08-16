---
layout:         post
title:          Linux 常用小工具整理
category:       blog
description:    整理 Linux 环境下排查问题的常用工具
---

## 文件传输-断点传序

- scp

    ```bash
    rsync -P --rsh=ssh home.tar 192.168.205.34:/home/home.tar
    # -P: 是包含了 “–partial –progress”， 部分传送和显示进度
    # -rsh=ssh 表示使用ssh协议传送数据
    ```

- wget

    ```bash
    wget -c -t 100 http://tarballs.99cloud.com.cn/animbus8/release/x86_64/IaaS-animbus-8.1.1-rc.tar
    ```

## 后台进程保持

- screen

    ```bash
    screen -S david

    # Ctrl+A d

    screen -ls

    screen -r 12865
    ```

- tmux

    ```bash
    tmux new -s roclinux

    # Ctrl+B d

    tmux ls

    tmux a -t roclinux
    ```

## 容器镜像工具包

- busybox

    ```bash
    kubectl run curl --image=radial/busyboxplus:curl -i --tty

    nslookup hello-python-service
    curl http://hello-python-service.default.svc.cluster.local:6000

    # 在不同的 namespaces 或者宿主机节点上，需要 FQDN 长名
    nslookup hello-python-service.default.svc.cluster.local 10.96.0.10
    ```
