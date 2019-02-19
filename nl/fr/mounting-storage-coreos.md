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
{:codeblock: .codeblock}


# Montage de {{site.data.keyword.filestorage_short}} sur Container Linux
{: #mountingCoreOS}

Container Linux by CoreOS est un système d'exploitation open source léger basé sur le noyau Linux. Il est conçu de manière à fournir une infrastructure aux déploiements en cluster. En tant que système d'exploitation, Container Linux offre la fonctionnalité minimale requise pour le déploiement d'applications au sein de conteneurs de logiciels, ainsi qu'un mécanisme intégré de reconnaissance de service et de partage de configuration. Pour plus d'informations, voir [Montage de ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://coreos.com/os/docs/latest/mounting-storage.html)

## Montage de stockage portable

Tous les fichiers de montage secondaires sont placés dans le répertoire `/etc/systemd/system` car les montages au niveau du système sont effectués dans un répertoire en lecture seule. Vous devez d'abord créer un fichier `MOUNTPOINT.mount`. La section **Where** du fichier `.mount` doit correspondre au nom de fichier. Si le point de montage comporte des barres obliques (`/`), vous devez nommer le fichier en respectant la syntaxe `chemin-de-montage.mount`. Par exemple, si vous souhaitez monter l'unité de stockage portable dans `/mnt/www`, vous devez nommer le fichier `mnt-www.mount`.

Vous pouvez utiliser `fdisk` ou `parted` pour créer la partition. Assurez-vous que le système de fichiers que vous créez correspond à celui qui est indiqué dans le fichier `.mount`, sinon, le service ne pourra pas démarrer.


```
[Unit]
Description = Mount for Portable Storage

[Mount]
What=/dev/xvdc1
Where=/mnt/www
Type=ext4

[Install]
WantedBy = multi-user.target
```
{:codeblock}


Etant donné que ce système d'exploitation utilise `systemd`, vous devez activer le fichier `*.mount` pour que le point de montage soit préservé au redémarrage. Si vous utilisez l'indicateur `--now`, la partition est montée immédiatement et définie pour démarrer à l'amorçage.

```
systemctl enable --now mnt-www.mount
```
{:pre}

Pour plus d'informations, voir la [documentation `systemd mount`![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.freedesktop.org/software/systemd/man/systemd.mount.html)

## Montage de {{site.data.keyword.filestorage_short}}

Le processus de montage de {{site.data.keyword.filestorage_short}} est le même. Etant donné que le montage fait appel à NFS, vous pouvez indiquer davantage d'options à l'aide de la ligne `Options=` dans le fichier de montage.

Dans l'exemple, le système NFS est défini pour un montage dans `/data/www`. Le point de montage NFS de l'instance {{site.data.keyword.filestorage_short}} peut être obtenu sur la page de la liste de {{site.data.keyword.filestorage_short}} ou via un appel API `SoftLayer_Network_Storage::getNetworkMountAddress()`.
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
