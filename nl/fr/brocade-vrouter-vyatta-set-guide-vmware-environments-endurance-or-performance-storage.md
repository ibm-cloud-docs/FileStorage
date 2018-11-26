---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configuration de Brocade vRouter (Vyatta) pour les environnements VMware avec {{site.data.keyword.filestorage_short}}

Vous pouvez configurer un dispositif Brocade vRouter (Vyatta) pour la haute disponibilité dans un environnement VMware qui utilise {{site.data.keyword.filestorage_full}}. Utilisez les informations suivantes ainsi que celles de l'article [Advanced Single-Site VMware Reference Architecture](https://console.bluemix.net/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window} pour configurer l'une des options de stockage suivantes dans votre environnement VMware.

La passerelle Brocade vRouter (Vyatta) sert de passerelle et de routeur dans votre environnement et inclut des zones composées de sous-réseaux. Des règles de pare-feu sont mises en place entre les zones pour leur permettre de communiquer entre elles. Dans le cas de zones qui n'ont pas besoin de communiquer avec d'autres zones, aucune règle de pare-feu n'est nécessaire.

Dans notre exemple de configuration, cinq zones sont créées dans Brocade vRouter (Vyatta) :

- SLSERVICE – services {{site.data.keyword.BluSoftlayer_full}}
- VMACCESS – machines virtuelles {{site.data.keyword.BluVirtServers_short}} sur le cluster de capacité
- MGMT – clusters de gestion et de capacité, et machines virtuelles de gestion
- STORAGE – serveur(servers) de stockage
- OUTSIDE – accès internet public


La Figure 1 décrit la communication entre chaque zone. Votre environnement peut être différent et nécessiter d'autres zones et règles de pare-feu.

![Figure 1 - Configuration des zones Brocade vRouter (Vyatta)](/images/figure1_6.png)



## Configuration de Brocade vRouter (Vyatta)

1. Etablissez une liaison SSH avec le dispositif à l'aide du mot de passe root trouvé sur l'écran Détails de l'unité.
2. Entrez `configure` pour passer en mode de configuration et suivez les étapes indiquées ultérieurement.

### Configuration des interfaces

Ensuite, les interfaces de liaison sur les deux dispositifs Brocade vRouter (Vyatta) sont liées aux sous-réseaux de l'environnement. Gardez à l'esprit qu'il convient de remplacer les réseaux locaux virtuels {{site.data.keyword.BluSoftlayer_full}} (1101, 1102 et 1103) par les réseaux locaux virtuels de votre environnement. De plus, les instructions qui affichent `<>` doivent être remplacées par les détails de votre environnement (les marques `<>` doivent être retirées).

Utilisez les commandes suivantes pour configurer les interfaces de liaison sur les dispositifs Brocade vRouter (Vyatta). Vous devez être en mode de configuration.

Brocade vRouter (Vyatta) 1
```
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ (Enter an IP address from Primary Private Subnet Bound to VLAN 1101/Management)
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ (Enter an IP address from Portable Private Subnet Bound to VLAN 1101/Management VMs)
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ (Enter an IP address from Portable Private Subnet Bound to VLAN 1102/Storage Path A)
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ (Enter an IP address from Portable Private Subnet Bound to VLAN 1102/Storage Path B)
set interfaces bonding bond0 vif 1103 address ‘##.###.###.###/##’ (Enter an IP address from Portable Private Subnet Bound to VLAN 1103/Virtual Machines)
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 advertise-interval '1'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 preempt 'false'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 priority '253'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 virtual-address ‘<GATEWAY ADDRESS/MASK of Primary Private VLAN Bound to 1101/Management>’
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 virtual-address ‘<GATEWAY ADDRESS/MASK of Portable Private VLAN Bound to 1101/Management VMs>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 advertise-interval '1'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 preempt 'false'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 priority '253'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY ADDRESS/MASK of Primary Private VLAN Bound to 1102/Storage>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY ADDRESS/MASK of Portable Private VLAN Bound to 1102/Storage Path A>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY ADDRESS/MASK of Portable Private VLAN Bound to 1102/Storage Path B>’
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 advertise-interval '1'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 preempt 'false'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 priority '253'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 virtual-address ‘<GATEWAY ADDRESS/MASK of Primary Private VLAN Bound to 1103/Virtual Machines>’
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 virtual-address ‘<GATEWAY ADDRESS/MASK of Portable Private VLAN Bound to 1103/Virtual Machines>’
commit
save
```

Brocade vRouter (Vyatta) 2
```
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ (Enter an IP address from Primary Private Subnet Bound to VLAN 1101/Management. Must be different than the one assigned to Brocade vRouter (Vyatta)1)
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ (Enter an IP address from Portable Private Subnet Bound to VLAN 1101/Management VMs. Must be different than the one assigned to Brocade vRouter (Vyatta)1)
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ (Enter an IP address from Portable Private Subnet Bound to VLAN 1102/Storage Path A. Must be different than the one assigned to Brocade vRouter (Vyatta)1)
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ (Enter an IP address from Portable Private Subnet Bound to VLAN 1102/Storage Path B. Must be different than the one assigned to Brocade vRouter (Vyatta)1)
set interfaces bonding bond0 vif 1103 address ‘##.###.###.###/##’ (Enter an IP address from Portable Private Subnet Bound to VLAN 1103/Virtual Machines. Must be different than the one assigned to Brocade vRouter (Vyatta)1)
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 advertise-interval '1'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 preempt 'false'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 priority '253'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 virtual-address ‘<GATEWAY ADDRESS/MASK of Primary Private VLAN Bound to 1101/Management>’
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 virtual-address ‘<GATEWAY ADDRESS/MASK of Portable Private VLAN Bound to 1101/Management VMs>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 advertise-interval '1'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 preempt 'false'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 priority '253'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY ADDRESS/MASK of Primary Private VLAN Bound to 1102/Storage>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY ADDRESS/MASK of Portable Private VLAN Bound to 1102/Storage Path A>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY ADDRESS/MASK of Portable Private VLAN Bound to 1102/Storage Path B>’
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 advertise-interval '1'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 preempt 'false'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 priority '253'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 virtual-address ‘<GATEWAY ADDRESS/MASK of Primary Private VLAN Bound to 1103/Virtual Machines>’
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 virtual-address ‘<GATEWAY ADDRESS/MASK of Portable Private VLAN Bound to 1103/Virtual Machines>’
commit
save
```
### Configuration de SNAT pour l'accès externe

Cette étape consiste à configurer SNAT de telle sorte que les machines virtuelles de gestion et celles du cluster de capacité puissent accéder à internet. A partir de cette étape, la configuration ne doit être effectuée que sur un seul Brocade vRouter (Vyatta) car la configuration sera synchronisée ultérieurement.

Utilisez les commandes suivantes en mode de configuration :
```
set nat source rule 10
set nat source rule 10 source address ##.###.###.###/## (Portable Private Subnet VLAN1101/Management VMs)
set nat source rule 10 translation address ##.###.###.### (Brocade vRouter (Vyatta) bond1 IP)
set nat source rule 10 outbound-interface bond1
set nat source rule 20
set nat source rule 20 source address ##.###.###.###/## (Portable Private Subnet VLAN1103/Virtual Machines)
set nat source rule 20 translation address ##.###.###.### (Brocade vRouter (Vyatta) bond1 IP)
set nat source rule 20 outbound-interface bond1
commit
save
```


### Configuration des groupes de pare-feu

Vous allez maintenant configurer les groupes de pare-feu associés à certaines plages d'adresses IP.

Utilisez les commandes suivantes en mode de configuration :
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
set firewall group network-group 1101PRIMARY network ###.###.###.### (Primary Private Subnet 1101/Management)
set firewall group network-group 1101MGMT network ###.###.###.### (Portable Private Subnet 1101/Management VMs)
set firewall group network-group 1102PRIMARY network ###.###.###.### (Primary Private Subnet 1102/Storage)
set firewall group network-group 1102STORAGEA network ###.###.###.### (Portable Private Subnet 1102/Storage Path A)
set firewall group network-group 1102STORAGEB network ###.###.###.### (Portable Private Subnet 1102/Storage Path B)
set firewall group network-group 1103VMACCESS network ###.###.###.### (Portable Private Subnet 1103/Virtual Machines)
set firewall group address-group PERFORMANCEENDURANCE address '##.###.###.###' (Performance/Endurance Primary IP)
set firewall group address-group PERFORMANCEENDURANCE address '##.###.###.###' (Performance/Endurance Secondary IP)
commit
save
```

### Configuration des règles de nom de pare-feu

Vous allez maintenant définir les règles de pare-feu pour chaque direction du trafic. Utilisez les commandes suivantes en mode de configuration :

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

### Configuration des liaisons de zone

Cette étape consiste à lier des zones particulières à des interfaces sur le dispositif Brocade vRouter (Vyatta).

Utilisez les commandes suivantes en mode de configuration :
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

### Application des règles de pare-feu aux zones

Appliquez les règles de pare-feu à la communication entre les zones. Utilisez les commandes suivantes en mode de configuration :

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

### Synchronisation avec l'autre dispositif Brocade vRouter (Vyatta) dans la paire haute disponibilité

L'un des dispositifs Brocade vRouter (Vyatta) de la paire haute disponibilité étant désormais configuré, vous devez synchroniser les modifications sur l'autre périphérique de passerelle.

Utilisez les commandes suivantes en mode de configuration :
```
set system config-sync remote-router <OTHER BROCADE VROUTER (VYATTA) IP> password <OTHER BROCADE VROUTER (VYATTA) PASSWORD>
set system config-sync remote-router <OTHER BROCADE VROUTER (VYATTA) IP> sync-map 'SYNC'
set system config-sync remote-router <OTHER BROCADE VROUTER (VYATTA) IP> username <OTHER BROCADE VROUTER (VYATTA) USERNAME>
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

### Association et routage des réseaux locaux virtuels

Une fois que les zones et les règle de pare-feu sont configurées sur le dispositif Brocade vRouter (Vyatta), vous devez associer à ce dernier les réseaux locaux virtuels et activer le routage de ces derniers via Brocade vRouter (Vyatta).

1. Connectez-vous au portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} et cliquez sur **Réseau > Dispositif de passerelle**, puis sur Brocade vRouter (Vyatta).
2. Sélectionnez un **réseau local virtuel** et cliquez sur le bouton **Associer**.
4. Localisez les réseaux locaux virtuels sous **VLAN associés** et cochez la case située en regard de chacun d'entre eux.
5. Cliquez sur le menu déroulant **Actions groupées** et sélectionnez **Router**.
6. Cliquez sur **OK**.

Vos réseaux locaux virtuels sont routés via le dispositif Brocade vRouter (Vyatta). Si vous remarquez une gêne dans la communication entre deux zones, ignorez le ou les réseaux locaux virtuels en question et vérifiez vos paramètres Brocade vRouter (Vyatta).
