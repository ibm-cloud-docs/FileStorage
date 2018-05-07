---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
 
# Configuration de {{site.data.keyword.filestorage_short}} en vue de la sauvegarde avec Plesk

Cet article fournit les instructions de configuration d'{{site.data.keyword.blockstoragefull}} dans le but d'effectuer les sauvegardes dans Plesk. Cela suppose que vous disposiez d'un accès racine ou sudo SSH et d'un niveau d'administrateur Plesk complet. Cet exemple est basé sur un hôte CentOS7.

**Remarque** : Vous trouverez la documentation Plesk relative à la sauvegarde et à la restauration [ici](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}.

1. Connectez-vous à l'hôte via SSH.

2. Vérifiez qu'il existe un point de montage cible. <br />
   **Remarque** : Plesk comporte deux options pour le stockage des sauvegardes : une pour le stockage Plesk interne (un stockage de sauvegarde situé sur votre serveur Plesk) et l'autre pour le stockage FTP externe (un stockage de sauvegarde situé sur un serveur externe sur le Web ou sur votre réseau local). Dans Plesk, les sauvegardes internes sont généralement stockées dans `/var/lib/psa/dumps` et elles utilisent `/tmp` comme répertoire temporaire. Dans notre exemple, le répertoire temporaire est conservé en local, mais le répertoire de vidage est déplacé vers la cible STaaS (`/backup/psa/dumps`). Aucune donnée d'identification d'utilisateur FTP n'est requise.
   
3. Configurez votre {{site.data.keyword.filestorage_short}} comme décrit dans [Accès à {{site.data.keyword.filestorage_short}} sur Red Hat Enterprise Linux](accessing-file-storage-linux.html) et [Montage NFS/{{site.data.keyword.filestorage_short}} dans CentOS](mounting-nsf-file-storage.html). Veillez à le monter correctement dans `/backup` et configurez-le dans `/etc/fstab` afin d'activer le montage à l'amorçage. <br />
   **Remarque** : Par défaut, NFS rétromigre tous les fichiers créés avec les droits root vers l'utilisateur nobody. Pour autoriser les clients root à conserver les droits root sur le partage NFS, ajoutez `no_root_squash` dans `/etc/exports`. <br />
   **Remarque** : Vous trouverez également des instructions pour le [Montage NFS/{{site.data.keyword.filestorage_short}} sur CoreOS](mounting-storage-coreos.html). <br />

4. **Facultatif** : Copiez les sauvegardes existantes vers le nouveau stockage. Utilisez `rsync`, par exemple :
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}
    
    **Remarque** : Cette commande transmet vos données compressées tout en les conservant autant que possible (à l'exception des liens fixes) et fournit des informations sur les fichiers transférés, ainsi qu'un bref résumé à la fin.
    
5. Editez `/etc/psa/psa.conf` pour pointer vers la valeur `DUMP_D` sur la nouvelle cible. 
    -  Résultat : `DUMP_D /backup/psa/dumps`. 

6. **Facultatif** : En fonction de votre cas d'utilisation particulier et de vos besoins métier, retirez l'ancien stockage du serveur et annulez-le dans le compte. 

