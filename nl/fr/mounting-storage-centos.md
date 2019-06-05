---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-22"

keywords: File Storage, mounting file storage, Linux, CentOS, NFS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}


# Montage de {{site.data.keyword.filestorage_short}} dans CentOS
{: #mountingCentOS}

Pour monter {{site.data.keyword.filestorage_full}} dans CentOS 7, vous devez d'abord autoriser l'hôte via le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} ou via l'interface de ligne de commande SL. Installez ensuite les utilitaires NFS comme indiqué dans la section [Montage de {{site.data.keyword.filestorage_short}} sur Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux).

Pour CentOS, vous pouvez spécifier des options supplémentaires à l'aide de la ligne `Options=` dans le fichier de montage. Dans l'exemple suivant, le système NFS est défini pour un montage dans `/data/www`.

Le point de montage NFS de l'instance {{site.data.keyword.filestorage_short}} peut être obtenu sur la page de la liste de {{site.data.keyword.filestorage_short}} ou via un appel API `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

```
$ cat data-www.mount
[Unit]
Description = Mount for Container Storage

[Mount]
What=<nfs_mount_point>
Where=/data/www
Type=nfs
Options=vers=4,sec=sys,noauto

[Install]
WantedBy = multi-user.target
```
{:codeblock}

A présent, activez le montage et vérifiez que celui-ci est correct.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}
