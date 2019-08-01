---

copyright:
  years: 2014, 2019
lastupdated: "2019-08-01"

keywords: File Storage, file storage, NSF, networking, jumbo frames

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# {{site.data.keyword.BluSoftlayer_notm}} でジャンボ・フレームを有効にする
{: #jumboframes}

ジャンボ・フレームとは、標準最大伝送単位 (MTU) である 1,500 バイトよりペイロードが大きいイーサネット・フレームです。 ジャンボ・フレームは、1 Gbps 以上をサポートするローカル・エリア・ネットワークで使用され、9,000 バイトのサイズにすることができます。

ソース・デバイスから <-> スイッチ <-> ルーター <-> スイッチ <-> 宛先デバイスまでのネットワーク・パス全体で、同じジャンボ・フレームを構成する必要があります。 チェーン全体で同じ設定になっていないと、チェーン全体がデフォルトの最小設定に変更されます。 {{site.data.keyword.cloud}} のネットワーク・デバイス数は、現在 9,000 に設定されています。 すべてのカスタマー・デバイスを同じ 9,000 の値に設定する必要があります。
{:important}


## Linux でのジャンボ・フレームの有効化

1. eth0 インターフェースのネットワーク構成ファイルを編集します。
   - CentOS、RHEL、Fedora Linux ユーザーは、`/etc/sysconfig/network-script/ifcfg-eth0` を編集します
     ```
     # vi /etc/sysconfig/network-script/ifcfg-eth0
     ```
     {: pre}

   - Debian および Ubuntu Linux ユーザーは、`/etc/network/interfaces` を編集します。

2. フレームのサイズをバイト単位で指定する構成ディレクティブを追加します。
   - CentOS、RHEL、Fedora Linux
     ```
     MTU 9000
     ```
     {: pre}

   - Debian および Ubuntu Linux
     ```
     MTU=9000
     ```
     {: pre}

3. ファイルを閉じて保存します。 インターフェース eth0 を再始動します。
   ```
   # /etc/init.d/networking restart
   ```
   {: pre}

   このアクションにより、ネットワーク接続の短い損失が発生します。
   {:important}
