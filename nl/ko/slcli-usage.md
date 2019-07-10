---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

keywords: File Storage, NSF, SLCLI, API

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}

# {{site.data.keyword.filestorage_short}}용 SLCLI CLI 명령
{: #SLCLIcommands}

SLCLI를 사용하여 일반적으로 [{{site.data.keyword.cloud}} 콘솔](https://{DomainName}/classic){: external}을 통해 처리되는 조치를 수행할 수 있습니다. 예를 들어, SLCLI를 사용하면 새 볼륨, 스냅샷 영역, 복제에 대한 주문, 권한 업데이트 및 볼륨 취소 등의 작업을 수행할 수 있습니다.

SLCLI 설치 및 사용 방법에 대한 자세한 정보는 [Python API 클라이언트](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}를 참조하십시오.
{:tip}

## 액세스 관련 SLCLI 명령
* [{{site.data.keyword.filestorage_short}} 관리](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)  
  ```
  slcli file access-authorize
  slcli file access-list
  slcli file access-revoke
  ```

## 복제 관련 SLCLI 명령

* [복제 관련 SLCLI 명령](/docs/infrastructure/FileStorage?topic=FileStorage-replication#clicommands)
  ```
  slcli file replica-failback
  slcli file replica-failover
  slcli file replica-locations
  slcli file replica-order
  slcli file replica-partners
  ```

## 스냅샷 관련 SLCLI 명령

* [스냅샷 구매](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots)
  ```
  slcli file snapshot-order
  ```

* [스냅샷 관리](/docs/infrastructure/FileStorage?topic=FileStorage-managingSnapshots)
  ```
  slcli file snapshot-cancel
  slcli file snapshot-create
  slcli file snapshot-delete
  slcli file snapshot-disable
  slcli file snapshot-enable
  slcli file snapshot-list
  slcli file snapshot-restore
  ```

## 볼륨 관련 SLCLI 명령

* [{{site.data.keyword.filestorage_short}} 볼륨 주문](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI)
* [복제 볼륨 작성](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)
  ```
  slcli file volume-duplicate
  ```
* [IOPS 조정](/docs/infrastructure/FileStorage?topic=FileStorage-adjustingIOPS#adjustingsteps)
  ```
  slcli file volume-modify
  ```
* [용량 확장](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity#resizingsteps)
  ```
  slcli file volume-modify
  ```
* [{{site.data.keyword.filestorage_short}} 관리](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)
  ```
  slcli file volume-cancel
  slcli file volume-count
  slcli file volume-detail
  slcli file volume-duplicate
  slcli file volume-list
  slcli file volume-order
  ```
* [스토리지 한계 관리](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits)
  ```
  slcli file volume-count
  ```
