---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 스토리지 한계 관리
{: #managinglimits}

기본적으로 총 250개의 결합된 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}} 볼륨을 전 세계적으로 프로비저닝할 수 있습니다.

보유하고 있는 볼륨이 얼마인지 잘 모르는 경우 다음 `slcli` 명령을 사용하여 각 데이터 센터의 볼륨을 나열할 수 있습니다.
```
# slcli file volume-count --help
Usage: slcli file volume-count [OPTIONS]

Options:
 -d, --datacenter TEXT  Datacenter shortname
 --sortby TEXT          Column to sort by
 -h, --help             Show this message and exit.
```

[{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}에서 티켓을 제출하여 한계 늘리기를 요청할 수 있습니다. 요청이 승인되면 특정 데이터 센터에 대해 설정된 볼륨 한계를 받습니다.  

한계 늘리기를 요청하려면 티켓을 열고 이를 영업 담당자에게 보내십시오.

티켓에서 다음 정보를 제공하십시오.

- **티켓 제목**: 데이터 센터 볼륨 개수 스토리지 한계 늘리기 요청

- **추가 볼륨 요청에 대한 유스 케이스는 무엇입니까?** <br />
*이에 대한 답변은 새 VMware 데이터 저장소, 새 개발 및 테스트 환경, SQL 데이터베이스 또는 로깅 등일 수 있습니다.*

- **유형, 크기, IOPS 및 위치별로 필요한 추가 블록 볼륨의 수는 얼마입니까?** <br />
*예를 들어, 답변은 "DAL09의 25x Endurance 2TB @ 4IOPS" 또는 "WDC04의 25x Performance 4TB @ 2IOPS"와 유사한 것일 수 있습니다.*

- **유형, 크기, IOPS 및 위치별로 필요한 추가 파일 볼륨의 수는 얼마입니까?** <br />
*예를 들어, 답변은 "DAL09의 25x Performance 20GB @ 10IOPS" 또는 "SJC03의 50x Endurance 2TB @ 0.25IOPS"와 유사한 것일 수 있습니다.*

- **요청된 모든 볼륨 늘리기의 프로비저닝에 대한 예상 또는 계획 시점을 제공하십시오.** <br />
 "*예를 들어, 답변은 "90일"과 유사한 것일 수 있습니다.*

- **이러한 볼륨의 예상된 평균 용량 사용량의 90일 예측치를 제공하십시오.** <br />
*이에 대한 답변은 "30일에 25퍼센트 이용, 60일에 50퍼센트 이용 및 90일에 75퍼센트 이용이 예상됨" 등일 수 있습니다. *

요청의 모든 질문과 명령문에 응답하십시오. 해당 사항은 처리와 승인에 필요합니다.
{:important}

사용자는 티켓 프로세스를 통해 한계에 대한 업데이트의 알림을 받습니다.
