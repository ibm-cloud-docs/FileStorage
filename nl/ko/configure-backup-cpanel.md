---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---
{:new_window: target="_blank"}
{:pre: .pre}
 
# cPanel로 백업하기 위한 {{site.data.keyword.filestorage_short}} 구성

이 기사에서는 cPanel을 통해 {{site.data.keyword.filestorage_full}}에 저장할 백업을 구성하는 지시사항을 제공하고자 합니다. 여기서는 루트 또는 sudo SSH 및 전체 WHM(WebHost Manager) 액세스가 사용 가능하다고 가정합니다. 이 예제는 **CentOS 7** 호스트를 기반으로 합니다.

**참고**: 백업 디렉토리 구성을 위한 cPanel의 문서는 [여기](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}에서 찾을 수 있습니다.

1. SSH를 통해 호스트에 연결하십시오.

2. 마운트 지점 대상이 존재하는지 확인하십시오.<br />
   **참고**: 기본적으로, cPanel 시스템은 백업 파일을 로컬로 `/backup` 디렉토리에 저장합니다. 이 문서에서 `/backup` 폴더가 존재하며 백업이 포함되어 있다고 가정하므로 `/backup2`를 새 마운트 지점으로 사용합니다.
   
3. [Red Hat Enterprise Linux에서 {{site.data.keyword.filestorage_short}} 액세스](accessing-file-storage-linux.html) 및 [CentOS에 NFS/{{site.data.keyword.filestorage_short}} 마운트](mounting-nsf-file-storage.html)에서 설명하는 대로 {{site.data.keyword.filestorage_short}}를 구성하십시오. 볼륨을 `/backup2`에 마운트하고 파일 시스템 테이블 `/etc/fstab`에서 이를 구성하여 부팅 시에 마운트를 사용하십시오. <br />
   **참고**: 기본적으로, NFS는 루트 권한으로 작성된 파일을 nobody 사용자에게 다운그레이드합니다. 루트 클라이언트가 NFS 공유에서 루트 권한을 유지할 수 있도록 하려면 `no_root_squash`를 `/etc/exports`에 추가해야 합니다. <br />
   **참고**: [CoreOS에 NFS/{{site.data.keyword.filestorage_short}} 마운트](mounting-storage-coreos.html)에도 사용 가능한 지시사항이 있습니다. <br />

4. **선택사항**: 기존 백업을 새 스토리지로 복사하십시오. 예를 들어, `rsync`를 사용하십시오.
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    **참고**: 이 명령은 가능한 많이 보존된 상태로 압축된 데이터를 전송합니다(하드링크는 제외). 또한 맨 끝에 간략한 설명과 전송 중인 파일에 대한 정보를 제공합니다.
    
5. WebHost Manager에 로그인하고 **홈** > **백업** > **백업 구성**을 통해 백업 구성으로 이동하십시오.

6. 새 마운트 지점에 백업을 저장하도록 구성을 편집하십시오. 
    - `/backup/` 디렉토리 대신 새 위치에 대한 절대 경로를 입력하여 기본 백업 디렉토리를 변경하십시오. 
    - **백업 드라이브 마운트 사용**을 선택하십시오. 이 설정으로 구성 프로세스는 백업 마운트(`/backup2`)에 대한 `/etc/fstab` 파일을 확인할 수 있습니다. <br /> **참고**: 마운트가 스테이징 디렉토리와 동일한 이름으로 존재하는 경우에는 백업 구성 프로세스가 드라이브를 마운트하고 이 드라이브에 정보를 백업합니다. 백업 프로세스가 완료되면 이는 드라이브를 마운트 해제합니다. 

7. **백업 구성** 인터페이스의 맨 아래에서 **구성 저장**을 클릭하여 변경사항을 적용하십시오.

8. **선택사항**: 특정 유스 케이스 및 비즈니스 요구사항에서 지시하는 대로 서버에서 이전 스토리지를 제거하고 계정에서 취소하십시오.
