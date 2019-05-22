---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, adjusting IOPS, increase IOPS, decrease IOPS, modify IOPS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# IOPS の調整
{: #adjustingIOPS}

この新機能により、{{site.data.keyword.filestorage_full}} ストレージ・ユーザーは、既存の{{site.data.keyword.filestorage_short}}の IOPS をただちに調整できます。 新しいストレージに複製を作成したり手動でデータをコピーしたりする必要はありません。 調整中に、ストレージが停止したり、利用できなくなったりすることはありません。

ストレージの請求方法が更新され、新しい価格の日割り額が現在の請求サイクルに加算されます。 次の請求サイクルでは新しい価格が全額請求されます。


## 調整可能な IOPS の利点

- コスト管理 – 高い IOPS はピーク使用時にしか必要ないというクライアントがいます。 例えば、大規模な小売店はホリデー・シーズンがピーク使用になるので、この時期は真夏と比べてストレージに必要な IOPS が高くなる可能性があります。 この機能を使用すると、コストを管理し、必要なときにだけ高い IOPS を購入することができます。

## 制限
{: #limitsofadjustIOPS}

この機能は、[一部のデータ・センター](/docs/infrastructure/FileStorage?topic=FileStorage-news)でのみ使用できます。

クライアントは IOPS を調整する際に、エンデュランスとパフォーマンスを切り替えることはできません。 ユーザーは、次の基準や制限に基づいて、ストレージの新しい IOPS ティアまたは IOPS レベルを指定できます。

- 元のボリュームがエンデュランス 0.25 ティアである場合は、IOPS ティアを更新できません。
- 元のボリュームが 0.30 IOPS/GB 以下のパフォーマンスである場合、0.30 IOPS/GB 以下になる組み合わせのサイズと IOPS しかオプションとして提供されません。
- 元のボリュームが 0.30 IOPS/GB より高いパフォーマンスである場合、0.30 IOPS/GB より高くなる組み合わせのサイズと IOPS しかオプションとして提供されません。

## レプリケーションに対する IOPS 調整の影響

ボリュームでレプリケーションが実行されている場合は、プライマリーの IOPS の選択と一致するように自動的にレプリカが更新されます。

## 実際のストレージでの IOPS の調整
{: #adjustingsteps}

1. {{site.data.keyword.filestorage_short}}のリストに進みます
    - カスタマー・ポータルから、**「ストレージ」**>**「{{site.data.keyword.filestorage_short}}」**をクリックします。または
    - {{site.data.keyword.BluSoftlayer_full}} コンソールで、**「インフラストラクチャー」** > **「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**をクリックします。
2. リストからボリュームを選択し、**「アクション」** > **「ボリュームの変更 (Modify Volume)」**をクリックします。
3. **「ストレージ IOPS オプション (Storage IOPS Options)」**で、新しい選択を行います。
    - 「エンデュランス (IOPS ティア) (Endurance (Tiered IOPS))」の場合、0.25 IOPS/GB より大きいストレージの IOPS ティアを選択します。 IOPS ティアはいつでも上げることができます。 ただし、下げることができるのは、月に 1 回のみです。
    - 「パフォーマンス (IOPS 割り振り) (Performance (Allocated IOPS))」の場合、IOPS として 100 から 48,000 までの範囲の値を入力して、ストレージの新しい IOPS オプションを指定します。 (サイズ別に定義されている具体的な範囲を注文フォームで確認してください)。
4. 選択内容と新しい価格を確認します。
5. **「マスター・サービス契約を読み...」**チェック・ボックスをクリックし、**「注文する」**をクリックします。
6. 新しいストレージ割り振りが数分後に使用可能になります。

代わりの方法として、SLCLI で IOPS を更新することもできます。
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
