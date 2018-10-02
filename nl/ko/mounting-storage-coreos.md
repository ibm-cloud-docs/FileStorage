---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}

# CoreOS에 {{site.data.keyword.filestorage_short}} 마운트

CoreOS는 다양한 인프라의 대규모 확장 가능한 배치를 쉽게 관리할 수 있게 빌드된 강력한 Linux 배포입니다. Chrome OS의 빌드를 기반으로 하는 CoreOS는 경량 호스트 시스템을 유지보수하며 모든 애플리케이션에 대해 Docker 컨테이너를 사용합니다.

## 휴대용 스토리지 마운트

시스템 레벨 마운트가 CoreOS의 읽기 전용 디렉토리에 있으므로, 모든 2차 마운트 파일은 `/etc/systemd/system` 디렉토리로 이동됩니다. 먼저 `MOUNTPOINT.mount` 파일을 작성해야 합니다. `.mount` 파일의 **Where** 섹션은 파일 이름과 일치해야 합니다. 마운트 지점이 `/` 바로 아래가 아닌 경우에는 `path-to-mount.mount` 구문을 사용하여 파일 이름을 지정해야 합니다. 예를 들어, `/mnt/www`에 휴대용 스토리지 드라이브를 마운트하려는 경우에는 파일 이름을 `mnt-www.mount`로 지정하십시오.

`fdisk` 또는 `parted`를 사용하여 파티션을 작성할 수 있으며, 작성하는 파일 시스템은 `.mount` 파일에 나열된 것과 일치하도록 해야 합니다. 그렇지 않은 경우에는 서비스가 시작되지 않습니다.


```
[Unit]
Description = Mount for Portable Storage

[Mount]
What=/dev/xvdc1
Where=/mnt/www
Type=ext4

[Install]
WantedBy = multi-user.target
```
{:codeblock}


CoreOS는 `systemd`를 사용하므로, 마운트 지점이 다시 시작 후에도 지속되도록 하려면 `*.mount` 파일을 사용으로 설정해야 합니다. `--now` 플래그를 사용하면 파티션이 즉시 마운트되며 부팅 시 시작하도록 설정됩니다.

```
$ systemctl enable --now mnt-www.mount
```
{:pre}

## NFS/{{site.data.keyword.filestorage_short}} 마운트

{{site.data.keyword.filestorage_short}} 마운트 프로세스 또한 동일합니다. 마운트가 NFS이므로 사용자는 마운트 파일에 `Options=` 행을 사용하여 추가 옵션을 지정할 수 있습니다. 

예에서 NFS는 `/data/www`에 마운트되도록 설정되어 있습니다. {{site.data.keyword.filestorage_short}} 인스턴스의 NFS 마운트 지점은 {{site.data.keyword.filestorage_short}} 목록 페이지에서, 또는 API 호출 `SoftLayer_Network_Storage::getNetworkMountAddress()`를 통해 얻을 수 있습니다.

```
$ cat data-www.mount
[Unit]
Description = Mount for Container Storage

[Mount]
What=<nfs_mount_point>
Where=/data/www
Type=nfs
Options=vers=4,sec=sys,noauto

[Install]
WantedBy = multi-user.target
```
{:codeblock}

그 후 마운트를 사용으로 설정하고 올바르게 마운트되었는지 확인하십시오.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}
 
## NAS/CIFS 마운트

CIFS 공유의 마운트는 기본적으로 CoreOS에서 지원되지 않지만, 호스트 시스템이 NAS 공유를 마운트할 수 있도록 하는 쉬운 임시 해결책이 있습니다. 컨테이너를 사용하여 `mount.cfis` 모듈을 빌드한 후 이를 CoreOS 시스템에 복사할 수 있습니다.
 
CoreOS 시스템에서 다음을 실행하여 Fedora 컨테이너를 다운로드하고 내부로 진입하십시오.

```
docker run -t -i -v /tmp:/host_tmp fedora /bin/bash
```
{:pre}
 
컨테이너 내부에서 다음을 실행하여 CIFS 유틸리티를 빌드하십시오.

```
dnf groupinstall -y "Development Tools" "Development Libraries"
dnf install -y tar
dnf install -y bzip2
curl https://ftp.samba.org/pub/linux-cifs/cifs-utils/cifs-utils-6.4.tar.bz2 | bunzip2 -c - | tar -xvf -
cd cifs-utils-6.4/
./configure && make
cp mount.cifs /host_tmp/
```
{:codeblock}
 
`mount.cifs` 파일이 호스트에 복사되었으므로 `exit` 명령을 입력하거나 **ctrl+d**를 눌러 Docker 컨테이너를 종료할 수 있습니다. CoreOS 시스템으로 다시 돌아오면 다음 명령으로 CIFS 공유를 마운트할 수 있습니다. 
```
/tmp/mount.cifs //nasXXX.service.softlayer.com/USERNAME -o username=USERNAME,password=PASSWORD /path/to/mount
```
{:pre}
 
## ISCSI 마운트

현재 이는 CoreOS에서 지원되지 않지만 향후 릴리스에서는 지원될 예정입니다(https://github.com/coreos/bugs/issues/634).
