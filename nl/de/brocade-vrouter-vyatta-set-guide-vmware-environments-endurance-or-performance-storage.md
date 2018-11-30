---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Brocade vRouter (Vyatta) für VMware-Umgebungen mit {{site.data.keyword.filestorage_short}} einrichten

Sie können eine Brocade vRouter-Appliance (Vyatta-Appliance) für hohe Verfügbarkeit (HA) in einer VMware-Umgebung konfigurieren, die {{site.data.keyword.filestorage_full}} verwendet. Verwenden Sie folgende Informationen zusammen mit [Advanced Single-Site VMware Reference Architecture](https://{DomainName}/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window}, um eine dieser Speicheroptionen in Ihrer VMware-Umgebung einzurichten.

Das Brocade vRouter-Gateway (Vyatta-Gateway) dient als Gateway und Router für Ihre Umgebung und enthält Zonen, die aus Teilnetzen bestehen. Zwischen den Zonen werden Firewallregeln eingerichtet, sodass die Zonen miteinander kommunizieren können. Für Zonen, die nicht mit anderen Zonen zu kommunizieren brauchen, wird keine Firewallregel benötigt.

In der hier beschriebenen Beispielkonfiguration werden fünf Zonen im Brocade vRouter (Vyatta) erstellt:

- SLSERVICE – Services von {{site.data.keyword.BluSoftlayer_full}}
- VMACCESS – {{site.data.keyword.BluVirtServers_short}} (VMs) auf dem Kapazitätscluster
- MGMT – Management- und Kapazitätscluster und Management-VMs
- STORAGE – Speicherserver
- OUTSIDE – Öffentlicher Internetzugriff


Abbildung 1 beschreibt die Kommunikation zwischen den einzelnen Zonen. Ihre Umgebung sieht möglicherweise anders aus und erfordert andere Zonen und Firewallregeln.

![Abbildung 1 - Konfiguration der Brocade vRouter-Zonen (Vyatta)](/images/figure1_6.png)



## Brocade vRouter (Vyatta) konfigurieren

1. Stellen Sie eine SSH-Verbindung in die Appliance mit dem Rootkennwort her, das auf der Anzeige der Gerätedetails (Device Details) zu finden ist.
2. Geben Sie `Configure` ein, um in den Konfigurationsmodus zu wechseln, und führen Sie die Schritte in den nachfolgenden Abschnitten aus.

### Schnittstellen einrichten

Als Nächstes werden die Bond-Schnittstellen auf beiden Brocade vRoutern (Vyatta) mit den Teilnetzen in der Umgebung verbunden. Beachten Sie, dass Sie die {{site.data.keyword.BluSoftlayer_full}}-VLANs (1101, 1102 und 1103) durch die entsprechenden VLANs in Ihrer Umgebung ersetzen müssen. Außerdem müssen die Anweisungen, die in die Zeichen `<>` eingeschlossen sind, durch die Details Ihrer Umgebung (ohne `<>`) ersetzt werden.

Verwenden Sie die folgenden Befehle, um die Bond-Schnittstellen auf den Brocade vRoutern (Vyatta) zu konfigurieren. Sie müssen im Konfigurationsmodus arbeiten.

Brocade vRouter (Vyatta) 1
```
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ (Geben Sie eine IP-Adresse aus dem primären privaten Teilnetz, gebunden an VLAN 1101/Management, an.)
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ (Geben Sie eine IP-Adresse aus dem portierbaren privaten Teilnetz, gebunden an VLAN 1101/Management-VMs, an.)
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ (Geben Sie eine IP-Adresse aus dem portierbaren privaten Teilnetz, gebunden an VLAN 1102/Speicherpfad A, an.)
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ (Geben Sie eine IP-Adresse aus dem portierbaren privaten Teilnetz, gebunden an VLAN 1102/Speicherpfad B, an.)
set interfaces bonding bond0 vif 1103 address ‘##.###.###.###/##’ (Geben Sie eine IP-Adresse aus dem portierbaren privaten Teilnetz, gebunden an VLAN 1103/Virtuelle Maschinen, an.)
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 advertise-interval '1'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 preempt 'false'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 priority '253'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 virtual-address ‘<GATEWAY-ADRESSE/MASKE des primären privaten VLAN, gebunden an 1101/Management>’
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 virtual-address ‘<GATEWAY-ADRESSE/MASKE des portierbaren privaten VLAN, gebunden an 1101/Management-VMs>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 advertise-interval '1'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 preempt 'false'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 priority '253'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY-ADRESSE/MASKE des primären privaten VLAN, gebunden an 1102/Speicher>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY-ADRESSE/MASKE des portierbaren privaten VLAN, gebunden an 1102/Speicherpfad A>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY-ADRESSE/MASKE des portierbaren privaten VLAN, gebunden an 1102/Speicherpfad B>’
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 advertise-interval '1'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 preempt 'false'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 priority '253'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 virtual-address ‘<GATEWAY-ADRESSE/MASKE des primären privaten VLAN, gebunden an 1103/Virtuelle Maschinen>’
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 virtual-address ‘<GATEWAY ADDRESS/MASK des portierbaren privaten VLAN, gebunden an 1103/Virtuelle Maschinen>’
commit
save
```

Brocade vRouter (Vyatta) 2
```
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ (Geben Sie eine IP-Adresse aus dem primären privaten Teilnetz, gebunden an VLAN 1101/Management, an. Muss sich von der IP-Adresse unterscheiden, die dem Brocade vRouter (Vyatta) 1 zugeordnet ist.)
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ (Geben Sie eine IP-Adresse aus dem portierbaren privaten Teilnetz, gebunden an VLAN 1101/Management-VMs, an. Muss sich von der IP-Adresse unterscheiden, die dem Brocade vRouter (Vyatta) 1 zugeordnet ist.)
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ (Geben Sie eine IP-Adresse aus dem portierbaren privaten Teilnetz, gebunden an VLAN 1102/Speicherpfad A, an. Muss sich von der IP-Adresse unterscheiden, die dem Brocade vRouter (Vyatta) 1 zugeordnet ist.)
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ (Geben Sie eine IP-Adresse aus dem portierbaren privaten Teilnetz, gebunden an VLAN 1102/Speicherpfad B, an. Muss sich von der IP-Adresse unterscheiden, die dem Brocade vRouter (Vyatta) 1 zugeordnet ist.)
set interfaces bonding bond0 vif 1103 address ‘##.###.###.###/##’ (Geben Sie eine IP-Adresse aus dem portierbaren privaten Teilnetz, gebunden an VLAN 1103/Virtuelle Maschinen, an. Muss sich von der IP-Adresse unterscheiden, die dem Brocade vRouter (Vyatta) 1 zugeordnet ist.)
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 advertise-interval '1'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 preempt 'false'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 priority '253'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 virtual-address ‘<GATEWAY-ADRESSE/MASKE des primären privaten VLAN, gebunden an 1101/Management>’
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 virtual-address ‘<GATEWAY-ADRESSE/MASKE des portierbaren privaten VLAN, gebunden an 1101/Management-VMs>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 advertise-interval '1'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 preempt 'false'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 priority '253'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY-ADRESSE/MASKE des primären privaten VLAN, gebunden an 1102/Speicher>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY-ADRESSE/MASKE des portierbaren privaten VLAN, gebunden an 1102/Speicherpfad A>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY-ADRESSE/MASKE des portierbaren privaten VLAN, gebunden an 1102/Speicherpfad B>’
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 advertise-interval '1'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 preempt 'false'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 priority '253'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 virtual-address ‘<GATEWAY-ADRESSE/MASKE des primären privaten VLAN, gebunden an 1103/Virtuelle Maschinen>’
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 virtual-address ‘<GATEWAY ADDRESS/MASK des portierbaren privaten VLAN, gebunden an 1103/Virtuelle Maschinen>’
commit
save
```
### SNAT für externen Zugriff konfigurieren

In diesem Schritt wird SNAT (Source Network Address Translation) so konfiguriert, dass Management-VMs und VMs im Kapazitätscluster auf das Internet zugreifen können. Von diesem Schritt an muss die Konfiguration nur auf einem einzigen Brocade vRouter (Vyatta) durchgeführt werden, da die Konfiguration später synchronisiert wird.

Verwenden Sie die folgenden Befehle im Konfigurationsmodus:
```
set nat source rule 10
set nat source rule 10 source address ##.###.###.###/## (Portierbares privates Teilnetz, VLAN1101/Management-VMs)
set nat source rule 10 translation address ##.###.###.### (Brocade vRouter (Vyatta) bond1-IP-Adresse)
set nat source rule 10 outbound-interface bond1
set nat source rule 20
set nat source rule 20 source address ##.###.###.###/## (Portierbares privates Teilnetz, VLAN1103/Virtuelle Maschinen)
set nat source rule 20 translation address ##.###.###.### (Brocade vRouter (Vyatta) bond1-IP-Adresse)
set nat source rule 20 outbound-interface bond1
commit
save
```


### Firewallgruppen konfigurieren

Als Nächstes werden die Firewallgruppen konfiguriert, die bestimmten IP-Bereichen zugeordnet sind.

Verwenden Sie die folgenden Befehle im Konfigurationsmodus:
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
set firewall group network-group 1101PRIMARY network ###.###.###.### (Primäres privates Teilnetz, 1101/Management)
set firewall group network-group 1101MGMT network ###.###.###.### (Portierbares privates Teilnetz, 1101/Management-VMs)
set firewall group network-group 1102PRIMARY network ###.###.###.### (Primäres privates Teilnetz, 1102/Speicher)
set firewall group network-group 1102STORAGEA network ###.###.###.### (Portierbares privates Teilnetz, 1102/Speicherpfad A)
set firewall group network-group 1102STORAGEB network ###.###.###.### (Portierbares privates Teilnetz, 1102/Speicherpfad B)
set firewall group network-group 1103VMACCESS network ###.###.###.### (Portierbares privates Teilnetz, 1103/Virtuelle Maschinen)
set firewall group address-group PERFORMANCEENDURANCE address '##.###.###.###' (Primäre Performance/Endurance-IP-Adresse)
set firewall group address-group PERFORMANCEENDURANCE address '##.###.###.###' (Sekundäre Performance/Endurance-IP-Adresse)
commit
save
```

### Firewallnamensregeln konfigurieren

Definieren Sie nun die Firewallregeln für die einzelnen Datenverkehrsrichtungen. Verwenden Sie die folgenden Befehle im Konfigurationsmodus:

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

### Zonenbindungen konfigurieren

In diesem Schritt werden bestimmte Zonen an Schnittstellen auf dem Brocade vRouter (Vyatta) gebunden.

Verwenden Sie die folgenden Befehle im Konfigurationsmodus:
```
set zone-policy zone OUTSIDE description “Internetzone”
set zone-policy zone OUTSIDE default-action drop
set zone-policy zone OUTSIDE interface bond1
set zone-policy zone SLSERVICE description “SoftLayer-Services”
set zone-policy zone SLSERVICE default-action drop
set zone-policy zone SLSERVICE interface bond0
set zone-policy zone MGMT description “Management-VMs & ESX-Hostzugriff”
set zone-policy zone MGMT default-action drop
set zone-policy zone MGMT interface bond0.1101
set zone-policy zone STORAGE description “Speicher”
set zone-policy zone STORAGE default-action drop
set zone-policy zone STORAGE interface bond0.1102
set zone-policy zone VMACCESS description “VM-Zugriff”
set zone-policy zone VMACCESS default-action drop
set zone-policy zone VMACCESS interface bond0.1103
commit
save
```

### Firewallregeln auf Zonen anwenden

Wenden Sie nun die Firewallregeln auf die Kommunikation zwischen Zonen an. Verwenden Sie die folgenden Befehle im Konfigurationsmodus:

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

### Mit dem anderen Brocade vRouter (Vyatta) im Hochverfügbarkeitspaar synchronisieren

Jetzt ist einer der Brocade vRouter (Vyatta) in dem Hochverfügbarkeitspaar (HA-Paar) eingerichtet und Sie müssen die Änderungen mit der anderen Gateway-Einheit synchronisieren.

Verwenden Sie die folgenden Befehle im Konfigurationsmodus:
```
set system config-sync remote-router <IP-ADRESSE DES ANDEREN BROCADE VROUTER (VYATTA)> password <KENNWORT DES ANDEREN BROCADE VROUTER (VYATTA)>
set system config-sync remote-router <IP-ADRESSE DES ANDEREN BROCADE VROUTER (VYATTA)> sync-map 'SYNC'
set system config-sync remote-router <IP-ADRESSE DES ANDEREN BROCADE VROUTER (VYATTA)> username <BENUTZERNAME DES ANDEREN BROCADE VROUTER (VYATTA)>
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

### VLANs zuordnen und VLAN-Routing aktivieren

Wenn die Zonen und Firewallregeln auf dem Brocade vRouter (Vyatta) eingerichtet sind, müssen Sie ihm die VLANs zuordnen und das Routing der VLANs über den Brocade vRouter (Vyatta) aktivieren.

1. Melden Sie sich am [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window} an, klicken Sie auf **Netz > Gateway-Appliance** und klicken Sie auf Brocade vRouter (Vyatta).
2. Wählen Sie ein **VLAN** aus und klicken Sie auf **Zuordnen**.
4. Lokalisieren Sie die VLANs unter **Zugeordnete VLANs** und wählen Sie das Kontrollkästchen neben jedem VLAN aus.
5. Klicken Sie auf das Dropdown–Menü **Massenaktionen** und wählen Sie **Route** aus.
6. Klicken Sie auf **OK**.

Ihre VLANs werden jetzt per Routing über den Brocade vRouter (Vyatta) geleitet. Wenn Sie bemerken, dass die Kommunikation zwischen zwei Zonen unterbunden wird, umgehen Sie das bzw. die betreffenden VLANs und prüfen Ihre Einstellungen für den Brocade vRouter (Vyatta).
