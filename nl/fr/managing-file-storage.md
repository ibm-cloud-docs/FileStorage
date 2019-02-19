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


# Gestion de {{site.data.keyword.filestorage_short}}
{: #managingstorage}

Vous pouvez gérer vos volumes {{site.data.keyword.filestorage_full}} via le portail {{site.data.keyword.slportal}}.

## Autorisation des hôtes pour l'accès à {{site.data.keyword.filestorage_short}}

Les hôtes "autorisés" sont des hôtes auxquels des droits d'accès à un volume spécifique ont été accordés. Sans autorisation d'hôte, vous ne pouvez pas accéder au stockage ni l'utiliser depuis votre système. L'autorisation d'un hôte pour l'accès à votre volume génère le nom d'utilisateur et le mot de passe.

Vous pouvez autoriser et connecter des hôtes qui se trouvent dans le même centre de données que votre stockage. Si vous pouvez disposer de plusieurs comptes, vous ne pouvez pas autoriser un hôte à partir d'un compte à accéder à votre stockage sur un autre compte.
{:important}

1. Cliquez sur **Stockage** > **{{site.data.keyword.filestorage_short}}**, puis sur **Nom de volume**.
2. Faites défiler l'écran jusqu'à la section **Hôtes autorisés** de la page.
3. Cliquez sur **Hôte autorisé** sur le côté droit. Sélectionnez les hôtes qui peuvent accéder à ce volume en particulier.

Vous pouvez également utiliser la commande suivante dans l'interface SLCLI.
```
# slcli file access-authorize --help
Usage: slcli file access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  -s, --subnet-id TEXT      The id of one SoftLayer_Network_Subnet to
                            authorize
  --help                    Show this message and exit.
```

## Affichage de la liste des hôtes autorisés à accéder à un volume {{site.data.keyword.filestorage_short}}

1. Cliquez sur **Stockage > {{site.data.keyword.filestorage_short}}**, puis sur **Nom du volume**.
2. Faites défiler la page jusqu'à la section **Hôtes autorisés**.

Vous y trouverez une liste des hôtes actuellement autorisés à accéder au volume.

Vous pouvez également utiliser la commande suivante dans l'interface SLCLI.
```
# slcli file access-list --help
Usage: slcli file access-list [OPTIONS] VOLUME_ID

Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: id, name, type,
                  private_ip_address, source_subnet, host_iqn, username,
                  password, allowed_host_id
  -h, --help      Show this message and exit.
```


## Affichage des volumes {{site.data.keyword.filestorage_short}} auxquels un hôte est autorisé à accéder

Vous pouvez afficher les volumes auxquels un hôte peut accéder, y compris les informations nécessaires pour l'établissement d'une connexion (Nom du volume, Type de stockage, Adresse cible, Capacité et Emplacement).

1. Cliquez sur **Unités** > **Liste des unités** dans le portail [{{site.data.keyword.slportal}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/){:new_window}
2. Cliquez sur l'unité appropriée.
2. Sélectionnez l'onglet Stockage.

La liste des volumes de stockage auxquels cet hôte particulier a accès s'affiche (les volumes sont regroupés par type de stockage : bloc, fichier, autre). Les menus **Action** respectifs vous permettent d'autoriser davantage de stockage ou de retirer l'accès.


## Montage et démontage de {{site.data.keyword.filestorage_short}}

Vous pouvez vous reporter aux informations relatives aux points de montage fournies dans la vue ** Détails du volume** pour monter {{site.data.keyword.filestorage_short}} à partir d'un hôte. Voir [Accès à {{site.data.keyword.filestorage_short}} sur Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux).


## Révocation de l'accès d'un hôte à {{site.data.keyword.filestorage_short}}

Si vous souhaitez ne plus autoriser un hôte à accéder à un volume de stockage en particulier, vous pouvez révoquer l'accès. Lorsque l'accès est révoqué, la connexion à l'hôte est retirée du volume. Le système d'exploitation et les applications ne peuvent plus communiquer avec le volume.

Pour empêcher tout problème côté hôte, démontez le volume de stockage de votre système d'exploitation avant de révoquer l'accès afin de prévenir tout incident lié à des unités manquantes ou à l'altération des données.
{:important}

Vous pouvez révoquer l'accès à partir de Stockage dans la Liste des unités ou dans les vues Stockage.

### Révocation de l'accès à partir de la liste des unités

1. Cliquez sur **Unités** > **Liste des unités** dans le portail [{{site.data.keyword.slportal}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/){:new_window}
2. Cliquez deux fois sur l'unité appropriée.
3. Sélectionnez l'onglet **Stockage**.
4. La liste des volumes de stockage auxquels cet hôte particulier a accès s'affiche (les volumes sont regroupés par type de stockage : bloc, fichier, autre). Sélectionnez le menu **Action** adéquat en regard du volume pour lequel vous souhaitez révoquer l'accès et cliquez sur **Révoquer le droit d'accès**.
5. Confirmez que vous souhaitez révoquer l'accès à un volume car cette action ne peut pas être annulée. Cliquez sur **Oui** pour révoquer l'accès au volume ou sur **Non** pour annuler l'action.

Si vous souhaitez déconnecter plusieurs volumes d'un hôte spécifique, vous devez répéter l'action Révoquer le droit d'accès pour chaque volume.
{:tip}


### Révocation de l'accès à partir de la vue Stockage

1. Cliquez sur **Stockage, {{site.data.keyword.filestorage_short}}** et sélectionnez le **Volume** pour lequel vous souhaitez révoquer l'accès.
2. Faites défiler l'écran jusqu'à la section **Hôtes autorisés** de la page.
3. Cliquez sur **Actions** en regard de l'hôte dont l'accès doit être révoqué et sélectionnez **Révoquer le droit d'accès**.
4. Confirmez que vous souhaitez révoquer l'accès à un volume car cette action ne peut pas être annulée. Cliquez sur **Oui** pour révoquer l'accès au volume ou sur **Non** pour annuler l'action.

Si vous souhaitez déconnecter plusieurs hôtes d'un volume spécifique, vous devez répéter l'action Révoquer le droit d'accès pour chaque hôte.
{:tip}

### Révocation de l'accès via l'interface SLCLI.
Vous pouvez également utiliser la commande suivante dans l'interface SLCLI.
```
# slcli file access-revoke --help
Usage: slcli file access-revoke [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to revoke
                            authorization
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to revoke
                            authorization
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to revoke authorization
  --ip-address TEXT         An IP address to revoke authorization
  -s, --subnet-id TEXT      The id of one SoftLayer_Network_Subnet to revoke
                            authorization
  --help                    Show this message and exit.
```

## Annulation d'un volume de stockage

Si vous n'avez plus besoin d'un volume spécifique, vous pouvez annuler ce stockage. Pour ce faire, vous devez d'abord révoquer l'accès à partir de tous les hôtes.

1. Cliquez sur **Stockage**>**{{site.data.keyword.filestorage_short}}**.
2. Cliquez sur **Actions** correspondant au volume à annuler et sélectionnez **Annuler {{site.data.keyword.filestorage_short}}**.
3. Confirmez l'annulation du volume de manière immédiate ou à la date anniversaire de la mise à disposition du volume.

   Si vous sélectionnez l'option d'annulation du volume à sa date anniversaire, vous pouvez annuler la demande d'annulation avant sa date anniversaire.
   {:tip}
4. Cliquez sur **Continuer** ou sur **Fermer**.
5. Cochez la case d'accusé de réception et cliquez sur **Confirmer**.

Vous pouvez également utiliser la commande suivante dans l'interface SLCLI.
```
# slcli file volume-cancel --help
Usage: slcli file volume-cancel [OPTIONS] VOLUME_ID

Options:
  --reason TEXT  An optional reason for cancellation
  --immediate    Cancels the file storage volume immediately instead of on the
                 billing anniversary
  -h, --help     Show this message and exit.
```
