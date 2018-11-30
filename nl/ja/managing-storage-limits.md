---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# ストレージ制限の管理

デフォルトでは、合計で 250 の {{site.data.keyword.filestorage_short}} ボリュームをグローバルにプロビジョンできます。

[{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}でチケットを送信して、制限の拡大を要求できます。 要求が承認されると、特定のデータ・センターでボリューム制限が設定されます。  

制限の拡大を要求するには、チケットを開いて、営業担当員に送信します。

チケットで、以下の情報を指定してください。

- **チケットの件名**: Request to Increase Data Center Volume Count Storage Limit

- **What is the use case for the additional volumes request?** (追加ボリュームの要求の用途) <br />
*例えば、「a new VMware Datastore」、「a new development and testing (dev/test) environment」、「an SQL database」、「logging」のような回答を入力します。*

- **How many extra Block volumes are needed by type, size, IOPS, and location?** (追加するブロック・ボリュームの数、タイプ、サイズ、IOPS、ロケーション) <br />
*例えば、「25x Endurance 2 TB @ 4 IOPS in DAL09」や「25x Performance 4 TB @ 2 IOPS in WDC04」のような回答を入力します。*

- **How many extra File volumes are needed by type, size, IOPS, and location?** (追加するファイル・ボリュームの数、タイプ、サイズ、IOPS、ロケーション) <br />
*例えば、「25x Performance 20 GB @ 10 IOPS in DAL09」や「50x Endurance 2 TB @ 0.25 IOPS in SJC03」のような回答を入力します。*

- **Provide an estimate of when you expect/plan to provision all of the requested volume increase.** (要求したボリューム増加のすべてのプロビジョンが完了すると見込まれる/計画する時間) <br />
 *例えば、「90 days」のような回答を入力します。*

- **Provide a 90-day forecast of expected average capacity usage of these volumes.** (それらのボリュームの 90 日間の推定平均使用量) <br />
*例えば、「expect 25 percent to be used in 30 days, 50 percent to be used in 60 days and 75 percent to be used in 90 days"」のような回答を入力します。*

上記の質問のすべてに回答する必要があります。 チケット・プロセスによって制限が更新されたことが通知されます。
