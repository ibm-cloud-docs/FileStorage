---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-24"

keywords: File Storage, NSF, mounting File Storage, mounting storage on Linux,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Montage de {{site.data.keyword.filestorage_short}} sur Linux
{: #mountingLinux}

Commencez par vérifier que l'hôte qui doit accéder au volume {{site.data.keyword.filestorage_full}} est autorisé via la [console {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external}.

1. Dans la console, sélectionnez **Infrastructure classique**  > **Stockage** > **{{site.data.keyword.filestorage_short}}**
2. Faites défiler l'écran jusqu'au partage de fichiers à monter puis cliquez sur **...** (Actions). Sélectionnez ensuite **Hôte autorisé**.
3. Filtrez la liste des hôtes disponibles en sélectionnant le type de périphérique, le sous-réseau ou l'adresse IP.

   Lorsque la liste est filtrée par sous-réseau, les sous-réseaux affichés sont souscrits dans le même centre de données que le volume de stockage.
   {:note}
4. Sélectionnez un ou plusieurs hôtes dans la liste puis cliquez sur **Sauvegarder**.

Vous pouvez également autoriser les hôtes via l'interface SLCLI.
```
# slcli file access-authorize --help
Usage: slcli file access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of one hardware server to authorize.
  -v, --virtual-id TEXT     The ID of one virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of one IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  -s, --subnet-id TEXT      An ID of one subnet to authorize.
  --help                    Show this message and exit.
```

## Montage du partage {{site.data.keyword.filestorage_short}}

Utilisez les instructions décrites ci-après pour connecter une instance de calcul {{site.data.keyword.cloud}} basée sur Linux à un partage NFS (Network File System). L'exemple repose sur Red Hat Enterprise Linux 6. Il est possible d'adapter les étapes pour d'autres distributions Linux en fonction de la documentation du fournisseur du système d'exploitation.

Le point de montage de l'instance File Storage peut être obtenu sur la page de la liste de {{site.data.keyword.filestorage_short}} ou via un appel API : `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

1. Installez les outils requis.
   ```
   # yum -y install nfs-utils nfs-utils-lib
   ```
   {:pre}

2. Montez le partage distant.
   ```
   # mount -t "nfs version" -o "options" <mount_point> /mnt
   ```

   Exemple
   ```
   # mount -t nfs4 -o hard,intr
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt
   ```

3. Vérifiez que le montage a abouti.
   ```
   # df -h
   Filesystem Size Used Avail Use% Mounted on
   /dev/xvda2 25G 1.4G 22G 6% /
   tmpfs 1.9G 0 1.9G 0% /dev/shm
   /dev/xvda1 97M 51M 42M 55%
   ```

4. Accédez au point de montage et aux fichiers en lecture/écriture.
   ```
   # touch /mnt/test
   # ls -la /mnt
   total 12
   drwxr-xr-x 2 nobody nobody 4096 Sep 8 15:52 .
   dr-xr-xr-x. 22 root root 4096 Sep 8 14:30 ..
   -rw-r--r-- 1 nobody nobody 0 Sep 8 15:52 test
   ```

   Les fichiers créés par le superutilisateur ont pour propriété `nobody:nobody`. Pour afficher correctement la propriété, vous devez mettre à jour `idmapd.conf` avec les paramètres de domaine corrects. Voir la section [Implémentation de no_root_squash pour NFS](#norootsquash).
   {:tip}

5. Montez le partage distant au démarrage. Pour terminer la configuration, éditez la table des systèmes de fichiers (`/etc/fstab`) et ajoutez le partage distant dans la liste des entrées qui sont automatiquement montées au démarrage :

   ```
   (hostname):/(username) /mnt "nfs version" "options" 0 0
   ```

   Exemple

   ```
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs4 defaults,hard,intr 0 0
   ```

6. Vérifiez que le fichier de configuration ne comporte pas d'erreurs.

   ```
   # mount -fav
   ```
   {:pre}

   Si la commande s'exécute sans erreur, votre installation est terminée.

   Si vous utilisez NFS 4.1, ajoutez `sec=sys` à la commande mount pour prévenir tout problème lié à la propriété des fichiers.
   {:tip}

   Si votre système d'exploitation hôte est CentOS, vous pouvez configurer des options supplémentaires. Pour plus d'informations, voir [Montage de {{site.data.keyword.filestorage_short}} dans CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS).


## Implémentation de `no_root_squash` pour NFS (facultatif)
{: #norootsquash}

La configuration de `no_root_squash` permet aux clients root de conserver les droits root sur le partage NFS.
- Pour NFSv3, aucune action n'est nécessaire de la part des clients ; `no_root_squash` fonctionne.
- Pour NFSv4, vous devez affecter au domaine nfsv4 la valeur `slnfsv4.com` et démarrer `rpcidmapd` ou un service similaire en fonction de la version du système d'exploitation.

Exemple

1. A partir de l'hôte, définissez le paramètre du domaine dans `/etc/idmapd.conf`.

   ```
   #vi /etc/idmapd.conf
   [General]
   #Verbosity = 0
   #The following should be set to the local NFSv4 domain name
   #The default is the host's DNS domain name.
   Domain = slnfsv4.com
   [Mapping]
   Nobody-User = nobody
   Nobody-Group = nobody
   ```

2. Exécutez `nfsidmap -c`.
3. Démarrez `rpcidmapd`.
   ```
   # /etc/init.d/rpcidmapd start
   Starting RPC idmapd: [ OK ]
   ```
## Démontage du système de fichiers

Pour démonter un système de fichiers actuellement monté sur votre hôte, exécutez la commande `umount` avec le nom de disque ou le nom de point de montage.

```
umount /dev/sdb
```
{:pre}

```
umount /mnt
```
{:pre}
