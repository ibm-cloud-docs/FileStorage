---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Linux에서 {{site.data.keyword.filestorage_short}} 마운트

먼저 {{site.data.keyword.filestorage_full}} 볼륨에 액세스할 호스트의 권한이 [{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}을 통해 부여되는지 확인하십시오.

1. {{site.data.keyword.filestorage_short}} 목록 페이지에서 새 공유와 연관된 **조치** 링크를 클릭하고 **호스트 권한 부여**를 클릭하십시오.
2. 목록에서 호스트를 선택하고 **제출**을 클릭하십시오. 이 조치는 공유에 액세스할 수 있도록 호스트에 권한을 부여합니다.

## {{site.data.keyword.filestorage_short}} 공유 마운트

Linux 기반 {{site.data.keyword.BluSoftlayer_full}} 컴퓨팅 인스턴스를 NFS(Network File System) 공유에 연결하려면 다음 지시사항을 사용하십시오. 이 예제는 Red Hat Enterprise Linux 6를 기반으로 합니다. 운영 체제(OS) 공급업체 문서에 따라 기타 Linux 배포의 경우에는 단계가 조정될 수 있습니다.

파일 스토리지 인스턴스의 마운트 지점은 {{site.data.keyword.filestorage_short}} 목록 페이지에서 또는 API호출 - `SoftLayer_Network_Storage::getNetworkMountAddress()`를 통해 가져올 수 있습니다.
{:tip}

1. 필수 도구를 설치하십시오.
   ```
   # yum -y install nfs-utils nfs-utils-lib
   ```
   {:pre}

2. 원격 공유를 마운트하십시오.
   ```
   # mount -t "nfs version" -o "options" <mount_point> /mnt
   ```

   예
   ```
   # mount -t nfs4 -o hard,intr
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt
   ```

3. 마운트에 성공했는지 확인하십시오.
   ```
   # df -h
   Filesystem Size Used Avail Use% Mounted on
   /dev/xvda2 25G 1.4G 22G 6% /
   tmpfs 1.9G 0 1.9G 0% /dev/shm
   /dev/xvda1 97M 51M 42M 55%
   ```

4. 마운트 지점으로 이동하여 파일 읽기/쓰기를 수행하십시오.
   ```
   # touch /mnt/test
   # ls -la /mnt
   total 12
   drwxr-xr-x 2 nobody nobody 4096 Sep 8 15:52 .
   dr-xr-xr-x. 22 root root 4096 Sep 8 14:30 ..
   -rw-r--r-- 1 nobody nobody 0 Sep 8 15:52 test
   ```

   루트가 작성한 파일은 `nobody:nobody`의 소유권을 갖습니다. 소유권을 올바르게 표시하려면 `idmapd.conf`를 올바른 도메인 설정으로 업데이트해야 합니다. [NFS에 대한 no_root_squash 구현 방법](#implementing-no_root_squash-for-nfs-optional-) 섹션을 참조하십시오.
   {:tip}

5. 시작 시 원격 공유를 마운트하십시오. 설정을 완료하려면 파일 시스템 테이블(`/etc/fstab`)을 편집하여 스타트업 시에 자동으로 마운트되는 항목의 목록에 원격 공유를 추가하십시오.

   ```
   (hostname):/(username) /mnt "nfs version" "options" 0 0
   ```

   예

   ```
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs4 defaults,hard,intr 0 0
   ```

6. 구성 파일에 오류가 없는지 확인하십시오.

   ```
   # mount -fav
   ```
   {:pre}

   명령이 오류 없이 완료되면 설정이 완료됩니다.

   NFS 4.1을 사용 중인 경우에는 마운트 명령에 `sec=sys`를 추가하여 파일 소유권 문제를 방지하십시오.
   {:tip}


## NFS에 대한 `no_root_squash` 구현(선택사항)

`no_root_squash`를 구성하면 루트 클라이언트가 NFS 공유에 대해 루트 권한을 유지합니다.
- NFSv3의 경우에는 클라이언트가 작업을 수행하지 않아도 `no_root_squash`가 작동합니다.
- NFSv4의 경우에는 nfsv4 도메인을 `slnfsv4.com`으로 설정하고 `rpcidmapd`, 또는 자신의 OS에서 사용하는 이와 유사한 서비스를 시작해야 합니다.

예

1. 호스트를 통해 `/etc/idmapd.conf`에서 도메인 설정을 설정하십시오.

   ```
   #vi /etc/idmapd.conf
   [General]
   #Verbosity = 0
   #The following should be set to the local NFSv4 domain name
   #The default is the host's DNS domain name.
   Domain = slnfsv4.com
   [Mapping]
   Nobody-User = nobody
   Nobody-Group = nobody
   ```

2. `nfsidmap -c`를 실행하십시오.
3. `rpcidmapd`를 시작하십시오.
   ```
   # /etc/init.d/rpcidmapd start
   Starting RPC idmapd: [ OK ]
   ```
