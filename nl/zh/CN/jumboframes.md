---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-11"

---
{:pre: .pre}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# 在 {{site.data.keyword.BluSoftlayer_notm}} 中启用巨型帧 - Windows 和 Linux

巨型帧是一种以太网帧，其有效内容大于标准的最大传输单元 (MTU) 1,500 字节。巨型帧在至少支持 1 Gbps 的局域网上使用，并且最大可达 9000 字节。

需要将巨型帧配置为在从“源设备”<->“交换机”<->“路由器”<->“交换机”<->“目标设备”的整个网络路径上相同。如果未将整个链设置为相同值，那么将缺省为链上的最低设置。目前，{{site.data.keyword.BluSoftlayer_full}} 的网络设备设置为 9,000。所有客户设备都需要设置为相同的值 9,000。
{:important}

## 在 Windows 中启用巨型帧

1. 打开**网络和共享中心**。
2. 单击**更改适配器设置**。
3.  、右键单击要为其启用巨型帧的 NIC，然后选择**属性**。
4. 在**网络**选项卡下，单击网络适配器的**配置**。
5. 选择**高级**选项卡。
6. 选择**巨型帧**，然后将值从**已禁用**更改为所需的值。值（例如，9 kB 或 9014 字节）取决于 NIC。
7. 单击所有窗口中的**确定**。

在您进行更改时，NIC 会有几秒钟时间失去网络连接。重新启动设备以确认更改生效。
{:tip}


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
