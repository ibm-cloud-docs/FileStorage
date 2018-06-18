---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---
{:new_window: target="_blank"}
{:pre: .pre}
 
# Plesk로 백업하기 위한 {{site.data.keyword.filestorage_short}} 구성

이 기사에서는 Plesk에서 백업을 위해 {{site.data.keyword.blockstoragefull}}를 구성하는 지시사항을 제공하고자 합니다. 여기서는 루트 또는 sudo SSH 및 전체 admin 레벨 Plesk 액세스가 사용 가능하다고 가정합니다. 이 예제는 CentOS7 호스트를 기반으로 합니다.

**참고**: 백업 및 복원을 위한 Plesk 문서는 [여기서](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window} 찾을 수 있습니다.

1. SSH를 통해 호스트에 연결하십시오.

2. 마운트 지점 대상이 존재하는지 확인하십시오.<br />
   **참고**: Plesk에는 백업 저장을 위한 두 가지 옵션이 있습니다. 하나는 내부 Plesk 스토리지(Plesk 서버의 백업 스토리지)입니다. 다른 하나는 외부 FTP 스토리지(웹 또는 로컬 네트워크의 일부 외부 서버에 있는 백업 스토리지)입니다. 일반적으로 Plesk 상자에서 내부 백업은 `/var/lib/psa/dumps`에 저장되며 `/tmp`를 임시 디렉토리로 사용합니다. 예제에서는 임시 디렉토리를 로컬로 유지하지만 덤프 디렉토리를 STaaS 대상(`/backup/psa/dumps`)으로 이동합니다. FTP 사용자 신임 정보는 필수입니다.
   
3. [Red Hat Enterprise Linux에서 {{site.data.keyword.filestorage_short}} 액세스](accessing-file-storage-linux.html) 및 [CentOS에 NFS/{{site.data.keyword.filestorage_short}} 마운트](mounting-nsf-file-storage.html)에서 설명하는 대로 {{site.data.keyword.filestorage_short}}를 구성하십시오. 볼륨을 `/backup`에 마운트하고 파일 시스템 테이블 `/etc/fstab`에서 이를 구성하여 부팅 시에 마운트를 사용하십시오. <br />
   **참고**: 기본적으로 NFS는 루트 권한으로 작성된 파일을 nobody 사용자로 다운그레이드합니다. 루트 클라이언트가 NFS 공유에서 루트 권한을 유지할 수 있도록 하려면 `no_root_squash`를 `/etc/exports`에 추가해야 합니다. <br />
   **참고**: [CoreOS에 NFS/{{site.data.keyword.filestorage_short}} 마운트](mounting-storage-coreos.html)에도 사용 가능한 지시사항이 있습니다. <br />

4. **선택사항**: 기존 백업을 새 스토리지로 복사하십시오. 예를 들어, `rsync`를 사용하십시오.
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {:pre}
    
    **참고**: 이 명령은 가능한 많이 보존된 상태로 압축된 데이터를 전송합니다(하드링크는 제외). 또한 맨 끝에 간략한 설명과 전송 중인 파일에 대한 정보를 제공합니다.
    
5. 새 대상에서 `DUMP_D` 값을 지시하도록 `/etc/psa/psa.conf`를 편집하십시오. 
    - `DUMP_D /backup/psa/dumps`처럼 표시되어야 합니다. 

6. **선택사항**: 특정 유스 케이스 및 비즈니스 요구사항에서 지시하는 대로 서버에서 이전 스토리지를 제거하고 계정에서 취소하십시오.

