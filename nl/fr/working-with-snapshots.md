---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Gestion des instantanés

## Comment créer un planning d'instantané ?

Vous pouvez décider de la fréquence et du moment de création d'un point de cohérence de référence dans votre volume de stockage en créant des plannings d'instantané. Vous disposez d'un maximum de 50 instantanés par volume de stockage. Les plannings sont gérés via l'onglet **Stockage** > **{{site.data.keyword.filestorage_short}}** du portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

Avant de pouvoir configurer votre planning initial, vous devez d'abord acheter de l'espace d'image instantanée si vous ne l'avez pas fait lors de la mise à disposition initiale du volume de stockage.

### Ajout d'un planning d'instantané

Vous pouvez configurer les plannings d'instantané à une fréquence horaire, quotidienne ou hebdomadaire, avec un cycle de conservation distinct. Vous disposez d'un maximum de 50 instantanés planifiés, avec différents plannings horaires, quotidiens et hebdomadaires, et instantanés manuels par volume de stockage.

1. Cliquez sur votre volume de stockage, cliquez dans la zone déroulante **Actions**, puis cliquez sur **Planning d'instantané**.
2. Dans la boîte de dialogue **Nouveau planning d'instantané**, trois fréquences d'instantané vous sont proposées. Vous pouvez utiliser une combinaison de ces trois fréquences pour créer un planning d'instantané complet.
   - Horaire
      - Indiquez la minute de chaque heure à laquelle un instantané doit être pris ; la minute en cours est indiquée par défaut.
      - Indiquez le nombre d'instantanés horaires à conserver avant la suppression du plus ancien d'entre eux. 
   - Quotidienne
      - Indiquez l'heure et la minute auxquelles un instantané doit être pris ; l'heure et la minute en cours sont indiquées par défaut.
      - Indiquez le nombre d'instantanés horaires à conserver avant la suppression du plus ancien d'entre eux. 
   - Hebdomadaire
      - Indiquez le jour de la semaine, l'heure et la minute de la prise d'un instantané ; le jour, l'heure et la minute en cours sont indiqués par défaut.
      - Sélectionnez le nombre d'instantanés hebdomadaires à conserver avant la suppression du plus ancien d'entre eux.
3. Cliquez sur **Enregistrer** et créez un autre planning avec une fréquence différente. </br> **Remarque** : si le nombre total d'instantanés planifiés est supérieur à 50, vous recevez un message d'avertissement et vous ne pouvez pas sauvegarder votre planning. 

La liste des instantanés s'affiche au fur et à mesure qu'ils sont pris dans la section **Instantanés** de la page **Détails**. 

## Comment prendre un instantané manuel ?

Il est possible de prendre des instantanés manuels à différents points d'une opération de mise à niveau ou de maintenance d'une application. Vous pouvez également prendre des instantanés sur plusieurs serveurs temporairement désactivés au niveau de l'application.

Vous disposez d'un maximum de 50 instantanés manuels par volume de stockage.

1. Cliquez sur votre volume de stockage.
2. Cliquez sur le menu déroulant Actions.
3. Sélectionnez **Prendre un instantané manuel**.

L'instantané est pris et s'affiche dans la section Instantanés de la page **Détails**. Son planning est indiqué comme Manuel.

## Comment afficher la liste des instantanés avec l'espace utilisé et les fonctions de gestion ?

La liste des instantanés conservés et de l'espace utilisé s'affiche sur la page **Détails** (**Stockage** > **{{site.data.keyword.filestorage_short}}**). Les fonctions de gestion (édition des plannings et ajout d'espace supplémentaire) figurent sur la page **Détail** dans le menu déroulant **Actions** ou les liens des différentes sections de la page.

## Comment afficher la liste des instantanés conservés ?

Les instantanés conservés dépendent du nombre que vous avez saisi dans la zone **Conserve les n derniers** lors de la configuration de vos plannings. Vous pouvez afficher les instantanés pris dans la section **Instantané**. Les instantanés sont indiqués par planning.

## Comment afficher la quantité d'espace d'image instantanée utilisée ?

Le graphique circulaire en haut de la page **Détails** indique la quantité d'espace utilisé et la quantité d'espace restant. Vous recevez des notifications lorsque vous atteignez les seuils d'espace suivants : 75 %, 90 % et 95 %.



## Comment modifier la quantité d'espace d'image instantanée pour mon volume ?

Il se peut que vous deviez ajouter de l'espace d'image instantanée à un volume qui n'en disposait pas auparavant ou qui en requiert davantage. Vous pouvez ajouter entre 5 et 4 000 Go, selon vos besoins. **Remarque** : il est uniquement possible d'accroître l'espace d'image instantanée, et pas de le réduire. Vous pouvez sélectionner une quantité peu élevée d'espace jusqu'à ce que vous déterminiez vos besoins réels. Gardez à l'esprit que les instantanés automatisés et manuels partagent le même espace.

Pour modifier l'espace d'image instantanée, accédez à **Stockage** > **{{site.data.keyword.filestorage_short}}**.

1. Cliquez sur vos volumes de stockage, puis sur le menu déroulant **Actions** et sélectionnez **Ajouter de l'espace instantané supplémentaire**.
2. Effectuez votre choix dans la plage de tailles présentée par l'invite. Les tailles vont généralement de 0 à la taille de votre volume.
3. Cliquez sur **Continuer** pour mettre à disposition l'espace supplémentaire.
4. Entrez un code promo le cas échéant et cliquez sur **Recalculer**. Des valeurs par défaut sont indiquées dans les zones **Prix pour cette commande** et **Vérification de la commande**.
5. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**. Votre espace d'image instantanée supplémentaire est mis à disposition en quelques minutes.

## Comment recevoir les notifications lorsque la limite d'espace d'image instantanée est presque atteinte et en cas de suppression d'instantanés ?

Des notifications sont envoyées via des tickets de demande de service par le support à l'utilisateur maître sur votre compte lorsque vous atteignez trois seuils d'espace distincts : 75 %, 90 % et 95 %.



- 75 % de la capacité : un avertissement indiquant que l'utilisation de l'espace d'image instantanée a dépassé 75 % de la capacité est envoyé. Si vous tenez compte de l'avertissement et que vous ajoutez de l'espace ou supprimez des images instantanés conservées et inutiles manuellement, l'action est notée et le ticket est fermé. Si vous ne faites rien, vous devez manuellement accuser réception du ticket, qui est ensuite fermé.
- 90 % de la capacité : un second avertissement indiquant que l'utilisation de l'espace d'image instantanée a dépassé 90 % de la capacité est envoyé. Comme lors du dépassement de 75 % de la capacité, si vous prenez les actions nécessaires pour réduire l'espace utilisé, l'action est notée et le ticket est fermé. Si vous ne faites rien, vous devez manuellement accuser réception du ticket, qui est ensuite fermé.
- 95 % de la capacité : un avertissement final est envoyé. Si vous ne faites rien pour ramener votre espace sous le seuil, une notification est générée et une suppression automatique est instaurée empêchant la création de futurs instantanés. Les instantanés planifiés sont supprimés, en commençant par le plus ancien, jusqu'à ce que l'utilisation soit ramenée au-dessous de 95 % ; cette suppression est effectuée à chaque fois que l'utilisation dépasse 95 % de la capacité jusqu'à ce qu'elle retombe sous le seuil. Si l'espace est augmenté manuellement ou que des instantanés sont supprimés, l'avertissement est réinitialisé et émis à nouveau en cas de nouveau dépassement du seuil. Si aucune action n'est effectuée, il s'agit du seul avertissement que l'utilisateur reçoit.

## Comment supprimer un planning d'instantané ?

Vous pouvez annuler des plannings d'instantané en accédant à **Stockage** > **{{site.data.keyword.filestorage_short}}**.

1. Dans le cadre **Plannings d'instantané** sur la page **Détails**, cliquez sur le planning à supprimer. 
2. Cochez la case située en regard du planning à supprimer, puis cliquez sur **Enregistrer**.<br/>
**Attention** : si vous utilisez la fonctionnalité de réplication, vérifiez que le planning que vous supprimez n'est pas celui qui est employé par la réplication. Cliquez [ici](replication.html) pour plus d'informations sur la suppression d'un planning de réplication.

## Comment supprimer un instantané ?

Il est possible de supprimer manuellement des instantanés inutiles afin de libérer de l'espace pour de futurs instantanés. Pour ce faire, accédez à **Stockage** > **{{site.data.keyword.filestorage_short}}**.

1. Cliquez sur le volume de stockage et faites défiler l'écran jusqu'à la section **Instantané** pour voir la liste des instantanés existants.
2. Cliquez dans la liste déroulante **Actions** en regard d'un instantané en particulier et sélectionnez **Supprimer** pour supprimer l'instantané. Cela n'affecte pas les instantanés futurs ou passés du même planning puisqu'il n'existe pas de dépendance entre les instantanés.

Les instantanés manuels qui ne sont pas supprimés comme indiqué précédemment sont automatiquement supprimés (la plus ancienne d'abord) lorsque vous atteignez les limites en termes d'espace.

## Comment restaurer mon volume de stockage à un point de cohérence spécifique à l'aide d'un instantané ?

Il se peut que vous ayez à ramener votre volume de stockage à un point de cohérence précis en raison d'une erreur d'utilisateur ou d'un endommagement des données.

1. Démontez et déconnectez le volume de stockage de l'hôte.
   - Cliquez [ici](accessing-file-storage-linux.html) pour obtenir les instructions liées à {{site.data.keyword.filestorage_short}} sur Linux.
2. Cliquez sur **Stockage**, **{{site.data.keyword.filestorage_short}}** dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
3. Faites défiler l'écran et cliquez sur le volume à restaurer. La section **Instantanés** de la page **Détails** affiche la liste de tous les instantanés enregistrés, ainsi que leur taille et leur date de création.
4. Cliquez sur **Actions** pour l'instantané à utiliser, puis sur **Restaurer**. 

   **Remarque** : Une restauration entraîne la perte des données créées ou modifiés depuis la prise de l'instantané utilisé. Une fois la restauration terminée, le volume de stockage revient à l'état dans lequel il se trouvait lors de la prise de l'instantané. Une invite apparaît et vous en informe.
5. Cliquez sur **Oui** pour lancer la restauration. Un message s'affiche en haut de la page indiquant que le volume a été restauré à l'aide de l'instantané sélectionné. De plus, une icône apparaît en regard du volume dans {{site.data.keyword.filestorage_short}} indiquant qu'une transaction active est en cours. Survolez cette icône pour ouvrir une boîte de dialogue précisant la transaction. L'icône disparaît lorsque la transaction est terminée. 
6. Montez et reconnectez le volume de stockage à l'hôte.
   - Cliquez [ici](accessing-file-storage-linux.html) pour obtenir les instructions liées à {{site.data.keyword.filestorage_short}} sur Linux.
   
**Remarque** : la restauration d'un volume entraîne la suppression de tous les instantanés antérieurs à l'instantané restauré.
