---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Linux에서 {{site.data.keyword.filestorage_short}}에 액세스

시작하기 전에 {{site.data.keyword.filestorage_full}} 볼륨에 액세스하는 호스트가 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}을 통해 권한이 부여되었는지 확인하십시오. 

1. {{site.data.keyword.filestorage_short}} 목록 페이지에서 새로 프로비저닝된 공유와 연관된 **조치**를 클릭하고 **호스트 권한 부여**를 클릭하십시오. 
2. 목록에서 원하는 호스트를 선택하고 **제출**을 클릭하십시오. 그러면 공유에 액세스할 권한을 호스트에 부여합니다. 

## {{site.data.keyword.filestorage_short}} 공유 마운트

다음은 Linux 기반 {{site.data.keyword.BluSoftlayer_full}} 컴퓨팅 인스턴스를 네트워크 파일 시스템(NFS) 공유에 연결하는 데 필요한 단계입니다. 예제는 Red Hat Enterprise Linux 6를 기반으로 합니다. 단계는 운영 체제(OS) 벤더 문서에 따라 다른 Linux 배포에 맞게 조정 가능합니다. 

**참고:** 파일 스토리지 인스턴스의 마운트 지점은 {{site.data.keyword.filestorage_short}} 목록 페이지에서 또는 API - SoftLayer_Network_Storage::getNetworkMountAddress()에 대한 호출을 통해 확보할 수 있습니다. 

1. 필수 패키지/도구를 설치하십시오. 

    `[root@TEST-RHEL6]# yum -y install nfs-utils nfs-utils-lib
    `
2. 원격 공유를 마운트하십시오.
    `[root@TEST-RHEL6]# mount -t "nfs version" -o "options" <mount_point> /mnt`
    
    다음은 스토리지 인스턴스에 원격 공유를 마운트하는 예제입니다. 
    
    `[root@TEST-RHEL6 mnt]# mount -t nfs4 -o hard,intr`
    
    `nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt`
 
3. 마운트에 성공했는지 확인하십시오. 

    `[root@TEST-RHEL6]# df -h`
    
    `Filesystem Size Used Avail Use% Mounted on`
    
    `/dev/xvda2 25G 1.4G 22G 6% /`
    
    `tmpfs 1.9G 0 1.9G 0% /dev/shm`
    
    `/dev/xvda1 97M 51M 42M 55%`
    
4. 마운트 지점으로 이동하고 파일을 읽고 쓰십시오. 

    `[root@TEST-RHEL6]# touch /mnt/test`
    
    `[root@TEST-RHEL6]# ls -la /mnt`
    
    `total 12`
    
    `drwxr-xr-x 2 nobody nobody 4096 Sep 8 15:52 .`
    
    `dr-xr-xr-x. 22 root root 4096 Sep 8 14:30 ..`
    
    `-rw-r--r-- 1 nobody nobody 0 Sep 8 15:52 test`

    **참고:** 루트에서 작성된 파일은 nobody:nobody의 소유권이 적용됩니다. 소유권을 올바르게 표시하려면 올바른 도메인 설정으로 idmapd.conf를 업데이트해야 합니다. 아래 “NFS에 대해 no_root_squash를 구현하는 방법”을 참조하십시오. 
    
5. 부팅 시 원격 공유를 마운트하십시오. 설정을 완료하려면 파일 시스템 테이블 /etc/fstab을 편집하여 스타트업 시 자동으로 마운트되는 항목 목록에 원격 공유를 추가하십시오. 

    `(hostname):/(username) /mnt "nfs version" "options" 0 0`
    
    원격 공유 예제의 마운트에서 인스턴스를 사용하면 항목은 다음과 같습니다. 
    
    `nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs4 defaults,hard,intr 0 0`
    
6.  구성 파일에 오류가 없는지 확인하십시오. 

    `[root@TEST-RHEL6 /]# mount -fav`
    
    명령이 오류 없이 완료되는 경우, 설정이 완료됩니다. 

**참고:** NFS 4.1을 사용하는 경우 마운트 명령에 'sec=sys'를 추가하여 파일 소유권 문제를 방지하십시오. 

 
## NFS에 대해 no_root_squash를 구현하는 방법(선택사항)

no_root_squash를 구성하면 루트 클라이언트가 NFS 공유에서 루트 권한을 보유할 수 있습니다. NFSv3의 경우 클라이언트가 수행할 작업은 없으며 no_root_squash가 그대로 작동합니다.
NFSv4의 경우 OS 버전에 따라 nfsv4 도메인을 slnfsv4.com으로 설정하고 rpcidmapd 또는 유사한 서비스를 시작해야 합니다. 

예를 살펴보겠습니다. 

1. 호스트에서, /etc/idmapd.conf에 도메인 설정을 설정하십시오. 

    `#vi /etc/idmapd.conf`
    
    `[General]`
    
    `#Verbosity = 0`
    
    `#The following should be set to the local NFSv4 domain name`
    
    `#The default is the host's DNS domain name.`
    
    `Domain = slnfsv4.com`
    
    `[Mapping]`
    
    `Nobody-User = nobody`
    
    `Nobody-Group = nobody`
    
2. nfsidmap -c를 실행하십시오. 
3. rpcidmapd를 시작하십시오. 

   ` # /etc/init.d/rpcidmapd start`
   
   ` Starting RPC idmapd: [ OK ]`
