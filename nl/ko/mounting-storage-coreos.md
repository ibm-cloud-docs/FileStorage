---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# CoreOS에서 {{site.data.keyword.filestorage_short}} 마운트

CoreOS는 다양한 인프라에서 확장 가능한 대형 배치를 쉽게 관리하도록 빌드된 강력한 Linux 배포입니다. Chrome OS 빌드에 기반하는 CoreOS는 경량 호스트 시스템을 유지보수하고 모든 애플리케이션에 대해 Docker 컨테이너를 사용합니다. 

## 휴대용 스토리지 마운트

시스템 레벨 마운트는 CoreOS에서 읽기 전용인 디렉토리에 있으므로 모든 보조 마운트 파일은 */etc/systemd/system* 디렉토리로 이동합니다. MOUNTPOINT.mount 파일을 작성합니다. .mount 파일의 Where 섹션은 파일 이름과 일치해야 합니다. 마운트 지점이 직접 /로 시작하지 않는 경우 다음 구문을 사용하여 파일 이름을 지정해야 합니다. path-to-mount.mount. 다음 예제에서 보는 대로, `/mnt/www`에 휴대용 스토리지 드라이브를 마운트하려고 하므로, 파일 이름을 `mnt-www.mount`로 지정합니다. 

파티션을 작성하기 위해 fdisk 또는 parted를 사용하고 작성한 파일 시스템이 `.mount` 파일에 나열된 항목과 일치하는지 확인해야 합니다. 그렇지 않으면 서비스가 시작되지 않습니다. 


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

CoreOS는 systemd를 사용하므로 마운트 지점을 유지하려면 재부팅 시 `*.mount` 파일을 사용해야 합니다. `--now` 플래그를 사용하는 경우 파티션은 즉시 마운트되며 부팅 시 시작하도록 설정됩니다. 

`$ systemctl enable --now mnt-www.mount`

## NFS/{{site.data.keyword.filestorage_short}} 마운트

Endurance/Performance {{site.data.keyword.filestorage_short}} 마운트 프로세스도 거의 비슷하지만, 마운트가 NFS이므로 mount 파일에서 Options= 행을 사용하여 몇 가지 추가 옵션을 지정할 수 있습니다. 다음 예제에서는 `/data/www`에서 마운트할 NFS를 설정합니다. {{site.data.keyword.filestorage_short}} 인스턴스의 NFS 마운트 포인트는 {{site.data.keyword.filestorage_short}} 목록 페이지에서 또는 API -SoftLayer_Network_Storage::getNetworkMountAddress() 호출을 통해 확보할 수 있습니다. 

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

이제 마운트를 사용하고 올바르게 마운트되었는지 확인할 수 있습니다. 

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
 
## NAS/Cifs 마운트

cifs 공유 마운트는 CoreOS에서 기본적으로 지원되지 않지만, 호스트 시스템이 NFS 공유를 마운트할 수 있도록 하는 쉬운 임시 해결책이 있습니다. 컨테이너를 사용하여 mount.cfis 모듈을 빌드하고 CoreOS 시스템에 복사합니다. 
 
CoreOS 시스템에서 다음을 실행하여 Fedora 컨테이너를 다운로드하고 여기에 넣습니다. <br/>
`docker run -t -i -v /tmp:/host_tmp fedora /bin/bash`
 
컨테이너에 있으면 다음을 실행하여 cifs 유틸리티를 빌드하려고 합니다. 
```
dnf groupinstall -y "Development Tools" "Development Libraries"
dnf install -y tar
dnf install -y bzip2
curl https://ftp.samba.org/pub/linux-cifs/cifs-utils/cifs-utils-6.4.tar.bz2 | bunzip2 -c - | tar -xvf -
cd cifs-utils-6.4/
./configure && make
cp mount.cifs /host_tmp/
```
 
mount.cifs 파일이 호스트 시스템에 복사되었으므로 `exit` 명령을 실행하거나 **ctrl+d**를 눌러 Docker 컨테이너를 종료할 수 있습니다. CoreOS 시스템으로 돌아가면 다음 명령으로 CIFS 공유를 마운트할 수 있습니다. <br/>
`/tmp/mount.cifs //nasXXX.service.softlayer.com/USERNAME -o username=USERNAME,password=PASSWORD /path/to/mount`
 
## iSCSI 마운트

현재 CoreOS에서 지원되지 않지만 향후 릴리스에서 준비 중입니다. https://github.com/coreos/bugs/issues/634
