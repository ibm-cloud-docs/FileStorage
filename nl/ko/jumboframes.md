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


# {{site.data.keyword.BluSoftlayer_notm}}에서의 Jumbo 프레임 사용
{: #jumboframes}

Jumbo 프레임은 1,500바이트의 표준 MTU(Maximum Transmission Unit)보다 큰 페이로드의 이더넷 프레임입니다. Jumbo 프레임은 최소한 1Gbps를 지원하는 근거리 통신망에서 사용되며 최대 9,000바이트 크기까지 가능합니다.

Jumbo 프레임은 소스 디바이스 <-> 스위치 <-> 라우터 <-> 스위치 <-> 대상 디바이스에서 전체 네트워크 경로에 대해 동일하게 구성되어야 합니다. 전체 체인이 동일하게 설정되지 않은 경우에는 체인에서 가장 낮은 설정으로 기본 설정됩니다. {{site.data.keyword.cloud}}의 네트워크 디바이스는 현재 9,000으로 설정되어 있습니다. 모든 고객 디바이스는 동일한 값인 9,000으로 설정되어야 합니다.
{:important}



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
