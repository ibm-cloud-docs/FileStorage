---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-24"

keywords: File Storage, modify volume, NFS, file storage, expand capacity

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# ファイル共有容量の拡張
{: #expandCapacity}

この新機能により、{{site.data.keyword.filestorage_full}} の現行ユーザーは、{{site.data.keyword.filestorage_short}} のサイズを GB 単位で最大 12 TB まですぐに拡張することができます。 拡張したボリュームに複製を作成したり手動でデータをマイグレーションしたりする必要はありません。 サイズ変更中に、ストレージが停止したり、利用できなくなったりすることはありません。

ボリュームの請求方法が自動的に更新され、新しい価格の日割り額が現在の請求サイクルに加算されます。 そして、次の請求サイクルでは新しい価格が全額請求されます。

この機能は、[一部のデータ・センター](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC)でのみ使用できます。

## 拡張可能ストレージの利点

- **コスト管理** – データが増加する可能性があることがわかっていても少量のストレージから始めたいという場合があります。 拡張機能により、お客様はストレージの初期コストを抑え、後からニーズに合わせて増やすことができます。  

- **ストレージ必要量の増加** - 急速な増加を経験しているお客様は、迅速かつ簡単にストレージのサイズを増やして増加に対応できる必要があります。

## ストレージ容量を拡張した場合のレプリケーションへの影響

プライマリー・ストレージで拡張アクションを実行すると、自動的にレプリカのサイズが変更されます。

## 制限
{: #limitsofextension}

この機能は、拡張機能を備えた[データ・センター](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC)でプロビジョンされたストレージにのみ使用できます。 これらのデータ・センターでプロビジョンされた暗号化ストレージは、最大 12 TB まで増やすことができます。

エンデュランスでプロビジョンされた{{site.data.keyword.filestorage_short}}の既存のサイズ制限が引き続き適用されます (10 IOPS ティアの場合は最大 4 TB、他のすべてのティアは最大 12 TB)。

## ストレージのサイズ変更
{: #resizingsteps}

1. [{{site.data.keyword.cloud}} コンソール](https://{DomainName}/){: external}に移動します。 メニューから**「クラシック・インフラストラクチャー」**を選択します。 **「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**をクリックします。
2. リストからボリュームを選択し、**「...」** > **「共有の変更 (Modify Share)」**をクリックします。
3. 新しいストレージ・サイズを GB 単位で入力します。
4. 選択内容と新しい価格を確認します。
5. **「マスター・サービス契約を読み...」**をクリックし、**「注文する」**をクリックします。
6. 新しいストレージ割り振りが数分後に使用可能になります。

OS が追加のストレージ・スペースを認識するようにするためには、ボリュームをアンマウントしてから、変更されたボリュームを再びマウントします。
{:tip}

代わりの方法として、SLCLI で以下のコマンドを使用することができます。
```
# slcli file volume-modify --help
Usage: slcli file volume-modify [OPTIONS] VOLUME_ID

Options:
  -c, --new-size INTEGER        New Size of file volume in GB. ***If no size
                                is given, the original size of volume is
                                used.***
                                Potential Sizes: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [the original size of the volume]
  -i, --new-iops INTEGER        Performance Storage IOPS, between 100 and 6000
                                in multiples of 100 [only for performance
                                volumes] ***If no IOPS value is specified, the
                                original IOPS value of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is less than 0.3, new IOPS/GB
                                must also be less than 0.3. If original
                                IOPS/GB for the volume is greater than or
                                equal to 0.3, new IOPS/GB for the volume must
                                also be greater than or equal to 0.3.]
  -t, --new-tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) [only for
                                endurance volumes] ***If no tier is specified,
                                the original tier of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is 0.25, new IOPS/GB for the
                                volume must also be 0.25. If original IOPS/GB
                                for the volume is greater than 0.25, new
                                IOPS/GB for the volume must also be greater
                                than 0.25.]
  -h, --help      Show this message and exit.
```
