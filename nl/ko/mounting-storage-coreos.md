---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-08"

keywords: File Storage, file storage, NFS, mounting volume in Container Linux, CoreOS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}


# Container Linux에 {{site.data.keyword.filestorage_short}} 마운트
{: #mountingCoreOS}

Container Linux by CoreOS는 Linux 커널을 기반으로 하는 오픈 소스, 경량 운영 체제입니다. 인프라를 클러스터된 배치에 제공하는 데 사용하도록 디자인되었습니다. Container Linux는 운영 체제로서, 서비스 발견 및 구성 공유를 위한 기본 제공 메커니즘과 함께 소프트웨어 컨테이너 내부에 애플리케이션을 배치하는 데 필요한 최소한의 기능을 제공합니다. 자세한 정보는 [스토리지 마운트](https://coreos.com/os/docs/latest/mounting-storage.html)를 참조하십시오.

시스템 레벨 마운트가 읽기 전용 디렉토리에 있으므로, 모든 2차 마운트 파일은 `/etc/systemd/system` 디렉토리로 이동됩니다. 먼저 `MOUNTPOINT.mount` 파일을 작성해야 합니다. `.mount` 파일의 **Where** 섹션은 파일 이름과 일치해야 합니다. 마운트 지점이 `/` 바로 아래가 아닌 경우에는 `path-to-mount.mount` 구문을 사용하여 파일 이름을 지정해야 합니다. 예를 들어, `/mnt/www`에 휴대용 스토리지 드라이브를 마운트하려는 경우에는 파일 이름을 `mnt-www.mount`로 지정하십시오.

`fdisk` 또는 `parted`를 사용하여 파티션을 작성할 수 있습니다. 작성하는 파일 시스템이 `.mount` 파일에 나열된 것과 일치하는지 확인하십시오. 그렇지 않으면, 서비스가 시작되지 않습니다. 마운트가 NFS이므로 사용자는 마운트 파일에 `Options=` 행을 사용하여 추가 옵션을 지정할 수 있습니다.

다음 예에서는 NFS가 `/data/www`에 마운트되도록 설정되어 있습니다. {{site.data.keyword.filestorage_short}} 인스턴스의 NFS 마운트 지점은 {{site.data.keyword.filestorage_short}} 목록 페이지에서, 또는 API 호출 `SoftLayer_Network_Storage::getNetworkMountAddress()`를 통해 얻을 수 있습니다.
{:tip}

```
$ cat data-www.mount
[Unit]
Description = Mount for Container Storage

[Mount]
What=<nfs_mount_point>
Where=/data/www
Type=nfs
Options=vers=3,sec=sys,noauto

[Install]
WantedBy = multi-user.target
```
{:codeblock}

그 후 마운트를 사용으로 설정하고 올바르게 마운트되었는지 확인하십시오.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs3 (rw,relatime,vers=3.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}

자세한 정보는 [`systemd mount` 문서](https://www.freedesktop.org/software/systemd/man/systemd.mount.html)를 참조하십시오.
