---
layout:         post
title:          Linux 网络-逆向学习问题总结
category:       course
description:    逆向法学习 Linux 网络，关于 Linux 网络需要能回答出的问题
---


## 问题分类
- 核心知识
	- **基础**: TCP/IP，iptables ( ipvs )，route，namespace，Linux Bridge
	- **进阶**: ovs（ ovn ），calico
- 常见应用
	- ** OpenStack **: vpn / lb / firewall / nat
	- ** Kubernetes **: CNI / MetalLB

---------------------------------------
---------------------------------------


## 核心知识

### 基础

1. [TCP/IP] 怎么翻墙？

1. [TCP/IP] 三次握手，四次分手？

1. [TCP/IP] 怎么判断和追踪端口泄漏问题？netstat

1. [TCP/IP] 怎么抓包（ tcpdump ），怎么分析（ wireshark ）？

1. [TCP/IP] 怎么分析 TLS 握手失败问题？

1. [TCP/IP] 怎么远程自动登录（ 跳板机 ）？ssh cert & config

1. [TCP/IP] 怎么优化 SSH，使其在网络环境不佳时不容易卡死？

1. [TCP/IP] 怎么配置 ipv6 网络？

1. [TCP/IP] 什么是 vlan？怎么配置 vlan？

1. [TCP/IP] 什么是 vxlan，怎么配置 vxlan？

1. [TCP/IP] 什么是 overlay 和 underlay？怎么理解 underlay 的 L2 / L3 兼容？

1. [iptables] iptables 几张表几条链？各自什么作用？

1. [iptables] 怎么持久化（ save ） iptables 规则？

1. [iptables] 怎么配置一个 forward 规则？怎么打开 ipv4 转发？

1. [iptables] 怎么配置一个 SNAT？

1. [iptables] 配置 DNAT 时，为了让包回去，要怎么配置路由或者 SNAT？

1. [iptables] ipvs 有什么优势？怎么启用 ipvs？

1. [route] 怎么判断从某个 IP 能 ping 通目的地址？

1. [route] 怎么配置策略路由？

1. [route] 怎么进行路由追溯？traceroute？

1. [route] 怎么进行 tcpping？

1. [route] 什么是 BGP？怎么配置 BGP 路由？

1. [namespace] 什么是 namespaces？

1. [namespace] namespaces 的基本使用方法和排错步骤

1. [namespace] 虚拟网卡上的抓包方法

1. [bridge] Linux Bridge 的基本用法和排查步骤

1. [bridge] Linux Bridge 什么时候 L3 IP 不起作用？

1. [bridge] Linux Bridge 和 OVS 相比有什么缺陷？

### 进阶

1. [ovs] OVS 的基本用法

1. [ovs] OVS 的问题排查思路和恢复手段，比如流表检查？

1. [ovs] OVN 的优势？

1. [ovs] OVN 的基本用法和排查思路？

1. [calico] Calico 的基本用法

1. [calico] Calico 的隧道模式基本使用

1. [calico] Calico 的 BGP 模式基本用法

## 常见应用

### OpenStack

1. [vpn] VPNaaS 的基本用法

1. [vpn] VPNaaS 的排查思路

1. [LB] 怎么配置 haproxy？

1. [LB] 怎么配置 nginx？

1. [LB] 怎么配置 keepalive VIP？

1. [LB] octavia 的原理和基本用法

1. [LB] octavia 的排错思路

1. [FW] FWaaS 的基本用法

1. [FW] FWaaS 的排查思路

1. [NAT] NATaaS 的基本用法

1. [NAT] NATaaS 的排查思路

### Kubernetes

1. [CNI] CNI 的原理和排查思路

1. [CNI] Calico CNI 的基本用法和排查思路

1. [CNI] Flannel CNI 的基本用法和排查思路

1. [CNI] Multus 网络平面的配置方案和排查思路

1. [CNI] IPv6 / IPv4 双栈解决方案，基于 Calico 和基于 Multus

1. [CNI] SRIOV 的基本用法和排查思路

1. [CNI] MacVlan 的基本用法和排查思路

1. [CNI] DPDK 的基本用法和排查思路

1. [MetalLB] MetalLB 的适用场景，是否支持 ipvs？

1. [MetalLB] MetalLB 的二层模式配置方法，有哪些弱点？单点故障 / 10s 切换

1. [MetalLB] MetalLB 的 BGP 模式配置方法


