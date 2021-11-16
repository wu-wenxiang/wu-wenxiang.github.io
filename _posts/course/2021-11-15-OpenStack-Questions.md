---
layout:         post
title:          OpenStack 逆向学习问题总结
category:       course
description:    逆向法学习 OpenStack，关于 OpenStack 需要能回答出的问题
---

## 问题分类

- **虚拟化**
- **环境搭建**
- **Keystone**
- **Glance**
- **Nova**
- **Cinder**
- **Neutron**


---------------------------------------
---------------------------------------


## 虚拟化

1. 什么是 Host / Guest / Hypervisor？

    宿主机 / 虚拟机 / Host 通过 Hypervisor 将自己的硬件（CPU/内存/IO）虚拟化，提供给 Guest 使用。

1. 什么是全虚拟化 / 半虚拟化？各有什么优势？

    全虚拟化（Xen/ESXi）是一个特殊定制的 Linux 系统，也叫 1 型虚拟化，对硬件虚拟化功能做了特别优化，性能上比半虚拟化要高。半虚拟化基于普通操作系统，比较灵活，支持虚拟机嵌套，比如 KVM 中再运行 KVM。

1. **什么是 KVM？**

    Kernel-Based Virtual Machine，通过内核模块 kvm.ko 来管理虚拟 CPU 和内存。KVM 只关注虚拟机调度和内存管理，IO 外设虚拟化（存储和网络）交给 Linux 内核和 Qemu。

1. **什么是 QEMU？**

    QEMU A generic and open source machine emulator and virtualizer. Full-system emulation. Run operating systems for any machine, on any supported architecture. User-mode emulation.

1. **什么是 Libvirt？**

    Libvirt 是 Hypervisor 管理工具，可以管理 KVM/Xen/VirtualBox。Libvirt 包含 3 个东西：后台 daemon程序 Libvirtd、API 库（virt-manager 就是用了 API）、命令行工具 virsh。

## 环境搭建

1. KVM

## Keystone

## Glance

## Nova

## Cinder

## Neutron
