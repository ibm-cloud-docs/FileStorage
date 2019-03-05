---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# 通过 VMware 供应 {{site.data.keyword.filestorage_short}}
{: #architectureguide}

以下步骤可帮助您在 vSphere 5.5 和 vSphere 6.0 环境中通过 {{site.data.keyword.BluSoftlayer_full}} 订购和配置 {{site.data.keyword.filestorage_full}}。

{{site.data.keyword.filestorage_short}} 旨在支持需要可预测性能级别的高 I/O 应用程序。通过为各个卷分配协议级别每秒输入/输出操作数 (IOPS)，可实现可预测的性能。

如果需要访问 VMware 数据存储的主机超过八个，那么最好选择 NFS {{site.data.keyword.filestorage_short}}。
{:tip}

{{site.data.keyword.filestorage_short}} 产品可通过 NFS 连接进行访问和安装。在 VMware 部署中，单个卷最多可以安装到 64 个 ESXi 主机作为共享存储器。还可以安装多个卷来创建存储器集群，以使用 vSphere 存储器分布式资源调度程序 (DRS)。

{{site.data.keyword.filestorage_short}} 的定价及配置选项根据保留的空间和所提供 IOPS 的组合收费。

## 订购注意事项

订购 {{site.data.keyword.filestorage_short}} 时，请考虑以下信息：

- 确定大小时，请考虑所需的工作负载和吞吐量的大小。对于“耐久性”服务，大小很重要，该服务的性能扩展与容量 (IOPS/GB) 呈线性关系。“性能”服务则不同，它允许管理员独立地选择容量和性能。对于“性能”服务，吞吐量需求很重要。

  吞吐量的计算公式是 IOPS x 16 KB。IOPS 基于 16 KB 的块大小进行度量，其中读/写操作构成比例为 50/50。<br/>增加块大小会增加吞吐量，但会降低 IOPS。例如，如果使块大小翻倍为 32 KB 的块，那么将保持最大吞吐量，但 IOPS 会降低一半。
  {:note}

- NFS 使用许多额外的文件控制操作，例如 `lookup`、`getattr` 和 `readdir`。这些操作可以与读/写操作一样计为 IOPS，并根据操作类型和 NFS 版本而变化。
- {{site.data.keyword.filestorage_short}} 卷将公开给已授权的设备、子网或 IP 地址。
- 为了避免在路径故障转移期间断开存储器连接，{{site.data.keyword.IBM}} 建议安装 VMware 工具，以用于设置适当的超时值。无需更改此值，缺省设置可足以确保 VMware 主机不会失去连接。
- {{site.data.keyword.BluSoftlayer_full}} 环境支持 NFS V3 和 NFS V4.1。但是，{{site.data.keyword.IBM}} 建议您使用 NFS V3，因为 NFS V4.1 是有状态协议（NFSv3 是无状态协议），在网络事件期间可能会发生协议问题。NFS V4.1 必须停止所有操作，然后才能完成锁定回收。这些操作正在执行时，可能会发生中断。

有关更多信息，请参阅 VMware 的白皮书 [Best Practices for running VMware vSphere on network-attached storage Storage![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/vmware-nfs-bestpractices-white-paper-en.pdf){:new_window}
{:tip}

**NFS 协议与 VMware 功能支持矩阵**
<table>
  <caption>表 1 显示了适用于两个不同版本 NFS 的 vSphere 功能。</caption>
 <thead>
  <tr>
   <th>vSphere 功能</th>
   <th>NFS V3</th>
   <th>NFS V4.1</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>vMotion 和 Storage vMotion</td>
   <td>是</td>
   <td>是</td>
  </tr>
  <tr>
   <td>高可用性 (HA)</td>
   <td>是</td>
   <td>是</td>
  </tr>
  <tr>
   <td>容错 (FT)</td>
   <td>是</td>
   <td>是</td>
  </tr>
  <tr>
   <td>分布式资源调度程序 (DRS)</td>
   <td>是</td>
   <td>是</td>
  </tr>
  <tr>
   <td>主机概要文件</td>
   <td>是</td>
   <td>是</td>
  </tr>
  <tr>
   <td>Storage DRS</td>
   <td>是</td>
   <td>否</td>
  </tr>
  <tr>
   <td>Storage I/O Control</td>
   <td>是</td>
   <td>否</td>
  </tr>
  <tr>
   <td>站点恢复管理器</td>
   <td>是</td>
   <td>否</td>
  </tr>
  <tr>
   <td>虚拟卷</td>
   <td>是</td>
   <td>否</td>
  </tr>
 </tbody>
</table>
*来源：[VMware - NFS 协议和 ESXi ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){:new_window}*



### 使用快照

通过 {{site.data.keyword.filestorage_short}}，管理员可以为每个存储卷设置快照安排，以自动创建和删除快照副本。管理员还可以创建额外的快照安排（每小时、每天和每周）来自动生成快照，也可以为业务连续性和灾难恢复 (BCDR) 方案手动创建特别快照。有关保留的快照和使用的空间的自动警报通过 [{{site.data.keyword.slportal}}![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){:new_window} 传递给卷所有者。

需要快照空间才能使用快照。可以在初始卷订购期间购买空间，也可以在初始供应后通过在**卷详细信息**页面上单击**操作**并选择**添加快照空间**来购买空间。

值得注意的是，VMware 环境并不知道快照。耐久性 {{site.data.keyword.filestorage_short}} 快照功能不能与 VMware 快照相混淆。使用 {{site.data.keyword.filestorage_short}} 快照功能的任何恢复都必须在 [{{site.data.keyword.slportal}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){:new_window} 中进行处理。

复原 {{site.data.keyword.filestorage_short}} 卷需要关闭 {{site.data.keyword.filestorage_short}} 上所有 VM 的电源。需要暂时从 ESXi 主机卸装该卷，以避免执行该过程期间发生任何数据损坏。

有关更多信息，请参阅[快照](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)文章。


### 使用复制

复制使用其中一个快照安排自动将快照复制到远程数据中心内的目标卷。如果发生灾难性事件或者发生数据损坏，那么可以在远程站点中恢复副本。

使用副本，您可以：

- 通过故障转移到目标卷，快速从站点故障和其他灾难进行恢复
- 故障转移到 DR 副本中的特定时间点

复制可以使两个不同位置的数据保持同步。如果要克隆卷并独立于原始卷来使用该卷，请参阅[创建复制文件卷](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)。
{:tip}

必须创建快照安排后，才能进行复制。

进行故障转移时，将“翻转开关”从主数据中心的存储卷切换到远程数据中心的目标卷。例如，主数据中心位于伦敦，辅助数据中心位于阿姆斯特丹。如果发生故障事件，将故障转移到阿姆斯特丹。故障转移意味着从阿姆斯特丹的 vSphere 集群实例连接到现在的主卷。修复伦敦的卷之后，会生成阿姆斯特丹卷的快照。然后，可以故障恢复到伦敦，并从伦敦的计算实例连接到原先的主卷。

在该卷故障恢复到主数据中心之前，需要停止远程站点上对该卷的使用。这将生成任何新信息或更改的信息的快照，并复制到主数据中心，然后才能将该卷重新安装到生产站点 ESXi 主机上。

有关配置副本的更多信息，请参阅[复制](/docs/infrastructure/FileStorage?topic=FileStorage-replication)。

对于无效数据（不管是损坏的数据、被黑客入侵的数据，还是被感染的数据），将根据快照安排和快照保留时间进行复制。使用最小的复制时段可提供更好的恢复点目标。不过，这还可能会缩短对无效数据的复制做出反应的时间。
{:note}


## 订购 {{site.data.keyword.filestorage_short}}

在 VMware 环境中，使用[高级单站点 VMware 参考体系结构 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window} 利用“耐久性”或“性能”选项来设置 {{site.data.keyword.filestorage_short}}。

订购 {{site.data.keyword.filestorage_short}} 的方式是通过 [IBM Cloud 目录 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/catalog/){:new_window} 或 [{{site.data.keyword.slportal}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){:new_window}。有关更多信息，请参阅[订购 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole)。

存储器将在不到一分钟的时间内进行供应，并且会在 [{{site.data.keyword.slportal}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){:new_window} 的 **{{site.data.keyword.filestorage_short}}** 页面上显示。

供应了卷后，必须授权将使用该卷的 {{site.data.keyword.BluBareMetServers_full}} 或 {{site.data.keyword.BluVirtServers_full}} 来访问该存储器。使用以下步骤来授权该卷。

1. 单击**存储** > **{{site.data.keyword.filestorage_short}}**。
2. 选择**耐久性**或**性能卷操作**菜单上的**访问主机**。
3. 单击**子网**。
4. 从分配给 ESXi 主机上 VMkernel 端口的可用子网的列表中进行选择，然后单击**提交**。<br/>

   显示的子网是与存储卷位于同一数据中心的预订子网。
   {:note}

在授权子网后，记下您希望在安装卷时使用的耐久性或性能存储服务器的主机名。可以通过在“{{site.data.keyword.filestorage_short}} 详细信息”页面上单击特定卷来找到此信息。


##  配置 VMware 虚拟机主机

在开始 VMware 配置过程之前，请确保满足以下先决条件：

- 为 VMware ESXi 的 {{site.data.keyword.BluBareMetServers}} 供应了适当的存储器配置和 ESXi 登录凭证。
- {{site.data.keyword.BluSoftlayer_full}} Windows 物理计算机或 {{site.data.keyword.virtualmachinesshort}} 与 {{site.data.keyword.BluBareMetServers}} 位于同一数据中心。包含 {{site.data.keyword.BluSoftlayer_full}} Windows VM 的公共 IP 地址和登录凭证。
- 有一台计算机具有因特网访问权，并且安装了 Web 浏览器软件和远程桌面协议 (RDP) 客户机。


### 1. 配置 VMware 主机。

1. 在连接到因特网的计算机中，启动 RDP 客户机，并与安装了 vSphere vCenter 的数据中心内所供应的 {{site.data.keyword.BluVirtServers_full}} 建立 RDP 会话。
2. 在 {{site.data.keyword.BluVirtServers_short}} 中，启动 Web 浏览器并通过 vSphere Web Client 连接到 VMware vCenter。
3. 在**主页**屏幕中，选择**主机和集群**。展开左侧的窗格，然后选择要用于此部署的 **VMware ESXi 服务器**。
4. 确保所有主机上都已打开用于 NFS 客户机的防火墙端口，以便可以在 vSphere 主机上配置 NFS 客户机。（在更新的 vSphere 发行版中，该端口会自动打开。）要检查该端口是否已打开，请转至 VMware® vCenter™ 中的 **ESXi 主机管理**选项卡，选择**设置**，然后选择**安全概要文件**。在**防火墙**部分中，单击**编辑**，然后向下滚动到 **NFS 客户机**。
5. 确保选中**允许来自任何 IP 地址或 IP 地址列表的连接**。<br/>
   ![允许连接](/images/1_4.png)
6. 通过转至 **ESXi 主机管理**选项卡，选择**管理**，然后选择**网络**来配置巨型帧。
7. 选择 **VMkernel 适配器**，突出显示 **vSwitch**，然后单击**编辑**（“画笔”图标）。
8. 选择 **NIC 设置**，并确保 NIC MTU 设置为 9000。
9. **可选**。验证巨型帧设置。
   - Windows
     ```
     ping -f -l 8972 a.b.c.d
     ```
     {:pre}

   - UNIX
     ```
     ping -s 8972 a.b.c.d
     ```
     {:pre}

     值 a.b.c.d 是相邻的 {{site.data.keyword.BluVirtServers_short}} 界面。

     示例
     ```
        ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
      8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
   ```

有关 VMware 和巨型帧的更多信息，请参阅[此处 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://kb.vmware.com/s/article/1003712){:new_window}。
{:tip}


### 2. 将上行链路适配器添加到虚拟交换机

1. 通过转至 **ESXi 主机管理**选项卡，选择**管理**，然后选择**网络**来配置新的上行链路适配器。
2. 选择**物理适配器**选项卡。
3. 单击**添加主机网络**（带加号的“地球”图标）。
4. 选择**物理网络适配器**作为连接类型，然后单击**下一步**。
5. 选择现有 **vSwitch**，然后单击**下一步**。
6. 选择**未使用的适配器**，然后单击**添加适配器**（加号）。
7. 单击其他“已连接”适配器，然后单击**确定**。<br/>
   ![向交换机添加物理适配器](/images/2_3.png)
8. 单击**下一步**，再单击**完成**。
9. 转回**虚拟交换机**选项卡，然后选择**虚拟交换机**标题下的**编辑设置**（“画笔”图标）。
10. 选择左侧的 vSwitch **成组和故障转移**条目。

11. 验证**负载均衡**选项是否设置为**基于源虚拟端口进行路由**，然后单击**确定**。


### 3. 配置静态路由（可选）

此体系结构指南的网络配置使用最少数量的端口组。如果您具有用于 NFS 存储器的 VMkernel 端口组，那么必须执行额外的步骤。缺省情况下，ESXi 将使用 NFS 卷所在子网上的 VMkernel 端口安装 NFS 卷。由于是将第 3 层路由用于安装 NFS 卷，因此必须强制 ESXi 使用配置为安装 NFS 卷的 VMkernel 端口。为了使用正确的端口，必须创建指向存储阵列的静态路由。

1. 要配置静态路由，请通过 SSH 登录到使用性能或耐久性存储器的每台 ESXi 主机，然后运行以下命令。记下 `ping` 命令（第一个命令）中生成的 IP 地址，并将其用于 `esxcli` network 命令。
   ```
   ping <host name of the storage array>
   ```
   {: pre}

   NFS 存储器 DNS 主机名是分配有多个 IP 地址的转发区域 (FZ)。这些 IP 地址是静态的，并且属于该特定 DNS 主机名。可以使用其中任一 IP 地址来访问特定卷。
{:note}

   ```
   esxcli network ip route ipv4 add –gateway GATEWAYIP –network <result of ping command>/32
   ```
   {: pre}

2. 在 ESXi 5.0 和更低版本上，静态路由在重新启动后不会持久存储。要确保添加的任何静态路由都是持久存储的，需要将此命令添加到每个主机上 `/etc/rc.local.d/` 目录下的 `local.sh` 文件中。使用可视编辑器来打开 `local.sh` 文件，然后将步骤 4.1 中的第二个命令添加到 `exit 0` 行前面。

记下 IP 地址，因为此地址可用于在下一步中安装卷。<br/>对于计划安装到 ESXi 主机的每个 NFS 卷，都需要完成此过程。<br/>有关更多信息，请参阅 VMware 知识库文章：[Configuring static routes for VMkernel ports on an ESXi host ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://kb.vmware.com/s/article/2001426){:new_window}。
{:tip}


##  在 ESXi 主机上创建和安装 {{site.data.keyword.filestorage_short}} 卷

1. 单击**转至 vCenter** 图标，然后单击**主机和集群**。
2. 在**相关对象**选项卡上，单击**数据存储**。
3. 单击**新建数据存储**图标。
4. 在**新建数据存储**屏幕上，选择 VMware 数据存储的位置，然后单击**下一步**。
5. 在**类型**屏幕上，选择 **NFS**，然后单击**下一步**。
6. 然后，选择 NFS 版本。NFS V3 和 NFS V4.1 均受支持，但首选 NFS V3。

   确保您仅使用一个 NFS 版本来访问数据存储。使用不同版本在同一数据存储上安装一个或多个主机可能会导致数据损坏。
   {:important}

7. 在**名称和配置**屏幕上，输入要对 VMware 数据存储命名的名称。此外，还请输入 NFS 服务器的主机名。使用 NFS 服务器的 FQDN 将生成至底层服务器的最佳流量分配。IP 地址也有效，但使用频率较低，并且仅在特定情况下使用。请以 `/foldername` 格式输入文件夹名称。
8. 在**主机可访问性**屏幕上，选择要安装 NFS VMware 数据存储的一个或多个主机，然后单击**下一步**。
9. 在下一个屏幕上复查输入内容，然后单击**完成**。
10. 对其他任何 {{site.data.keyword.filestorage_short}} 卷重复此操作。

{{site.data.keyword.BluSoftlayer_full}} 建议使用 FQDN 名称来连接到 VMware 数据存储。使用直接 IP 寻址可能会绕过使用 FQDN 提供的负载均衡机制。
{:important}

要使用 IP 地址而不使用 FQDN，只需对服务器执行 ping 操作即可获取 IP 地址。
```
ping <host name of the storage array>
```
{: pre}

要从 ESXi 主机获取 IP 地址，请使用以下命令。生成的 10.2.125.80 是使用 FQDN 的 IP。
```
~ # vmkping nfsdal0902a-fz.service.softlayer.com
PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
```


## 启用 ESXi Storage I/O Control（可选）

Storage I/O Control (SIOC) 是可用于使用 Enterprise Plus 许可证的客户的功能。在环境中启用 SIOC 后，此功能会更改单个 VM 的设备队列长度。对设备队列长度的更改会使所有 VM 的存储阵列队列减少到相同的份额。仅当资源受到约束且存储器 I/O 等待时间超过定义的阈值时，SIOC 才会起作用。


为了使 SIOC 能够确定存储设备何时拥堵或受到约束，SIOC 需要有定义的阈值。对于不同的存储类型，拥堵阈值等待时间各不相同。缺省选择是达到峰值吞吐量的 90%。峰值吞吐量百分比值指示 VMware 数据存储在使用该百分比的估算峰值吞吐量时，估计的等待时间阈值。


为 VMware 数据存储或 VMDK 错误地配置 SIOC 可能会显著影响性能。
{:important}


### 配置用于 VMware 数据存储的 Storage I/O Control

1. 在 vSphere Web Client 导航器中，浏览至 VMware 数据存储。
2. 单击**管理**选项卡。
3. 单击**设置**，然后单击**常规**。
4. 对于**数据存储功能**，单击**编辑**。
5. 选中**启用 Storage I/O Control** 复选框。<br/>
   ![NSF VMware 数据存储](/images/3_0.png)
6. 单击**确定**。

此设置特定于 VMware 数据存储，而不是特定于主机。
{:note}


### 配置用于 {{site.data.keyword.BluVirtServers_short}} 的 Storage I/O Control

您还可以限制各个 VM 的单个虚拟盘，或者向其授予针对 SIOC 的不同份额。通过限制磁盘和授予不同份额，可以将环境与具有所获取 {{site.data.keyword.filestorage_short}} 卷 IOPS 数的工作负载相匹配。此限制由 IOPS 设置，并且可以设置其他权重或“份额”。份额设置为“高”（2,000 个份额）的虚拟盘收到的 I/O 是设置为“正常”（1,000 个份额）的磁盘的 2 倍，是设置为“低”（500 个份额）的磁盘的 4 倍。“正常”是所有 VM 的缺省值，因此您需要针对需调整的 VM 将值调整为高于或低于“正常”。

使用以下步骤来更改 VDisk 份额和限制。

1. 在 **VM 和模板**清单中，选择 {{site.data.keyword.BluVirtServers_short}}。
2. 选择要进行 I/O 控制的 {{site.data.keyword.BluVirtServers_short}}。
3. 单击**管理**选项卡，然后单击**设置**。单击**编辑**。
4. 展开**硬盘**箭头。根据您的环境，相应选择**修改份额**或**限制 - IOPS**。从列表中选择虚拟硬盘，然后修改“份额”选项，以选择要分配给 {{site.data.keyword.BluVirtServers_short}} 的相对份额数（“低”、“正常”或“高”）。还可以单击**定制**并输入用户定义的份额值。
5. 单击**限制 - IOPS** 列，然后输入要分配给虚拟机的最大存储资源。
6. 单击**确定**。


此过程用于在 {{site.data.keyword.BluVirtServers_short}} 中设置各个 vDisk 的资源消耗限制，即便未启用 SIOC，此过程也适用。尽管 SIOC 会使用这些设置，但这些设置特定于单个访客，而不是特定于主机。
{:important}


## 配置 ESXi 主机端设置

配置 ESXi 5.x 主机以使用 NFS 存储器需要一些额外设置。此表显示了每个设置需要的内容。

|参数|设置为...|
|----------|------------|
|Net.TcpipHeapSize|	32|
|Net.TcpipHeapMax|	对于 vSphere 5.0/5.1，设置为 128<br/> 对于 vSphere 5.5 或更高版本，设置为 512|
|NFS.MaxVolumes|	256|
|NFS41.MaxVolumes|	256（仅限 vSphere 6.0 或更高版本）|
|NFS.HeartbeatMaxFailures|	10|
|NFS.HeartbeatFrequency|	12|
|NFS.HeartbeatTimeout|	5|
|NFS.MaxQueueDepth|	64|


### 在 ESXi 5.x 主机上使用 CLI 更新高级配置参数

以下示例使用 CLI 来设置高级配置参数，然后对其进行检查。示例中使用的 `esxcfg-advcfg` 工具位于 ESXi 5.x 主机上的 `/usr/sbin` 目录中。

   - 在 ESXi 5.x CLI 中设置高级配置参数。

     ```
     #esxcfg-advcfg -s 32 /Net/TcpipHeapSize
     #esxcfg-advcfg -s 128 /Net/TcpipHeapMax(For vSphere 5.0/5.1)
     #esxcfg-advcfg -s 512 /Net/TcpipHeapMax(For vSphere 5.5 and higher)
     #esxcfg-advcfg -s 256 /NFS/MaxVolumes
     #esxcfg-advcfg -s 256 /NFS41/MaxVolumes (ESXi 6.0 and higher)
     #esxcfg-advcfg -s 10 /NFS/HeartbeatMaxFailures
     #esxcfg-advcfg -s 12 /NFS/HeartbeatFrequency
     #esxcfg-advcfg -s 5 /NFS/HeartbeatTimeout   
     #esxcfg-advcfg -s 64 /NFS/MaxQueueDepth
     #esxcfg-advcfg -s 32 /Disk/QFullSampleSize
     #esxcfg-advcfg -s 8 /Disk/QFullThreshold
     ```

  - 在 ESXi 5.x CLI 中检查高级配置参数。

    ```
    #esxcfg-advcfg -g /Net/TcpipHeapSize
    #esxcfg-advcfg -g /Net/TcpipHeapMax
    #esxcfg-advcfg -g /NFS/MaxVolumes
    #esxcfg-advcfg -g /NFS41/MaxVolumes (ESXi 6.0 and higher)
    #esxcfg-advcfg -g /NFS/HeartbeatMaxFailures
    #esxcfg-advcfg -g /NFS/HeartbeatFrequency
    #esxcfg-advcfg -g /NFS/HeartbeatTimeout
    #esxcfg-advcfg -g /NFS/MaxQueueDepth
    #esxcfg-advcfg -g /Disk/QFullSampleSize
    #esxcfg-advcfg -g /Disk/QFullThreshold
    ```
要了解有关高级单站点 VMware 参考体系结构的更多信息，请访问[此处](/docs/infrastructure/virtualization?topic=Virtualization-advanced-single-site-vmware-reference-architecture){:new_window}。
{:tip}
