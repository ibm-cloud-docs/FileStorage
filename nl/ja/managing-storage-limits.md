---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# ストレージ制限の管理
{: #managinglimits}

デフォルトでは、合計 250 の {{site.data.keyword.blockstorageshort}} および {{site.data.keyword.filestorage_short}} ボリュームをグローバルにプロビジョンできます。

存在するボリュームの数を確認するには、次のように `slcli` コマンドを使用して、各データ・センターのボリュームをリストすることができます。
```
# slcli file volume-count --help
Usage: slcli file volume-count [OPTIONS]

Options:
 -d, --datacenter TEXT  Datacenter shortname
 --sortby TEXT          Column to sort by
 -h, --help             Show this message and exit.
```

[{{site.data.keyword.slportal}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/){:new_window} でチケットを送信して、制限の拡大を要求できます。 要求が承認されると、特定のデータ・センターでボリューム制限が設定されます。  

制限の拡大を要求するには、チケットを開いて、営業担当員に送信します。

チケットで、以下の情報を指定してください。

- **チケットの件名**: Request to Increase Data Center Volume Count Storage Limit

- **What is the use case for the additional volumes request?** (追加ボリュームの要求の用途) <br />
*例えば、新しい VMware データ・ストア、新しい開発とテスト (dev/test) 環境、SQL データベース、ロギングなどの回答が考えられます。*

- **How many extra Block volumes are needed by type, size, IOPS, and location?** (追加するブロック・ボリュームの数、タイプ、サイズ、IOPS、ロケーション) <br />
*例えば、「25x Endurance 2 TB @ 4 IOPS in DAL09」や「25x Performance 4 TB @ 2 IOPS in WDC04」のような回答を入力します。*

- **How many extra File volumes are needed by type, size, IOPS, and location?** (追加するファイル・ボリュームの数、タイプ、サイズ、IOPS、ロケーション) <br />
*例えば、「25x Performance 20 GB @ 10 IOPS in DAL09」や「50x Endurance 2 TB @ 0.25 IOPS in SJC03」のような回答を入力します。*

- **Provide an estimate of when you expect or plan to provision all of the requested volume increase.** (要求したボリューム増加のすべてのプロビジョンが完了すると見込まれる時間または計画する時間) <br />
 *例えば、「90 days」のような回答を入力します。*

- **Provide a 90-day forecast of expected average capacity usage of these volumes.** (それらのボリュームの 90 日間の推定平均使用量) <br />
*例えば、「expect 25 percent to be used in 30 days, 50 percent to be used in 60 days and 75 percent to be used in 90 days"」のような回答を入力します。*

要求内の質問と文のすべてに回答してください。 処理や承認の際に必要になります。
{:important}

チケット・プロセスによって制限が更新されたことが通知されます。
