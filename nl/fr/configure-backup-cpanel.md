---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, cPanel, backups

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configuration de {{site.data.keyword.filestorage_short}} pour une sauvegarde avec cPanel
{: #cPanelBackups}

Vous pouvez utiliser ces instructions afin de configurer le stockage de vos sauvegardes dans {{site.data.keyword.filestorage_full}} par cPanel. Nous supposons ici que root ou sudo SSH et un accès complet à WebHost Manager (WHM) sont disponibles. Cet exemple est basé sur un hôte **CentOS 7**.

Pour plus d'informations, voir [cPanel - Configuration du répertoire de sauvegarde ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}.
{:tip}

1. Connectez-vous à l'hôte via SSH.
2. Vérifiez qu'il existe un point de montage cible. <br />

   Par défaut, le système cPanel enregistre les fichiers de sauvegarde localement, dans le répertoire `/backup`. Dans ce document, on suppose que le répertoire `/backup` existe déjà et qu'il contient des sauvegardes, et `/backup2` peut être utilisé comme nouveau point de montage.
   {:note}

3. Configurez votre {{site.data.keyword.filestorage_short}} comme décrit dans [Accès à {{site.data.keyword.filestorage_short}} sur Red Hat Enterprise Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux) et [Montage de {{site.data.keyword.filestorage_short}} dans CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)/[Montage de NFS/{{site.data.keyword.filestorage_short}} sur CoreOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS). Montez le volume dans `/backup2` et configurez-le dans la table du système de fichiers (`/etc/fstab`) afin d'activer le montage au moment du démarrage. <br />

   Par défaut, NFS rétromigre tous les fichiers qui ont été créés avec les droits root vers l'utilisateur nobody. Pour autoriser les clients root à conserver les droits root sur le partage NFS, vous devez ajouter `no_root_squash` dans `/etc/exports`.
   {:tip}

4. **Facultatif**. Copiez les sauvegardes existantes vers le nouveau stockage. Vous pouvez utiliser `rsync`, par exemple.
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

    Cette commande compresse et transmet vos données tout en les conservant autant que possible (à l'exception des liens fixes). Elle fournit également des informations sur les fichiers transférés via NFS, ainsi qu'un bref résumé à la fin.
   {:tip}

5. Connectez-vous à WebHost Manager et accédez à la configuration de la sauvegarde via **Home** > **Backup** > **Backup Configuration**.

6. Editez la configuration pour sauvegarder les sauvegardes dans le nouveau point de montage.
    - Modifiez le répertoire de sauvegarde par défaut en remplaçant le chemin d'accès absolu au répertoire `/backup/` par le nouvel emplacement.
    - Sélectionnez **Enable to mount a backup drive**. Avec ce paramètre, le processus de configuration vérifie si un montage de sauvegarde (`/backup2`) existe dans le fichier `/etc/fstab`. <br />

      S'il existe un montage portant le même nom que le répertoire de transfert, le processus de configuration de sauvegarde monte l'unité et sauvegarde les informations dedans. Une fois le processus de sauvegarde terminé, il démonte l'unité.
      {:note}
7. Appliquez les modifications en cliquant sur **Save Configuration**.
8. **Facultatif**. En fonction de votre cas d'utilisation et de vos besoins métier spécifiques, retirez l'ancien stockage du serveur et annulez-le dans le compte.
