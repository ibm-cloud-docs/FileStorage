---

copyright:
  years: 2014, 2019
lastupdated: "2019-08-01"

keywords: File Storage, file storage, NSF, networking, jumbo frames

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# 在 {{site.data.keyword.BluSoftlayer_notm}} 中启用巨型帧
{: #jumboframes}

巨型帧是一种以太网帧，其有效内容大于标准的最大传输单元 (MTU) 1,500 字节。巨型帧在至少支持 1 Gbps 的局域网上使用，并且最大可达 9000 字节。

需要将巨型帧配置为在从“源设备”<->“交换机”<->“路由器”<->“交换机”<->“目标设备”的整个网络路径上相同。如果未将整个链设置为相同值，那么将缺省为链上的最低设置。目前，{{site.data.keyword.cloud}} 的网络设备设置为 9,000。所有客户设备都需要设置为相同的值 9,000。
{:important}

## 在 Linux 中启用巨型帧

1. 编辑 eth0 接口的网络配置文件。
   - CentOS、RHEL、Fedora Linux 用户编辑 `/etc/sysconfig/network-script/ifcfg-eth0`
     ```
     # vi /etc/sysconfig/network-script/ifcfg-eth0
     ```
     {: pre}

   - Debian 和 Ubuntu Linux 用户编辑 `/etc/network/interfaces`。

2. 附加以下配置伪指令，用于指定以字节为单位的帧大小。
   - CentOS、RHEL、Fedora Linux
     ```
     MTU 9000
     ```
     {: pre}

   - Debian 和 Ubuntu Linux
     ```
     MTU=9000
     ```
     {: pre}

3. 关闭并保存文件。重新启动接口 eth0。
   ```
   # /etc/init.d/networking restart
   ```
   {: pre}

   此操作会导致短暂失去网络连接。
   {:important}
