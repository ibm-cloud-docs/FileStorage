---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Accès à {{site.data.keyword.filestorage_short}} sur Linux

Avant de commencer, vérifiez que l'hôte accédant au volume {{site.data.keyword.filestorage_full}} a été autorisé via le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} :

1. Dans la page des listes de {{site.data.keyword.filestorage_short}}, cliquez sur l'option **Actions** associée au partage nouvellement mis à disposition, puis cliquez sur **Hôte autorisé**.
2. Sélectionnez le ou les hôtes souhaités dans la liste et cliquez sur **Soumettre** pour autoriser les hôtes à accéder au partage. 

## Montage du partage {{site.data.keyword.filestorage_short}}

Voici les étapes permettant de connecter une instance de traitement {{site.data.keyword.BluSoftlayer_full}} basée sur Linux à un partage NFS. L'exemple repose sur Red Hat Enterprise Linux 6. Il est possible d'adapter les étapes pour d'autres distributions Linux en fonction de la documentation du fournisseur du système d'exploitation.

**Remarque :** Le point de montage de l'instance de stockage de fichiers peut être obtenu sur la page des listes de {{site.data.keyword.filestorage_short}} ou via un appel de l'API SoftLayer_Network_Storage::getNetworkMountAddress().

1. Installez les packages/outils requis.

    `[root@TEST-RHEL6]# yum -y install nfs-utils nfs-utils-lib
    `
2. Montez le partage distant.
    `[root@TEST-RHEL6]# mount -t "nfs version" -o "options" <mount_point> /mnt`
    
    Voici un exemple de montage du partage distant sur une instance de stockage.
    
    `[root@TEST-RHEL6 mnt]# mount -t nfs4 -o hard,intr`
    
    `nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt`
 
3. Vérifiez que le montage a abouti.

    `[root@TEST-RHEL6]# df -h`
    
    `Filesystem Size Used Avail Use% Mounted on`
    
    `/dev/xvda2 25G 1.4G 22G 6% /`
    
    `tmpfs 1.9G 0 1.9G 0% /dev/shm`
    
    `/dev/xvda1 97M 51M 42M 55%`
    
4. Accédez au point de montage et au fichiers en lecture/écriture. 

    `[root@TEST-RHEL6]# touch /mnt/test`
    
    `[root@TEST-RHEL6]# ls -la /mnt`
    
    `total 12`
    
    `drwxr-xr-x 2 nobody nobody 4096 Sep 8 15:52 .`
    
    `dr-xr-xr-x. 22 root root 4096 Sep 8 14:30 ..`
    
    `-rw-r--r-- 1 nobody nobody 0 Sep 8 15:52 test`

    **Remarque :** Les fichiers créés par le superutilisateur ont pour propriété nobody:nobody. Si vous souhaitez afficher correctement la propriété, vous devez mettre à jour idmapd.conf avec les paramètres de domaine corrects. Voir "Implémentation de no_root_squash pour NFS" ci-dessous.
    
5. Montez le partage distant à l'amorçage. Pour terminer la configuration, éditez la table des systèmes de fichiers /etc/fstab et ajoutez le partage distant dans la liste des entrées automatiquement montées au démarrage : 

    `(hostname):/(username) /mnt "nfs version" "options" 0 0`
    
    Si vous utilisez l'instance de l'exemple de montage du partage distant, l'entrée est la suivante : 
    
    `nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs4 defaults,hard,intr 0 0`
    
6.  Vérifiez l'absence d'erreurs dans le fichier de configuration. 

    `[root@TEST-RHEL6 /]# mount -fav`
    
    Si la commande s'exécute sans erreur, votre installation est terminée.

**Remarque :** Si vous utilisez NFS 4.1, ajoutez 'sec=sys' à la commande mount pour prévenir tout problème lié à la propriété des fichiers. 

 
## Implémentation de no_root_squash pour NFS (facultatif)

La configuration de no_root_squash permet aux clients root de conserver les droits root sur le partage NFS. Pour NFSv3, aucune action n'est nécessaire de la part des clients ; no_root_squash devrait fonctionner.
Pour NFSv4, vous devez définir le domaine nfsv4 sur slnfsv4.com et démarrer rpcidmapd ou un service similaire en fonction de la version du système d'exploitation. 

Voici un exemple :

1. A partir de l'hôte, définissez le paramètre du domaine dans /etc/idmapd.conf.

    `#vi /etc/idmapd.conf`
    
    `[General]`
    
    `#Verbosity = 0`
    
    `#The following should be set to the local NFSv4 domain name`
    
    `#The default is the host's DNS domain name.`
    
    `Domain = slnfsv4.com`
    
    `[Mapping]`
    
    `Nobody-User = nobody`
    
    `Nobody-Group = nobody`
    
2. Exécutez nfsidmap -c.
3. Démarrez rpcidmapd.

   ` # /etc/init.d/rpcidmapd start`
   
   ` Starting RPC idmapd: [ OK ]`
