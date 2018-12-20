---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configuration de {{site.data.keyword.filestorage_short}} en vue de la sauvegarde avec Plesk

Vous pouvez utiliser ces instructions afin de configurer {{site.data.keyword.filestorage_full}} pour vos sauvegardes dans Plesk. Cela suppose que vous disposiez d'un accès racine ou sudo SSH et d'un niveau d'administrateur Plesk complet. Cet exemple est basé sur un hôte CentOS7.

Pour plus d'informations, voir la [documentation relative à Plesk pour la sauvegarde et la restauration ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}.
{:tip}

1. Connectez-vous à l'hôte via SSH.
2. Vérifiez qu'il existe un point de montage cible. <br />

   Plesk dispose de deux options pour stocker les sauvegardes. L'une correspond au stockage Plesk interne (stockage situé sur votre serveur Plesk). L'autre correspond à un stockage FTP externe (stockage situé sur un serveur externe sur le Web ou sur votre réseau local). Dans Plesk, les sauvegardes internes sont généralement stockées dans `/var/lib/psa/dumps` et elles utilisent `/tmp` comme répertoire temporaire. Dans cet exemple, le répertoire temporaire est conservé en local, mais le répertoire `dumps` est déplacé vers le répertoire cible {{site.data.keyword.filestorage_short}} (`/backup/psa/dumps`). Aucune donnée d'identification d'utilisateur FTP n'est requise.
   {:note}
3. Configurez votre {{site.data.keyword.filestorage_short}} comme décrit dans [Accès à {{site.data.keyword.filestorage_short}} sur Red Hat Enterprise Linux](accessing-file-storage-linux.html) et [Montage de NFS/{{site.data.keyword.filestorage_short}} dans CentOS](mounting-nsf-file-storage.html) ou [Montage de NFS/{{site.data.keyword.filestorage_short}} sur CoreOS](mounting-storage-coreos.html). Montez le volume dans `/backup` et configurez-le dans la table du système de fichiers (`/etc/fstab`) afin d'activer le montage au moment du démarrage. <br />

   Par défaut, NFS rétromigre tous les fichiers qui ont été créés avec les droits root vers l'utilisateur nobody. Pour autoriser les clients root à conserver les droits root sur le partage NFS, vous devez ajouter `no_root_squash` dans `/etc/exports`.
   {:tip}
4. **Facultatif**. Copiez les sauvegardes existantes vers le nouveau stockage. Utilisez `rsync`, par exemple :
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {:pre}

   Cette commande compresse et transmet vos données tout en les conservant autant que possible (à l'exception des liens fixes). Elle fournit également des informations sur les fichiers transférés, ainsi qu'un bref résumé à la fin.
   {:tip}
5. Editez `/etc/psa/psa.conf` pour pointer vers la valeur `DUMP_D` sur la nouvelle cible.
    - Résultat : `DUMP_D /backup/psa/dumps`.
6. **Facultatif**. En fonction de votre cas d'utilisation et de vos besoins métier spécifiques, retirez l'ancien stockage du serveur et annulez-le dans le compte.
