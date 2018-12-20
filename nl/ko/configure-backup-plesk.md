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

# Plesk로 백업을 위한 {{site.data.keyword.filestorage_short}} 구성

다음 지시사항을 사용하여 Plesk에서의 백업을 위해 {{site.data.keyword.filestorage_full}}를 구성할 수 있습니다. 여기서는 루트 또는 sudo SSH 및 전체 admin 레벨 Plesk 액세스가 사용 가능하다고 가정합니다. 이 예제는 CentOS7 호스트를 기반으로 합니다.

자세한 정보는 [백업 및 복원에 관한 Plesk 문서![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}을 참조하십시오.
{:tip}

1. SSH를 통해 호스트에 연결하십시오.
2. 마운트 지점 대상이 존재하는지 확인하십시오. <br />

   Plesk에는 백업 저장을 위한 두 가지 옵션이 있습니다. 하나는 Plesk 서버에 있는 스토리지인 내부 Plesk 스토리지입니다. 다른 하나는 웹 또는 로컬 네트워크의 외부 서버에 있는 스토리지인 외부 FTP 스토리지입니다. 일반적으로 Plesk 상자에서 내부 백업은 `/var/lib/psa/dumps`에 저장되며 `/tmp`를 임시 디렉토리로 사용합니다. 이 예에서도 임시 디렉토리는 로컬이 사용되지만, `dumps` 디렉토리는 {{site.data.keyword.filestorage_short}} 대상(`/backup/psa/dumps`)으로 이동되었습니다. FTP 사용자 인증 정보는 필수입니다.
   {:note}
3. [Red Hat Enterprise Linux에서 {{site.data.keyword.filestorage_short}}에 액세스](accessing-file-storage-linux.html) 및 [CentOS에서 NFS/{{site.data.keyword.filestorage_short}} 마운트](mounting-nsf-file-storage.html) 또는 [CoreOS에서 NFS/{{site.data.keyword.filestorage_short}} 마운트](mounting-storage-coreos.html)에 설명되어 있는 바와 같이 {{site.data.keyword.filestorage_short}}를 구성하십시오. 볼륨을 `/backup`에 마운트하고 시작 시 마운트할 수 있도록 이를 파일 시스템 테이블(`/etc/fstab`)에서 구성하십시오. <br />

   기본적으로 NFS는 루트 권한으로 작성된 파일을 nobody 사용자로 다운그레이드합니다. 루트 클라이언트가 NFS 공유에서 루트 권한을 유지할 수 있도록 하려면 `no_root_squash`를 `/etc/exports`에 추가해야 합니다.
   {:tip}
4. **선택사항**: 기존 백업을 새 스토리지에 복사하십시오. 예를 들어, `rsync`를 사용하십시오.
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {:pre}

   이 명령은 데이터를 압축하고 전송하며, 이는 최대한 많은 양을 유지합니다(하드링크의 경우는 제외). 또한 맨 끝에 간략한 설명과 전송 중인 파일에 대한 정보를 제공합니다.
   {:tip}
5. 새 대상에서 `DUMP_D` 값을 지시하도록 `/etc/psa/psa.conf`를 편집하십시오.
    - 이는 `DUMP_D /backup/psa/dumps`와 같이 표시됩니다.
6. **선택사항**: 특정 유스 케이스 및 비즈니스 요구사항에서 지시하는 대로 이전 스토리지를 서버에서 제거하고 계정에서 취소하십시오.
