---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

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


# Windows 및 Linux의 {{site.data.keyword.BluSoftlayer_notm}}에서의 Jumbo 프레임 사용
{: #jumboframes}

Jumbo 프레임은 1,500바이트의 표준 MTU(Maximum Transmission Unit)보다 큰 페이로드의 이더넷 프레임입니다. Jumbo 프레임은 최소한 1Gbps를 지원하는 근거리 통신망에서 사용되며 최대 9,000바이트 크기까지 가능합니다.

Jumbo 프레임은 소스 디바이스 <-> 스위치 <-> 라우터 <-> 스위치 <-> 대상 디바이스에서 전체 네트워크 경로에 대해 동일하게 구성되어야 합니다. 전체 체인이 동일하게 설정되지 않은 경우에는 체인에서 가장 낮은 설정으로 기본 설정됩니다. {{site.data.keyword.cloud}}의 네트워크 디바이스는 현재 9,000으로 설정되어 있습니다. 모든 고객 디바이스는 동일한 값인 9,000으로 설정되어야 합니다.
{:important}

## Windows에서의 Jumbo 프레임 사용

1. **네트워크 및 공유 센터**를 여십시오.
2. **어댑터 설정 변경**을 클릭하십시오.
3. Jumbo 프레임을 사용할 NIC에 마우스 오른쪽 단추를 클릭하고 **특성**을 선택하십시오.
4. **네트워킹** 탭 아래에서 네트워크 어댑터에 대한 **구성**을 클릭하십시오.
5. **고급** 탭을 선택하십시오.
6. **Jumbo 프레임**을 선택하고 **사용 안함** 값을 원하는 값으로 변경하십시오. 값(9kB 또는 9014바이트 등)은 NIC에 따라 달라집니다.
7. 모든 창에서 **확인**을 클릭하십시오.

변경을 수행할 때 잠시 동안 NIC의 네트워크 연결이 끊어집니다. 변경사항이 적용되었는지 확인하려면 디바이스를 다시 시작하십시오.
{:tip}


## Linux에서의 Jumbo 프레임 사용

1. eth0 인터페이스의 네트워크 구성 파일을 편집하십시오.
   - CentOS, RHEL, Fedora Linux 사용자는 `/etc/sysconfig/network-script/ifcfg-eth0`을 편집합니다.
     ```
     # vi /etc/sysconfig/network-script/ifcfg-eth0
     ```
     {: pre}

   - Debian 및 Ubuntu Linux 사용자는 `/etc/network/interfaces`를 편집합니다.

2. 프레임 크기(바이트)를 지정하는 다음 구성 지시문을 추가하십시오.
   - CentOS, RHEL, Fedora Linux
     ```
     MTU 9000
     ```
     {: pre}

   - Debian 및 Ubuntu Linux
     ```
     MTU=9000
     ```
     {: pre}

3. 파일을 닫고 저장하십시오. Interface eth0을 다시 시작하십시오.
   ```
   # /etc/init.d/networking restart
   ```
   {: pre}

   이 조치를 수행하면 네트워크 연결이 잠시 끊어집니다.
