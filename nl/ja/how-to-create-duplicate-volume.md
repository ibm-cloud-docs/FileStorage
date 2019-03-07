---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 複製{{site.data.keyword.filestorage_short}}の作成
{: #duplicatevolume}

既存の {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.filestorage_full}} の複製を作成できます。 複製ボリュームは元のボリュームの容量とパフォーマンスのオプションをデフォルトで継承し、スナップショットの時点までのデータの複製を保管します。   

複製はポイントインタイム・スナップショットのデータに基づくため、複製を作成するためには、元のボリューム上にスナップショット・スペースが必要です。 スナップショットについて、およびスナップショット・スペースの注文方法について詳しくは、[スナップショットの説明](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)を参照してください。  

複製は**プライマリー**・ボリュームからでも**レプリカ**・ボリュームからでも作成できます。 新しい複製は元のボリュームと同じデータ・センターに作成されます。 レプリカ・ボリュームから複製を作成した場合、新規ボリュームはレプリカ・ボリュームと同じデータ・センターに作成されます。

{{site.data.keyword.containerlong}} の「専用」アカウント・ユーザーである場合は、[{{site.data.keyword.containerlong_notm}} 資料](/docs/containers?topic=containers-backup_restore#backup_restore)にある、ボリュームを複製するためのオプションを参照してください。
{:tip}

ストレージがプロビジョンされたら、ただちに複製ボリュームに対してホストから読み取り/書き込みアクセスを行えます。 ただし、元のボリュームから複製へのデータ・コピーが完了するまで、スナップショットおよびレプリケーションは許可されません。 データ・コピーが完了したら、複製は独立したボリュームとして管理、使用できます。

この機能は、ほとんどの場所で使用可能です。 使用可能なデータ・センターのリストについては、[ここ](/docs/infrastructure/FileStorage?topic=FileStorage-news)をクリックしてください。

重複ボリュームの一般的な使用法の例を以下に示します。
- **災害復旧テスト**。 災害が発生してもデータが損なわれることなく使用できることを検証するため、レプリケーションを中断せずにレプリカ・ボリュームの複製を作成する。
- **ゴールデン・コピー**。 さまざまな用途のために複数インスタンスを作成できるゴールデン・コピーとしてストレージ・ボリュームを使用する。
- **データ・リフレッシュ**。 テスト用の非実稼働環境にマウントするために実動データの複製を作成する。
- **スナップショットからの復元**。 スナップショット復元機能で元のボリューム全体を上書きするのではなく、スナップショット中の特定のファイルおよび日付を使用して、元のボリューム上のデータを復元する。
- **開発とテスト (dev/test)**。 1 つのボリュームから一度に最大 4 つの複製を同時作成して、開発とテスト用の複製データを作成する。
- **ストレージのサイズ変更**。 データを移動することなく、新しいサイズまたは IOPS レート (あるいはその両方) を指定したボリュームを作成する。  

[{{site.data.keyword.slportal}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/){:new_window} で重複ボリュームを作成するには、いくつかの方法があります。


## ストレージ・リスト内の特定のボリュームから複製を作成する

1. {{site.data.keyword.filestorage_short}}のリストに進みます
    - カスタマー・ポータルから、**「ストレージ」**>**「{{site.data.keyword.filestorage_short}}」**をクリックします。または
    - {{site.data.keyword.BluSoftlayer_full}} カタログで、**「インフラストラクチャー」** > **「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**をクリックします。
2. リストから LUN を選択し、**「アクション」** > **「LUN (ボリューム) の複製 (Duplicate LUN (Volume))」**をクリックします。
3. スナップショット・オプションを選択します。
    - 非レプリカ・ボリュームから注文する場合は、次のようにします。
      - **「新規スナップショットから作成 (Create from new snapshot)」**を選択します。このアクションにより、複製に使用するスナップショットが作成されます。 このオプションは、現在、ボリュームのスナップショットがない場合や、今すぐ複製を作成する場合に使用します。</br>
      - **「最新のスナップショットから作成 (Create from latest snapshot)」**を選択します。このアクションにより、このボリュームについて存在する最新のスナップショットから複製が作成されます。
    - レプリカ・ボリュームから注文する場合、スナップショットの唯一のオプションは、使用可能な最新のスナップショットを使用することのみです。
4. ストレージ・タイプとロケーションは、元のボリュームと同じままです。
5. 「時間単位または月単位の請求 (Hourly or Monthly Billing)」– 複製 LUN のプロビジョンに時間単位の請求を使用するか月単位の請求を使用するかを選択できます。 元のボリュームの請求タイプが自動的に選択されます。 複製ストレージに別の請求タイプを選択する場合は、ここでその選択を行うことができます。
5. 必要に応じて、新規ボリュームの IOPS または IOPS ティアを指定できます。 デフォルトでは、元のボリュームの IOPS 指定が設定されます。 選択可能なパフォーマンスとサイズの組み合わせが表示されます。
    - 元のボリュームが 0.25 IOPS エンデュランス・ティアである場合は、新しい選択を行えません。
    - 元のボリュームが 2、4、または 10 IOPS エンデュランス・ティアである場合、新規ボリュームでは、これらのティアのどれにでも移動できます。
6. 新規ボリュームのサイズを更新して元のものより大きくすることができます。 デフォルトでは、元のボリュームのサイズが設定されます。

   {{site.data.keyword.filestorage_short}}は、ボリュームの元のサイズの 10 倍にサイズ変更できます。
   {:tip}
7. 新規ボリュームのスナップショット・スペースを更新して、スナップショット・スペースを増減したり、なくしたりすることができます。 デフォルトでは、元のボリュームのスナップショット・スペースが設定されます。
8. **「続行」**をクリックして、注文します。


## 特定のスナップショットから複製を作成する

1. {{site.data.keyword.filestorage_short}}のリストに進みます
2. リストからボリュームをクリックして詳細ページを表示します。 レプリカ・ボリュームまたは非レプリカ・ボリュームのいずれかになります。
3. 詳細ページでリストをスクロールダウンして既存のスナップショットを選択し、**「アクション」** >**「重複」**をクリックします。   
4. ストレージ・タイプ (エンデュランスまたはパフォーマンス) とロケーションは、元のボリュームと同じままです。
5. 選択可能なパフォーマンスとサイズの組み合わせが表示されます。 デフォルトでは、元のボリュームの IOPS 指定が設定されます。 新規ボリュームの IOPS または IOPS ティアを指定できます。
    - 元のボリュームが 0.25 IOPS エンデュランス・ティアである場合は、新しい選択を行えません。
    - 元のボリュームが 2、4、または 10 IOPS エンデュランス・ティアである場合、新規ボリュームでは、これらのティアのどれにでも移動できます。
6. 新規ボリュームのサイズを更新して元のものより大きくすることができます。 デフォルトでは、元のボリュームのサイズが設定されます。

   {{site.data.keyword.filestorage_short}}は、ボリュームの元のサイズの 10 倍にサイズ変更できます。
   {:tip}
7. 新規ボリュームのスナップショット・スペースを更新して、スナップショット・スペースを増減したり、なくしたりすることができます。 デフォルトでは、元のボリュームのスナップショット・スペースが設定されます。
8. **「次へ進む (Continue)」**をクリックして、複製を注文します。

## SLCLI で複製を作成する
```
# slcli file volume-duplicate --help
Usage: slcli file volume-duplicate [OPTIONS] ORIGIN_VOLUME_ID

Options:
  -o, --origin-snapshot-id INTEGER
                                  ID of an origin volume snapshot to use for
                                  duplcation.
  -c, --duplicate-size INTEGER    Size of duplicate file volume in GB. ***If
                                  no size is specified, the size of the origin
                                  volume will be used.***
                                  Minimum: [the size
                                  of the origin volume]
  -i, --duplicate-iops INTEGER    Performance Storage IOPS, between 100 and
                                  6000 in multiples of 100 [only used for
                                  performance volumes] ***If no IOPS value is
                                  specified, the IOPS value of the origin
                                  volume will be used.***
                                  Requirements: [If
                                  IOPS/GB for the origin volume is less than
                                  0.3, IOPS/GB for the duplicate must also be
                                  less than 0.3. If IOPS/GB for the origin
                                  volume is greater than or equal to 0.3,
                                  IOPS/GB for the duplicate must also be
                                  greater than or equal to 0.3.]
  -t, --duplicate-tier [0.25|2|4|10]
                                  Endurance Storage Tier (IOPS per GB) [only
                                  used for endurance volumes] ***If no tier is
                                  specified, the tier of the origin volume
                                  will be used.***
                                  Requirements: [If IOPS/GB
                                  for the origin volume is 0.25, IOPS/GB for
                                  the duplicate must also be 0.25. If IOPS/GB
                                  for the origin volume is greater than 0.25,
                                  IOPS/GB for the duplicate must also be
                                  greater than 0.25.]
  -s, --duplicate-snapshot-size INTEGER
                                  The size of snapshot space to order for the
                                  duplicate. ***If no snapshot space size is
                                  specified, the snapshot space size of the
                                  origin file volume will be used.***
                                  Input
                                  "0" for this parameter to order a duplicate
                                  volume with no snapshot space.
  --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                  to monthly)
  -h, --help                      Show this message and exit.
```

## 重複ボリュームの管理

元のボリュームから複製にデータがコピーされている間、複写が進行中であることを示す状況が詳細ページに表示されます。 この間、ホストに接続してボリュームに対する読み取り/書き込みを行えますが、スナップショット・スケジュールを作成することはできません。 複写プロセスが完了すると、新規ボリュームはオリジナルから独立したものになり、スナップショットやレプリケーションを使用して通常どおりに管理できます。
