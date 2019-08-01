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


# Abilitazione dei frame Jumbo in {{site.data.keyword.BluSoftlayer_notm}}
{: #jumboframes}

Un frame jumbo è un frame Ethernet con un payload più grande della MTU (maximum transmission unit) standard di 1.500 byte. I frame jumbo sono utilizzati sulle LAN (local area network) che supportano almeno 1 Gbps e possono arrivare a una dimensione di 9.000 byte.

I frame Jumbo devono essere configurati nello stesso modo nell'intero percorso di rete dal dispositivo di origine <-> switch <-> router <-> switch <-> dispositivo di destinazione. Se l'intera catena non è impostata nello stesso modo, viene per impostazione predefinita utilizzata l'impostazione più bassa lungo la catena. {{site.data.keyword.cloud}} ha i dispositivi di rete impostati su 9.000, attualmente. Tutti i dispositivi cliente devono essere impostati sullo stesso valore di 9.000.
{:important}


## Abilitazione di frame Jumbo in Linux

1. Modifica il file di configurazione di rete per l'interfaccia eth0.
   - Gli utenti CentOS, RHEL e Fedora Linux modificano `/etc/sysconfig/network-script/ifcfg-eth0`
     ```
     # vi /etc/sysconfig/network-script/ifcfg-eth0
     ```
     {: pre}

   - Gli utenti Debian e Ubuntu Linux modificano `/etc/network/interfaces`.

2. Accoda la seguente direttiva di configurazione, che specifica la dimensione del frame in byte.
   - CentOS, RHEL, Fedora Linux
     ```
     MTU 9000
     ```
     {: pre}

   - Debian e Ubuntu Linux
     ```
     MTU=9000
     ```
     {: pre}

3. Chiudi e salva il file. Riavvia l'interfaccia eth0.
   ```
   # /etc/init.d/networking restart
   ```
   {: pre}

   Questa azione causa una breve perdita della connettività di rete.
   {:important}
