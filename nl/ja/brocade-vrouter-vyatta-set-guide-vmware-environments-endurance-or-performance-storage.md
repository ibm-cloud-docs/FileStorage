---

copyright:
  years: 2014, 2017
lastupdated: "2018-02-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_short}}を使用する VMware 環境の Brocade vRouter (Vyatta) のセットアップ・ガイド

いずれかの {{site.data.keyword.filestorage_full}} を使用する VMware 環境内で、Brocade vRouter (Vyatta) アプライアンスを高可用性 (HA) 構成にすることができます。以下の情報と『[Advanced Single-Site VMware Reference Architecture](https://console.bluemix.net/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window}』を合わせて使用して、これらのストレージ・オプションのいずれかを VMware 環境にセットアップできます。

## Brocade vRouter (Vyatta) の概要

Brocade vRouter (Vyatta) ゲートウェイは、お客様の環境のゲートウェイおよびルーターとして機能し、サブネットで構成される複数のゾーンが含まれています。 ゾーン間で通信できるように、ゾーンの間にファイアウォール・ルールを設定します。 他のゾーンと通信する必要がないゾーンにはファイアウォール・ルールは不要です。すべてのパケットがドロップされるからです。

この構成例では、Brocade vRouter (Vyatta) 内に以下の 5 つのゾーンを作成します。

- SLSERVICE – {{site.data.keyword.BluSoftlayer_full}} サービス
- VMACCESS – キャパシティー・クラスター上の {{site.data.keyword.BluVirtServers_short}} (VM)
- MGMT – 管理クラスターとキャパシティー・クラスター、および管理 VM
- STORAGE – ストレージ・サーバー
- OUTSIDE - パブリック・インターネット・アクセス


図 1 は、ゾーン間の通信を表しています。ご使用の環境によって、異なるものになったり、別のゾーンやファイアウォール・ルールが必要になったりする可能性があります。

![図 1: Brocade vRouter (Vyatta) ゾーン構成](/images/figure1_6.png)



## Brocade vRouter (Vyatta) の構成

Brocade vRouter (Vyatta) を構成するには、以下のようにします。

1. 「デバイスの詳細」画面にある root パスワードを使用して、アプライアンスに SSH でログインします。
2. 「Configure」と入力して構成モードに入り、後続のセクションの手順に従います。

### インターフェースのセットアップ

このセクションでは、環境内のサブネットにリンクするために、両方の Brocade vRouter (Vyatta) に結合インターフェースを構成します。 この VLAN (1101、1102、および 1103) は、ご使用の環境の対応する VLAN に置き換えてください。また、<> で説明している箇所は、ご使用の環境の詳細情報に置き換える必要があります (<> は除いてください)。

以下のコマンドを使用して、Brocade vRouter (Vyatta) に結合インターフェースを構成します。構成モードに入っておく必要があります。

Brocade vRouter (Vyatta) 1
```
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ (VLAN 1101/管理に結合されるプライマリー・プライベート・サブネットの IP アドレスを入力)
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ (VLAN 1101/管理 VM に結合されるポータブル・プライベート・サブネットの IP アドレスを入力)
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ (VLAN 1102/ストレージ・パス A に結合されるポータブル・プライベート・サブネットの IP アドレスを入力)
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ (VLAN 1102/ストレージ・パス B に結合されるポータブル・プライベート・サブネットの IP アドレスを入力)
set interfaces bonding bond0 vif 1103 address ‘##.###.###.###/##’ (VLAN 1103/仮想マシンに結合されるポータブル・プライベート・サブネットの IP アドレスを入力)
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 advertise-interval '1'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 preempt 'false'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 priority '253'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 virtual-address ‘<1101/管理に結合されるプライマリー・プライベート VLAN のゲートウェイ・アドレス/マスク>’
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 virtual-address ‘<1101/管理 VM に結合されるポータブル・プライベート VLAN のゲートウェイ・アドレス/マスク>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 advertise-interval '1'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 preempt 'false'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 priority '253'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<1102/ストレージに結合されるプライマリー・プライベート VLAN のゲートウェイ・アドレス/マスク>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<1102/ストレージ・パス A に結合されるポータブル・プライベート VLAN のゲートウェイ・アドレス/マスク>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<1102/ストレージ・パス B に結合されるポータブル・プライベート VLAN のゲートウェイ・アドレス/マスク>’
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 advertise-interval '1'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 preempt 'false'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 priority '253'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 virtual-address ‘<1103/仮想マシンに結合されるプライマリー・プライベート VLAN のゲートウェイ・アドレス/マスク>’
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 virtual-address ‘<1103/仮想マシンに結合されるポータブル・プライベート VLAN のゲートウェイ・アドレス/マスク>’
commit
save
```

Brocade vRouter (Vyatta) 2
```
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ (VLAN 1101/管理に結合されるプライマリー・プライベート・サブネットの IP アドレスを入力。Brocade vRouter (Vyatta)1 に割り当てたものとは別のもの)
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ (VLAN 1101/管理 VM に結合されるポータブル・プライベート・サブネットの IP アドレスを入力。Brocade vRouter (Vyatta)1 に割り当てたものとは別のもの)
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ (VLAN 1102/ストレージ・パス A に結合されるポータブル・プライベート・サブネットの IP アドレスを入力。Brocade vRouter (Vyatta)1 に割り当てたものとは別のもの)
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ (VLAN 1102/ストレージ・パス B に結合されるポータブル・プライベート・サブネットの IP アドレスを入力。Brocade vRouter (Vyatta)1 に割り当てたものとは別のもの)
set interfaces bonding bond0 vif 1103 address ‘##.###.###.###/##’ (VLAN 1103/仮想マシンに結合されるポータブル・プライベート・サブネットの IP アドレスを入力。Brocade vRouter (Vyatta)1 に割り当てたものとは別のもの)
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 advertise-interval '1'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 preempt 'false'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 priority '253'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 virtual-address ‘<1101/管理に結合されるプライマリー・プライベート VLAN のゲートウェイ・アドレス/マスク>’
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 virtual-address ‘<1101/管理 VM に結合されるポータブル・プライベート VLAN のゲートウェイ・アドレス/マスク>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 advertise-interval '1'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 preempt 'false'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 priority '253'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<1102/ストレージに結合されるプライマリー・プライベート VLAN のゲートウェイ・アドレス/マスク>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<1102/ストレージ・パス A に結合されるポータブル・プライベート VLAN のゲートウェイ・アドレス/マスク>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<1102/ストレージ・パス B に結合されるポータブル・プライベート VLAN のゲートウェイ・アドレス/マスク>’
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 advertise-interval '1'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 preempt 'false'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 priority '253'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 virtual-address ‘<1103/仮想マシンに結合されるプライマリー・プライベート VLAN のゲートウェイ・アドレス/マスク>’
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 virtual-address ‘<1103/仮想マシンに結合されるポータブル・プライベート VLAN のゲートウェイ・アドレス/マスク>’
commit
save
```
### 外部アクセス用の SNAT の構成

この手順では、管理 VM とキャパシティー・クラスター上の VM がインターネットにアクセスできるように SNAT を構成します。後でセットアップを同期するので、これ以降の手順では、片方の Brocade vRouter (Vyatta) だけを構成します。

構成モードで以下のコマンドを使用します。
```
set nat source rule 10
set nat source rule 10 source address ##.###.###.###/## (ポータブル・プライベート・サブネット VLAN1101/管理 VM)
set nat source rule 10 translation address ##.###.###.### (Brocade vRouter (Vyatta) bond1 IP)
set nat source rule 10 outbound-interface bond1
set nat source rule 20
set nat source rule 20 source address ##.###.###.###/## (ポータブル・プライベート・サブネット VLAN1103/仮想マシン)
set nat source rule 20 translation address ##.###.###.### (Brocade vRouter (Vyatta) bond1 IP)
set nat source rule 20 outbound-interface bond1
commit
save
```
### ファイアウォール・グループの構成

次に、特定の IP 範囲に関連付けられたファイアウォール・グループを構成します。

構成モードで以下のコマンドを使用します。
```
set firewall group network-group SLSERVICES
set firewall group network-group SLSERVICES network 10.1.128.0/19
set firewall group network-group SLSERVICES network 10.0.86.0/24
set firewall group network-group SLSERVICES network 10.1.176.0/24
set firewall group network-group SLSERVICES network 10.1.64.0/19
set firewall group network-group SLSERVICES network 10.1.96.0/19
set firewall group network-group SLSERVICES network 10.1.192.0/20
set firewall group network-group SLSERVICES network 10.1.160.0/20
set firewall group network-group SLSERVICES network 10.2.32.0/20
set firewall group network-group SLSERVICES network 10.2.64.0/20
set firewall group network-group SLSERVICES network 10.0.64.0/19
set firewall group network-group SLSERVICES network 10.2.128.0/20
set firewall group network-group SLSERVICES network 10.2.200.0/24
set firewall group network-group SLSERVICES network 10.1.0.0/24
set firewall group network-group SLSERVICES network 10.1.24.0/24
set firewall group network-group SLSERVICES network 10.2.208.0/24
set firewall group network-group SLSERVICES network 10.1.236.0/24
set firewall group network-group SLSERVICES network 10.1.56.0/24
set firewall group network-group SLSERVICES network 10.1.8.0/24
set firewall group network-group SLSERVICES network 10.1.224.0/24
set firewall group network-group SLSERVICES network 10.2.192.0/24
set firewall group network-group SLSERVICES network 10.1.16.0/24
set firewall group network-group SLSERVICES network 10.0.0.0/14
set firewall group network-group 1101PRIMARY network ###.###.###.### (プライマリー・プライベート・サブネット 1101/管理)
set firewall group network-group 1101MGMT network ###.###.###.### (ポータブル・プライベート・サブネット 1101/管理 VM)
set firewall group network-group 1102PRIMARY network ###.###.###.### (プライマリー・プライベート・サブネット 1102/ストレージ)
set firewall group network-group 1102STORAGEA network ###.###.###.### (ポータブル・プライベート・サブネット 1102/ストレージ・パス A)
set firewall group network-group 1102STORAGEB network ###.###.###.### (ポータブル・プライベート・サブネット 1102/ストレージ・パス B)
set firewall group network-group 1103VMACCESS network ###.###.###.### (ポータブル・プライベート・サブネット 1103/仮想マシン)
set firewall group address-group PERFORMANCEENDURANCE address '##.###.###.###' (パフォーマンス/エンデュランスのプライマリー IP)
set firewall group address-group PERFORMANCEENDURANCE address '##.###.###.###' (パフォーマンス/エンデュランスのセカンダリー IP)
commit
save
```

### ファイアウォール名ルールの構成

次は、トラフィックの方向ごとにファイアウォール・ルールを定義します。

構成モードで以下のコマンドを使用します。
```
set firewall name INSIDE2OUTSIDE
set firewall name INSIDE2OUTSIDE default-action drop
set firewall name INSIDE2OUTSIDE rule 10 action accept
set firewall name INSIDE2OUTSIDE rule 10 protocol all
set firewall name INSIDE2OUTSIDE rule 10 source group network-group 1101MGMT
set firewall name INSIDE2OUTSIDE rule 20 action accept
set firewall name INSIDE2OUTSIDE rule 20 protocol all
set firewall name INSIDE2OUTSIDE rule 20 source group network-group 1103VMACCESS
set firewall name OUTSIDE2INSIDE
set firewall name OUTSIDE2INSIDE default-action drop
set firewall name OUTSIDE2INSIDE rule 10 action accept
set firewall name OUTSIDE2INSIDE rule 10 protocol udp
set firewall name OUTSIDE2INSIDE rule 20 action accept
set firewall name OUTSIDE2INSIDE rule 20 protocol udp
set firewall name OUTSIDE2INSIDE rule 20 destination port 4500
set firewall name OUTSIDE2INSIDE rule 30 action accept
set firewall name OUTSIDE2INSIDE rule 30 protocol udp
set firewall name OUTSIDE2INSIDE rule 30 destination port 500
set firewall name OUTSIDE2INSIDE rule 40 action accept
set firewall name OUTSIDE2INSIDE rule 40 ipsec match-ipsec
set firewall name OUTSIDE2INSIDE rule 50 action accept
set firewall name OUTSIDE2INSIDE rule 50 protocol gre
set firewall name OUTSIDE2INSIDE rule 60 action accept
set firewall name OUTSIDE2INSIDE rule 60 protocol tcp
set firewall name OUTSIDE2INSIDE rule 60 destination port 1723
set firewall name OUTSIDE2INSIDE rule 70 action accept
set firewall name OUTSIDE2INSIDE rule 70 protocol tcp
set firewall name OUTSIDE2INSIDE rule 70 destination port 80
set firewall name OUTSIDE2INSIDE rule 80 action accept
set firewall name OUTSIDE2INSIDE rule 80 protocol tcp
set firewall name OUTSIDE2INSIDE rule 80 destination port 443
set firewall name OUTSIDE2INSIDE rule 90 action accept
set firewall name OUTSIDE2INSIDE rule 90 state established enable
set firewall name SLSERVICE2INSIDE
set firewall name SLSERVICE2INSIDE default-action drop
set firewall name SLSERVICE2INSIDE rule 10 action accept
set firewall name SLSERVICE2INSIDE rule 10 protocol all
set firewall name SLSERVICE2INSIDE rule 10 source group network-group SLSERVICES
set firewall name INSIDE2SLSERVICE
set firewall name INSIDE2SLSERVICE default-action drop
set firewall name INSIDE2SLSERVICE rule 10 action accept
set firewall name INSIDE2SLSERVICE rule 10 protocol all
set firewall name INSIDE2SLSERVICE rule 10 destination group network-group SLSERVICES
set firewall name VMACCESS2MGMT
set firewall name VMACCESS2MGMT default-action drop
set firewall name VMACCESS2MGMT rule 10 action drop
set firewall name VMACCESS2MGMT rule 10 protocol all
set firewall name VMACCESS2MGMT rule 10 source group network-group 1103VMACCESS
set firewall name STORAGE2MGMT
set firewall name STORAGE2MGMT default-action drop
set firewall name STORAGE2MGMT rule 10 action accept
set firewall name STORAGE2MGMT rule 10 protocol all
set firewall name STORAGE2MGMT rule 10 source group network-group 1102PRIMARY
set firewall name STORAGE2MGMT rule 20 action accept
set firewall name STORAGE2MGMT rule 20 protocol all
set firewall name STORAGE2MGMT rule 20 source group network-group 1102STORAGEA
set firewall name STORAGE2MGMT rule 30 action accept
set firewall name STORAGE2MGMT rule 30 protocol all
set firewall name STORAGE2MGMT rule 30 source group network-group 1102STORAGEB
set firewall name MGMT2STORAGE
set firewall name MGMT2STORAGE default-action drop
set firewall name MGMT2STORAGE rule 10 action accept
set firewall name MGMT2STORAGE rule 10 protocol all
set firewall name MGMT2STORAGE rule 10 source group network-group 1101PRIMARY
set firewall name MGMT2STORAGE rule 10 source group network-group 1101MGMT
set firewall name INSIDE2SLSERVICE rule 6 action 'accept'
set firewall name INSIDE2SLSERVICE rule 6 destination group address-group ' PERFORMANCEENDURANCE '
set firewall name INSIDE2SLSERVICE rule 6 protocol 'all'
set firewall name INSIDE2SLSERVICE rule 6 source group network-group '1102STORAGEA'
set firewall name INSIDE2SLSERVICE rule 7 action 'accept'
set firewall name INSIDE2SLSERVICE rule 7 destination group address-group ' PERFORMANCEENDURANCE '
set firewall name INSIDE2SLSERVICE rule 7 protocol 'all'
set firewall name INSIDE2SLSERVICE rule 7 source group network-group '1102STORAGEB'
set firewall name INSIDE2SLSERVICE rule 5 action 'accept'
set firewall name INSIDE2SLSERVICE rule 5 destination group address-group ' PERFORMANCEENDURANCE '
set firewall name INSIDE2SLSERVICE rule 5 protocol 'all'
set firewall name INSIDE2SLSERVICE rule 5 source group network-group '1102PRIMARY'
set firewall name SLSERVICE2INSIDE rule 6 action 'accept'
set firewall name SLSERVICE2INSIDE rule 6 source group address-group ' PERFORMANCEENDURANCE '
set firewall name SLSERVICE2INSIDE rule 6 protocol 'all'
set firewall name SLSERVICE2INSIDE rule 6 destination group network-group '1102STORAGEA'
set firewall name SLSERVICE2INSIDE rule 7 action 'accept'
set firewall name SLSERVICE2INSIDE rule 7 source group address-group ' PERFORMANCEENDURANCE '
set firewall name SLSERVICE2INSIDE rule 7 protocol 'all'
set firewall name SLSERVICE2INSIDE rule 7 destination group network-group '1102STORAGEB'
set firewall name SLSERVICE2INSIDE rule 5 action 'accept'
set firewall name SLSERVICE2INSIDE rule 5 source group address-group ' PERFORMANCEENDURANCE '
set firewall name SLSERVICE2INSIDE rule 5 protocol 'all'
set firewall name SLSERVICE2INSIDE rule 5 destination group network-group '1102PRIMARY'
set firewall name INSIDE2SLSERVICE rule 8 action 'drop'
set firewall name INSIDE2SLSERVICE rule 8 destination group address-group ' PERFORMANCEENDURANCE '
set firewall name INSIDE2SLSERVICE rule 8 protocol 'all'
set firewall name SLSERVICE2INSIDE rule 8 action 'drop'
set firewall name SLSERVICE2INSIDE rule 8 source group address-group ' PERFORMANCEENDURANCE '
set firewall name SLSERVICE2INSIDE rule 8 protocol 'all'
commit
save
```
### ゾーン・バインディングの構成

この手順では、Brocade vRouter (Vyatta) 上のインターフェースに特定のゾーンをバインドします。

構成モードで以下のコマンドを使用します。
```
set zone-policy zone OUTSIDE description “Internet Zone”
set zone-policy zone OUTSIDE default-action drop
set zone-policy zone OUTSIDE interface bond1
set zone-policy zone SLSERVICE description “SoftLayer Services”
set zone-policy zone SLSERVICE default-action drop
set zone-policy zone SLSERVICE interface bond0
set zone-policy zone MGMT description “Management VMs & ESX Host Access”
set zone-policy zone MGMT default-action drop
set zone-policy zone MGMT interface bond0.1101
set zone-policy zone STORAGE description “Storage”
set zone-policy zone STORAGE default-action drop
set zone-policy zone STORAGE interface bond0.1102
set zone-policy zone VMACCESS description “VM Access”
set zone-policy zone VMACCESS default-action drop
set zone-policy zone VMACCESS interface bond0.1103
commit
save
```

### ゾーンへのファイアウォール・ルールの適用

次は、ゾーン間の通信にファイアウォール・ルールを適用します。

構成モードで以下のコマンドを使用します。
```
set zone-policy zone OUTSIDE from MGMT firewall name INSIDE2OUTSIDE
set zone-policy zone OUTSIDE from VMACCESS firewall name INSIDE2OUTSIDE
set zone-policy zone VMACCESS from OUTSIDE firewall name OUTSIDE2INSIDE
set zone-policy zone VMACCESS from SLSERVICE firewall name SLSERVICE2INSIDE
set zone-policy zone MGMT from OUTSIDE firewall name OUTSIDE2INSIDE
set zone-policy zone MGMT from SLSERVICE firewall name SLSERVICE2INSIDE
set zone-policy zone SLSERVICE from MGMT firewall name INSIDE2SLSERVICE
set zone-policy zone SLSERVICE from VMACCESS firewall name INSIDE2SLSERVICE
set zone-policy zone SLSERVICE from STORAGE firewall name INSIDE2SLSERVICE
set zone-policy zone STORAGE from SLSERVICE firewall name SLSERVICE2INSIDE
set zone-policy zone STORAGE from MGMT firewall name MGMT2STORAGE
commit
save
```

### HA ペアの Brocade vRouter (Vyatta) との同期

HA ペアのうち片方の Brocade vRouter (Vyatta) をセットアップしたので、もう一方のゲートウェイ・デバイスに変更内容を同期する必要があります。

構成モードで以下のコマンドを使用します。
```
set system config-sync remote-router <他方の BROCADE VROUTER (VYATTA) の IP> password <他方の BROCADE VROUTER (VYATTA) のパスワード>
set system config-sync remote-router <他方の BROCADE VROUTER (VYATTA) の IP> sync-map 'SYNC'
set system config-sync remote-router <他方の BROCADE VROUTER (VYATTA) の IP> username <他方の BROCADE VROUTER (VYATTA) のユーザー名>
set system config-sync sync-map SYNC rule 1 action 'include'
set system config-sync sync-map SYNC rule 1 location 'nat'
set system config-sync sync-map SYNC rule 2 action 'include'
set system config-sync sync-map SYNC rule 2 location 'firewall'
set system config-sync sync-map SYNC rule 3 action 'include'
set system config-sync sync-map SYNC rule 3 location 'vpn'
set system config-sync sync-map SYNC rule 4 action 'include'
set system config-sync sync-map SYNC rule 4 location 'interfaces tunnel'
set system config-sync sync-map SYNC rule 5 action 'include'
set system config-sync sync-map SYNC rule 5 location 'firewall'
set system config-sync sync-map SYNC rule 6 action 'include'
set system config-sync sync-map SYNC rule 6 location 'zone-policy'
set system config-sync sync-map SYNC rule 7 action 'include'
set system config-sync sync-map SYNC rule 7 location 'vpn'
set system config-sync sync-map SYNC rule 8 action 'include'
set system config-sync sync-map SYNC rule 8 location 'protocols static route'
set system config-sync sync-map SYNC rule 9 action 'include'
set system config-sync sync-map SYNC rule 9 location 'protocols static table'
set system config-sync sync-map SYNC rule 10 action 'include'
set system config-sync sync-map SYNC rule 10 location 'policy route'
set system config-sync sync-map SYNC rule 11 action 'include'
set system config-sync sync-map SYNC rule 11 location 'nat'
commit
save
```
### VLAN の関連付けとルーティング

Brocade vRouter (Vyatta) でゾーンとファイアウォール・ルールをセットアップしたら、それに VLAN を関連付け、Brocade vRouter (Vyatta) を介する VLAN のルーティングを有効にする必要があります。

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}にログインして、**「ネットワーク」>「ゲートウェイ・アプライアンス」**をクリックし、Brocade vRouter (Vyatta) をクリックします。
2. **「VLAN」**を選択し、**「関連付け」**ボタンをクリックします。
3. ご使用の環境用に作成した VLAN ごとに、手順 2 を繰り返します。次に、Brocade vRouter (Vyatta) に関連付けるために、VLAN のルーティングを有効にする必要があります。
4. **「関連付けられた VLAN」**で VLAN を見つけ、それぞれの横にあるボックスにチェック・マークを付けます。
5. **「一括アクション」**ドロップダウン・メニューをクリックし、**「経路」**を選択します。
6. ポップアップ画面で**「OK」**をクリックします。

これで、VLAN が Brocade vRouter (Vyatta) 経由でルーティングされるようになりました。 2 つのゾーンの間で通信できないことに気付いた場合は、問題の VLAN をバイパスし、Brocade vRouter (Vyatta) 設定を確認してください。

これで、Brocade vRouter (Vyatta) で保護された単一サイト VMware 環境を、{{site.data.keyword.BluSoftlayer_full}} で使用できるようになりました。

 
