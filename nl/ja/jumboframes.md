---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

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


# {{site.data.keyword.BluSoftlayer_notm}} でジャンボ・フレームを有効にする - Windows および Linux
{: #jumboframes}

ジャンボ・フレームとは、標準最大伝送単位 (MTU) である 1,500 バイトよりペイロードが大きいイーサネット・フレームです。 ジャンボ・フレームは、1 Gbps 以上をサポートするローカル・エリア・ネットワークで使用され、9,000 バイトのサイズにすることができます。

ソース・デバイスから <-> スイッチ <-> ルーター <-> スイッチ <-> 宛先デバイスまでのネットワーク・パス全体で、同じジャンボ・フレームを構成する必要があります。 チェーン全体で同じ設定になっていないと、チェーン全体がデフォルトの最小設定に変更されます。 {{site.data.keyword.BluSoftlayer_full}} のネットワーク・デバイス数は、現在 9,000 に設定されています。 すべてのカスタマー・デバイスを同じ 9,000 の値に設定する必要があります。
{:important}

## Windows でのジャンボ・フレームの有効化

1. **「ネットワークと共有センター」**を開きます。
2. **「アダプターの設定の変更」**をクリックします。
3. ジャンボ・フレームを有効にする NIC を右クリックし、**「プロパティ」**を選択します。
4. **「ネットワーク」**タブでネットワーク・アダプターの**「構成」**をクリックします。
5. **「詳細設定」**タブを選択します。
6. **「ジャンボ・フレーム」**を選択し、値を**「無効」**から変更して適切な値にします。 値 (9 KB や 9014 バイトなど) は、NIC に応じて異なります。
7. すべてのウィンドウで**「OK」**をクリックします。

変更を加えると、数秒間 NIC のネットワーク接続が失われます。 デバイスを再起動して、変更が有効になるようにしてください。
{:tip}


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
