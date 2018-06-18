---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---
{:new_window: target="_blank"}
{:pre: .pre}
 
# Configuration de {{site.data.keyword.filestorage_short}} en vue de la sauvegarde avec cPanel

Cet article fournit les instructions de configuration de vos sauvegardes en vue de leur stockage dans {{site.data.keyword.filestorage_full}} par cPanel. Cela suppose que vous disposiez d'un accès racine ou sudo SSH et WebHost Manager (WHM) complet. Cet exemple est basé sur un hôte **CentOS 7**.

**Remarque** : vous trouverez la documentation cPanel relative à la configuration du répertoire de sauvegarde [ici](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}.

1. Connectez-vous à l'hôte via SSH.

2. Vérifiez qu'il existe un point de montage cible. <br />
   **Remarque** : Par défaut, le système cPanel sauvegarde les fichiers de sauvegarde en local, dans le répertoire `/backup`. Dans le cadre de ce document, on suppose que le répertoire `/backup` existe déjà et qu'il contient des sauvegardes ; `/backup2` est donc utilisé comme nouveau point de montage. 
   
3. Configurez votre {{site.data.keyword.filestorage_short}} comme décrit dans [Accès à {{site.data.keyword.filestorage_short}} sur Red Hat Enterprise Linux](accessing-file-storage-linux.html) et [Montage NFS/{{site.data.keyword.filestorage_short}} dans CentOS](mounting-nsf-file-storage.html). Montez le volume dans `/backup2` et configurez-le dans la table du système de fichiers `/etc/fstab` afin d'activer le montage au moment de l'amorçage. <br />
   **Remarque** : Par défaut, NFS rétromigre tous les fichiers créés avec les droits root vers l'utilisateur nobody. Pour autoriser les clients root à conserver les droits root sur le partage NFS, vous devez ajouter `no_root_squash` dans `/etc/exports`. <br />
   **Remarque** : Vous trouverez également des instructions pour le [Montage NFS/{{site.data.keyword.filestorage_short}} sur CoreOS](mounting-storage-coreos.html). <br />

4. **Facultatif** : Copiez les sauvegardes existantes vers le nouveau stockage. Utilisez `rsync`, par exemple :
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    **Remarque** : cette commande transmet vos données compressées tout en les conservant autant que possible (à l'exception des liens fixes). Elle fournit également des informations sur les fichiers transférés, ainsi qu'un bref résumé à la fin. 
    
5. Connectez-vous à WebHost Manager et accédez à la configuration de la sauvegarde via **Home** > **Backup** > **Backup Configuration**.

6. Editez la configuration pour sauvegarder les sauvegardes dans le nouveau point de montage.  
    - Modifiez le répertoire de sauvegarde par défaut en saisissant le chemin d'accès absolu vers le nouvel emplacement à la place du répertoire `/backup/`.  
    - Sélectionnez **Enable to mount a backup drive**. Avec ce paramètre, le processus de configuration vérifie si un montage de sauvegarde (`/backup2`) existe dans le fichier `/etc/fstab`. <br /> **Remarque** : s'il existe un montage portant le même nom que le répertoire de transfert, le processus de configuration de sauvegarde monte l'unité et sauvegarde les informations dedans. Une fois le processus de sauvegarde terminé, il démonte l'unité. 

7. Appliquez les modifications en cliquant sur **Save Configuration** en bas de l'interface **Backup Configuration**.

8. **Facultatif** : En fonction de votre cas d'utilisation particulier et de vos besoins métier, retirez l'ancien stockage du serveur et annulez-le dans le compte.
