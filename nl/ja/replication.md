---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-11"

keywords: File Storage, file storage, NFS, replication, duplication, synchronous, replica schedule, replica space, disaster recovery

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# データのレプリケーション
{: #replication}

レプリケーションでは、スナップショット・スケジュールの 1 つを使用して、スナップショットを自動的にリモート・データ・センター内の宛先ボリュームにコピーします。 壊滅的なイベントが発生した場合やデータが破損した場合、コピーをリモート・サイトでリカバリーすることができます。

レプリケーションを行うと、2 つの異なる場所にあるデータの同期を保つことができます。 使用しているボリュームを複製したものを元のボリュームとは独立して使用する場合は、[複製ファイル・ボリュームの作成](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)を参照してください。
{:tip}

レプリケーションを行うには、その前にスナップショット・スケジュールを作成する必要があります。
{:important}


## 複製ストレージ・ボリュームのリモート・データ・センターの判別

{{site.data.keyword.cloud}} のデータ・センターは、世界的規模でプライマリーとリモートの組み合わせのペアを構成しています。
データ・センターの可用性とレプリケーションのターゲットの完全なリストについては、表 1 を参照してください。

<table>
  <caption style="text-align: left;"><p>表 1 - この表には、拡張機能を備えているすべてのデータ・センターを地域別にリストしています。 地域ごとに列を分けています。 ダラス、サンホセ、ワシントン DC、アムステルダム、フランクフルト、ロンドン、シドニーなどのいくつかの都市には、複数のデータ・センターがあります。</p>
  <p>&#42; 米国 1 地域のデータ・センターには、拡張ストレージがありません。 拡張ストレージ機能を備えたデータ・センターのホストは、米国 1 のデータ・センターのレプリカ・ターゲットとのレプリケーションを開始<strong>できません</strong>。</p>
  </caption>
  <thead>
    <tr>
      <th>米国 1 &#42;</th>
      <th>米国 2</th>
      <th>ラテンアメリカ</th>
      <th>カナダ</th>
      <th>ヨーロッパ</th>
      <th>アジア太平洋</th>
      <th>オーストラリア</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>DAL01<br />
          DAL05<br />
	  DAL06<br />
	  HOU02<br />
	  SJC01<br />
	  WDC01<br />
	  <br /><br /><br /><br /><br /><br />
      </td>
      <td>SJC03<br />
	  SJC04<br />
	  WDC04<br />
	  WDC06<br />
	  WDC07<br />
	  DAL09<br />
	  DAL10<br />
	  DAL12<br />
	  DAL13<br />
	  <br /><br /><br />
      </td>
      <td>MEX01<br />
	  SAO01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>TOR01<br />
          MON01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>AMS01<br />
	  AMS03<br />
	  FRA02<br />
	  FRA04<br />
	  FRA05<br />
	  LON02<br />
	  LON04<br />
	  LON05<br />
	  LON06<br />
	  OSL01<br />
	  PAR01<br />
	  MIL01<br />
      </td>
      <td>HKG02<br />
          TOK02<br />
	  TOK04<br />
	  TOK05<br />
	  SNG01<br />
	  SEO01<br />
          CHE01<br />
	  <br /><br /><br /><br /><br />
      </td>
      <td>SYD01<br />
          SYD04<br />
          SYD05<br />
          MEL01<br />
          <br /><br /><br /><br /><br /><br /><br /><br />
      </td>
    </tr>
  </tbody>
</table>

## 初期レプリカの作成

レプリケーションは、スナップショット・スケジュールに基づいて作動します。 レプリケーションを実行するには、その前にまずソース・ボリューム用のスナップショット・スペースとスナップショット・スケジュールを準備する必要があります。 レプリケーションをセットアップしようとして、そのいずれかが準備されていない場合には、より多くのスペースを購入するか、スケジュールをセットアップするようプロンプトが出されます。 レプリケーションは、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}の**「ストレージ」 ** > **「{{site.data.keyword.filestorage_short}}」**で管理されます。

1. ストレージ・ボリュームをクリックします。
2. **「レプリカ」**をクリックし、**「レプリケーションの購入 (Purchase a replication)」**をクリックします。
3. レプリケーションに使用する既存のスナップショット・スケジュールを選択します。 リストには、すべてのアクティブなスナップショット・スケジュールが入っています。 <br />

   時間単位、日単位、および週単位が混在する場合でも、選択できるスケジュールは 1 つだけです。 前回のレプリケーション・サイクル以降に収集されたすべてのスナップショットが、それらを生成したスケジュールに関係なく複製されます。<br />スナップショットがセットアップされていない場合は、それをセットアップしてからレプリケーションを注文するように求めるプロンプトが表示されます。 詳しくは、[スナップショットの処理](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)を参照してください。
   {:tip}
3. **「ロケーション」**をクリックして、DR サイトにするデータ・センターを選択します。
4. **「次へ進む (Continue)」**をクリックします。
5. **プロモーション・コード**がある場合は入力し、**「再計算」**をクリックします。 ウィンドウ内のその他のフィールドは、デフォルトで入力されています。

   注文の処理時に割引が適用されます。
   {:note}
6. **「マスター・サービス契約を読み…」**チェック・ボックスをクリックし、**「注文する」**をクリックします。


## 既存のレプリケーションの編集

[{{site.data.keyword.slportal}}![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/){: external}から、**「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**の下にある**「プライマリー」**または**「レプリカ」**タブで、レプリケーション・スケジュールを編集してレプリケーション・スペースを変更できます。


## レプリケーション・スケジュールの編集

レプリケーション・スケジュールは既存のスナップショット・スケジュールに基づいています。 レプリケーション・スケジュールを「毎時」から「毎日」や「毎週」に、またはその逆に変更するには、レプリカ・ボリュームをキャンセルして新しいものをセットアップする必要があります。

ただし、**「毎日」**のレプリケーションが行われる時刻を変更する場合には、「プライマリー」タブまたは「レプリカ」タブで既存のスケジュールを調整することができます。

1. **「プライマリー」**タブまたは**「レプリカ」**タブで、**「アクション」**をクリックします。
2. **「スナップショット・スケジュールの編集」**を選択します。
3. **「スケジュール」**の**「スナップショット」**フレームで、レプリケーションに使用しているスケジュールを確認します。 必要に応じてスケジュールを変更します。 
4. **「保存」**をクリックします。


## レプリケーション・スペースの変更

プライマリー・スナップショット・スペースとレプリケーション・スペースは同じでなければなりません。 **「プライマリー」**タブまたは**「レプリカ」**タブでスペースを変更すると、ソースと宛先の両方のデータ・センターにスペースが自動的に追加されます。 スナップショット・スペースを増やすと、レプリケーションの即時更新もトリガーされます。

1. **「プライマリー」**タブまたは**「レプリカ」**タブで、**「アクション」**をクリックします。
2. **「スナップショット・スペースの追加」**を選択します。
3. リストからストレージ・サイズを選択して、**「次へ進む (Continue)」**をクリックします。
4. **プロモーション・コード**がある場合は入力し、**「再計算」**をクリックします。 ダイアログ・ボックスのその他のフィールドは、デフォルトで入力されています。
5. **「マスター・サービス契約を読み…」**チェック・ボックスをクリックし、**「注文する」**をクリックします。


## 1 次データ・センター内のスナップショット・スペースが増加するときにレプリカ・データ・センター内のスナップショット・スペースも増加する

ボリューム・サイズは、プライマリーとレプリカのストレージ・ボリュームで同じでなければなりません。 一方がもう一方よりも大きくてはなりません。 プライマリー・ボリュームのスナップショット・スペースを増やすと、レプリカ・スペースは自動的に増えます。 スナップショット・スペースを増やすと、レプリケーションの即時更新がトリガーされます。 両方のボリュームの増加が、請求書に品目として表示され、必要に応じて日割り計算されます。

スナップショット・スペースを増やす方法について詳しくは、[スナップショット](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)を参照してください。
## ボリューム・リストでのレプリカ・ボリュームの表示

**「ストレージ」**>**「{{site.data.keyword.filestorage_short}}」**の下の「{{site.data.keyword.filestorage_short}}」ページでレプリケーション・ボリュームを確認できます。 ボリューム名には、プライマリー・ボリュームの名前とそれに続いて REP が表示されます。 **「タイプ」**は、「エンデュランス - レプリカ」または「パフォーマンス - レプリカ」です。 **「ターゲット・アドレス」**は、「N/A」であり (レプリカ・ボリュームはレプリカ・データ・センターでマウントされていないため)、**「状況」**には「非アクティブ」と表示されます。


## レプリカ・データ・センターでの複製されたボリュームの詳細の表示

**「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**の下の**「レプリカ」**タブで、レプリカ・ボリュームの詳細を確認できます。 もう 1 つの方法としては、**「{{site.data.keyword.filestorage_short}}」**ページからレプリカ・ボリュームを選択して、**「レプリカ」**タブをクリックします。


## レプリケーション履歴の表示

レプリケーション履歴は、**「管理」**の**「アカウント」**タブ上の**「監査ログ」**で表示できます。 プライマリーとレプリカの両方のボリュームに、同一のレプリケーション履歴が表示されます。以下の項目が示されます。

- レプリケーションのタイプ (フェイルオーバーまたはフェイルバック)。
- 開始日時
- レプリケーションに使用されたスナップショット。
- レプリケーションのサイズ。
- 完了日時。


## レプリカの複製の作成

既存の {{site.data.keyword.cloud}} {{site.data.keyword.filestorage_full}} の複製を作成できます。 複製ボリュームは元のストレージ・ボリュームの容量とパフォーマンスのオプションをデフォルトで継承し、スナップショットの時点までのデータの複製を保管します。

複製はプライマリー・ボリュームからでもレプリカ・ボリュームからでも作成できます。 新しい複製は元のボリュームと同じデータ・センターに作成されます。 レプリカ・ボリュームから複製を作成した場合、新規ボリュームはレプリカ・ボリュームと同じデータ・センターに作成されます。

ストレージがプロビジョンされたら、ただちに複製ボリュームに対してホストから読み取り/書き込みアクセスを行えます。 ただし、元のボリュームから複製へのデータ・コピーが完了するまで、スナップショットおよびレプリケーションは許可されません。

詳しくは、[複製のファイル・ボリュームの作成](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)を参照してください。

## レプリカを使用して災害発生時にフェイルオーバーする

フェイルオーバー時には、プライマリー・データ・センターのストレージ・ボリュームからリモート・データ・センターの宛先ボリュームにスイッチを「切り替え」ます。 例えば、プライマリー・データ・センターがロンドンにあり、セカンダリー・データ・センターがアムステルダムにあるとします。 障害が発生したら、アムステルダムにフェイルオーバーします。つまり、アムステルダムのコンピューティング・インスタンスにある新プライマリー・ボリュームに接続します。 ロンドンのボリュームが修復されたら、ロンドンにフェイルバックしてロンドンのコンピューティング・インスタンスのボリュームをプライマリーに戻すために、アムステルダムのボリュームのスナップショットが作成されます。

* 1 次の場所で問題が発生したときに、ストレージとホストは引き続きオンラインになっている場合は、[プライマリー・ボリュームがアクセス可能である場合のフェイルオーバー](/docs/infrastructure/FileStorage?topic=FileStorage-dr-accessible)を参照してください。
* 1 次の場所がダウンしている場合は、[プライマリー・ボリュームがアクセス不能になった場合のフェイルオーバー](/docs/infrastructure/FileStorage?topic=FileStorage-dr-inaccessible)を参照してください。

## 既存のレプリケーションのキャンセル

レプリケーションは、即時にキャンセルすることも、支払い日にキャンセルすることもでき、そこで請求が終了します。 レプリケーションのキャンセルは、**「プライマリー」**タブまたは**「レプリカ」**タブで実行できます。

1. **「{{site.data.keyword.filestorage_short}}」**ページからボリュームをクリックします。
2. **「プライマリー」**タブまたは**「レプリカ」**タブで、**「アクション」**をクリックします。
3. **「レプリカのキャンセル」**を選択します。
4. キャンセルするタイミングを選択します。 **「即時」**または**「請求料金の確定日」**を選択し、**「続行」**をクリックしてください。
5. **「私は、キャンセルによりデータ損失が生じる可能性があることを了解します」**をクリックして、**「レプリカのキャンセル」**をクリックします。


## プライマリー・ボリュームをキャンセルしたときのレプリケーションのキャンセル

　プライマリー・ボリュームがキャンセルされると、レプリカ・データ・センターのレプリケーション・スケジュールとボリュームは削除されます。 レプリカは、**「{{site.data.keyword.filestorage_short}}」**ページからキャンセルされます。

 1. **「{{site.data.keyword.filestorage_short}}」**ページでボリュームを強調表示します。
 2. **「アクション」**をクリックし、**「{{site.data.keyword.filestorage_short}}のキャンセル (Cancel for {{site.data.keyword.filestorage_short}})」**を選択します。
 3. ボリュームをキャンセルするタイミングを選択します。 **「即時」**または**「請求料金の確定日」**を選択し、**「続行」**をクリックしてください。
 4. **「私は、キャンセルによりデータ損失が生じる可能性があることを了解します」**をクリックして、**「キャンセル」**をクリックします。

## SLCLI でのレプリケーション関連のコマンド
{: #clicommands}

* 特定のボリュームの適切な複製データ・センターをリストします。
  ```
  # slcli file replica-locations --help
  Usage: slcli file replica-locations [OPTIONS] VOLUME_ID

  Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: ID, Long Name, Short Name
  -h, --help      Show this message and exit.
  ```

* File Storage レプリカ・ボリュームを注文します。
  ```
  # slcli file replica-order --help
  Usage: slcli file replica-order [OPTIONS] VOLUME_ID

  Options:
  -s, --snapshot-schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                                  Snapshot schedule to use for replication,
                                  (INTERVAL | HOURLY | DAILY | WEEKLY)
                                  [required]
  -l, --location TEXT             Short name of the data center for the
                                  replicant (e.g.: dal09)  [required]
  --tier [0.25|2|4|10]            Endurance Storage Tier (IOPS per GB) of the
                                  primary volume for which a replicant is
                                  ordered [optional]
  -h, --help                      Show this message and exit.
  ```

* ファイル・ボリュームの既存レプリカ・ボリュームをリストします。
  ```
  # slcli file replica-partners --help
  Usage: slcli file replica-partners [OPTIONS] VOLUME_ID

  Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: ID, Username, Account ID,
                  Capacity (GB), Hardware ID, Guest ID, Host ID
  -h, --help      Show this message and exit.
  ```

* ファイル・ボリュームを特定のレプリカ・ボリュームにフェイルオーバーします。
  ```
  # slcli file replica-failover --help
  Usage: slcli file replica-failover [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  --immediate          Failover to replicant immediately.
  -h, --help           Show this message and exit.
  ```

* 特定のレプリカ・ボリュームからファイル・ボリュームをフェイルバックします。
  ```
  # slcli file replica-failback --help
  Usage: slcli file replica-failback [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  -h, --help           Show this message and exit.
  ```
