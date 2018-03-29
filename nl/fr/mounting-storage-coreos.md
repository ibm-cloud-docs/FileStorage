---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Montage de {{site.data.keyword.filestorage_short}} sur CoreOS

CoreOS est une distribution Linux puissante conçue pour simplifier la gestion de déploiements importants et évolutifs sur des infrastructures variées. Basé sur une génération de Chrome OS, CoreOS gère un système hôte léger et utilise des conteneurs Docker pour toutes les applications.

## Montage de stockage portable

Tous les fichiers de montage secondaires sont placés dans le répertoire */etc/systemd/system* car les montages au niveau du système sont effectués dans un répertoire en lecture seule dans CoreOS. Vous allez créer un fichier MOUNTPOINT.mount. La section Where du fichier .mount doit correspondre au nom de fichier. Si le point de montage comporte des barres obliques (/), vous devez nommer le fichier en respectant la syntaxe suivante : chemin-de-montage.mount. Comme vous pouvez le voir dans l'exemple suivant, l'unité de stockage portable doit être montée dans `/mnt/www`, donc le fichier est nommé `mnt-www.mount`.

Vous devez utiliser fdisk ou parted pour créer la partition et vérifier que le système de fichiers que vous créez correspond à celui qui est indiqué dans le fichier `.mount` ; dans le cas contraire, le démarrage du service échoue. 


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

CoreOS utilise systemd ; par conséquent, vous devez activer le fichier  `*.mount` pour que le point de montage survive au réamorçage. Si vous utilisez l'indicateur `--now`, la partition est montée immédiatement et est définie pour démarrer à l'amorçage. 

`$ systemctl enable --now mnt-www.mount`

## Montage NFS/{{site.data.keyword.filestorage_short}}

Le processus de montage d'un stockage {{site.data.keyword.filestorage_short}} de type Endurance/Performance est similaire, mais étant donné que le montage fait appel à NFS, il est possible d'indiquer certaines options supplémentaires à l'aide de la ligne Options= dans le fichier de montage. Dans l'exemple suivant, le système NFS est défini pour un montage dans `/data/www`. Notez que le point de montage NFS de l'instance {{site.data.keyword.filestorage_short}} peut être obtenu dans la page de la liste des stockages {{site.data.keyword.filestorage_short}} ou via un appel à l'API -SoftLayer_Network_Storage::getNetworkMountAddress().

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

Vous pouvez maintenant activer le montage et vérifier qu'il est monté correctement. 

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
 
## Montage NAS/Cifs

Le montage d'un partage cifs n'est pas pris en charge nativement dans CoreOS, mais une solution de contournement très simple permet d'autoriser le système hôte à monter des partages NAS. Vous allez utiliser un conteneur pour créer le module mount.cfis, puis le copier sur le système CoreOS.
 
Sur le système CoreOS, exécutez la commande suivante pour télécharger et accéder à un conteneur Fedora :   <br/>
`docker run -t -i -v /tmp:/host_tmp fedora /bin/bash`
 
Une fois dans le conteneur, exécutez la commande suivante pour créer l'utilitaire cifs :
```
dnf groupinstall -y "Development Tools" "Development Libraries"
dnf install -y tar
dnf install -y bzip2
curl https://ftp.samba.org/pub/linux-cifs/cifs-utils/cifs-utils-6.4.tar.bz2 | bunzip2 -c - | tar -xvf -
cd cifs-utils-6.4/
./configure && make
cp mount.cifs /host_tmp/
```
 
Notez que lorsque le fichier mount.cifs est copié sur la machine hôte, vous pouvez quitter le conteneur Docker à l'aide de la commande `exit` ou des touches **ctrl+d**. Une fois revenu dans le système CoreOS, vous pouvez monter le partage CIFS avec la commande suivante :  <br/>
`/tmp/mount.cifs //nasXXX.service.softlayer.com/USERNAME -o username=USERNAME,password=PASSWORD /path/to/mount`
 
## Montage d'ISCSI

Ce type de montage n'est actuellement pas pris en charge dans CoreOS, mais il le sera dans une version ultérieure : https://github.com/coreos/bugs/issues/634.
