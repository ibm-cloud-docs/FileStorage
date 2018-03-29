---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-11"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Utilisation d'instantanés

Les instantanés sont une fonction d'{{site.data.keyword.filestorage_full}}. Un instantané représente le contenu d'un volume à un point précis dans le temps. Les instantanés vous permettent de protéger vos données sans impact sur les performances et avec une consommation minimale de l'espace. Ils sont considérés comme votre première ligne de défense dans le cadre de la protection des données. La fonction d'instantané permet de restaurer facilement et rapidement les données à partir d'une copie d'image instantanée si un utilisateur modifie ou supprime accidentellement des données essentielles d'un volume.

{{site.data.keyword.filestorage_short}} propose deux manières de prendre vos instantanés. La première méthode consiste à utiliser un planning d'instantané configurable qui crée et supprime automatiquement les copies d'image instantanée de chaque volume de stockage. Vous pouvez également créer des plannings d'instantané supplémentaires, supprimer des copies manuellement et gérer les plannings en fonction de vos besoins. La seconde méthode consiste à prendre un instantané manuel. 

Une copie d'image instantanée est une image en lecture seule d'un volume {{site.data.keyword.filestorage_short}} qui capture l'état du volume à un point dans le temps. Les copies d'image instantanée sont extrêmement efficaces en termes de durée de création et d'espace de stockage. La création d'une copie d'image instantanée {{site.data.keyword.filestorage_short}} ne prend que quelques seconde, généralement moins d'une seconde, quel que soit la taille du volume ou le niveau d'activité sur le stockage. Une fois la copie d'image instantanée créée, les modifications apportées aux objets sont reflétées dans les mises à jour de la version en cours des objets, comme s'il n'existait pas de copies d'image instantanée. Pendant ce temps, la copie des données reste complètement stable. 

Une copie d'image instantanée ne génère pas de surcharge des performances ; les utilisateurs peuvent facilement stocker jusqu'à 50 instantanés planifiés et 50 instantanés manuels par volume {{site.data.keyword.blockstorageshort}}, tous accessibles en lecture seule et sous forme de versions en ligne des données. 

Les instantanés permettent aux utilisateurs

- de créer des points de récupération à un point de cohérence sans interruption, 
- de rétablir des volumes à des points de cohérence.
- Vous devez acheter une certaine quantité d'espace d'image instantanée pour votre volume pour pouvoir en prendre des instantanés. Il est possible d'ajouter de l'espace d'image instantanée lors de la commande initiale du volume ou après sa mise à disposition initiale via la page Détails du volume, en cliquant sur le bouton du menu déroulant Actions et en sélectionnant Ajouter de l'espace d'instantané. Gardez à l'esprit que les instantanés planifiés et manuels partagent l'espace d'image instantanée ; veillez à commander l'espace en conséquence. 

## Meilleures pratiques concernant les instantanés
La conception des instantanés dépend de l'environnement du client. Prenez en compte les éléments suivants pour planifier et implémenter les copies d'image instantanée :  
- 	Il est possible de créer jusqu'à 50 instantanés via un planning et jusqu'à 50 instantanés manuels sur chaque volume ou numéro d'unité logique. 
- 	Ne prenez pas trop d'instantanés. Veillez à ce que la fréquence des instantanés planifiés corresponde à vos besoins en termes d'objectif de temps de reprise et d'objectif de point de reprise, ainsi qu'à vos exigences professionnelles liées aux applications et planifiez des instantanés à un rythme horaire, quotidien ou hebdomadaire.  
- 	La fonction de suppression automatique des instantanés permet de contrôler la croissance de la consommation d'espace de stockage.  <br/>
    **Remarque** : Le seuil de suppression automatique est fixé à 95 %.
    
## Comment les copies d'image instantanée consomment-elles l'espace disque ? 
Les copies d'image instantanée minimisent la consommation du disque en conservant des blocs individuels plutôt que des fichiers entiers. Elles commencent à consommer de l'espace supplémentaire uniquement en cas de modification ou de suppression des fichiers dans le système de fichiers actif. Dans ce cas, les blocs de fichier d'origine sont toujours conservés dans une ou plusieurs copies d'image instantanée.
Dans le système de fichiers actif, les blocs modifiés sont réécrits à des emplacements différents sur le disque ou retirés sous la forme de blocs de fichier complets. Ainsi, outre l'espace disque employé par les blocs dans le système de fichiers actif modifié, l'espace disque utilisé par les blocs d'origine est toujours conservé pour refléter le statut du système de fichiers actif avant la modification. 

<table>
    <colgroup>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
    </colgroup>
    <tbody>
      <tr>
        <th colspan="3" style="border: 0.0px;text-align: center;">Utilisation de l'espace disque avant et après une copie d'image instantanée</th>
     </tr><tr>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle1.png" alt="Avant une copie d'image instantanée"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle3.png" alt="Après une copie d'image instantanée"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle2.png" alt="Modifications après une copie d'image instantanée"></td>
     </tr><tr>
        <td style="border: 0.0px;">Avant la création d'une copie d'image instantanée, l'espace disque est consommé uniquement par le système de fichiers actif.</td>
        <td style="border: 0.0px;">Après la création d'une copie d'image instantanée, le système de fichiers actif et la copie d'image instantanée pointent vers les mêmes blocs disque. La copie d'image instantanée n'utilise pas d'espace disque supplémentaire. </td>
        <td style="border: 0.0px;">Après la suppression de <i>myfile.txt</i> du système de fichiers actif, la copie d'image instantanée inclut toujours le fichier et fait référence à ses blocs disque. C'est pourquoi, la suppression de données appartenant au système de fichiers actif ne libère pas toujours d'espace disque. </td>
      </tr>
    </tbody>
</table>



## Comment acheter de l'espace d'image instantanée ? 

Pour créer des instantanés de votre volume de stockage, que ce soit de manière automatisée ou manuelle, vous devez acheter de l'espace afin de les conserver. Vous pouvez acquérir une capacité maximale égale à la quantité de votre volume de stockage (lors de l'achat initial du volume ou ultérieurement en suivant les étapes ci-après). 

1. Accédez à votre stockage via l'onglet **Stockage** > **{{site.data.keyword.filestorage_short}}** du portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Cliquez sur le lien Ajouter de l'espace d'instantané dans le cadre Instantanés. Cliquez sur le lien **Achetez maintenant de l'espace pour les instantanés** dans le cadre Instantanés.
3. Sélectionnez la quantité d'espace dont vous avez besoin en cliquant sur le bouton radio en regard de la quantité souhaitée. 
4. Cliquez sur **Continuer**.
5. Entrez un code promo le cas échéant et cliquez sur **Recalculer**. Des valeurs par défaut sont indiquées dans les zones Prix pour cette commande et Vérification de la commande. 
6. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**. Votre espace d'image instantanée est mis à disposition en quelques minutes.

## Comment créer un planning d'instantané ?

Les plannings d'instantané vous permettent de décider de la fréquence et du moment de création d'un point de cohérence de référence dans votre volume de stockage. Vous disposez d'un maximum de 50 instantanés par volume de stockage. Les plannings sont gérés via l'onglet **Stockage** > **{{site.data.keyword.filestorage_short}}** du portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

Pour pouvoir configurer votre planning initial, vous devez d'abord acheter de l'espace d'image instantanée si vous ne l'avez pas fait lors de la mise à disposition initiale du volume de stockage.

### Ajout d'un planning d'instantané

Vous pouvez configurer les plannings d'instantané à une fréquence horaire, quotidienne ou hebdomadaire, avec un cycle de conservation distinct. Vous disposez d'un maximum de 50 instantanés planifiés, avec différents plannings horaires, quotidiens et hebdomadaires, et instantanés manuels par volume de stockage. 

1. Cliquez sur votre volume de stockage, puis sur le menu déroulant **Actions** et sélectionnez **Planifier un instantané**.
2. La boîte de dialogue Nouveau Planning vous propose d'effectuer votre choix parmi trois fréquences d'instantané. Vous pouvez utiliser une combinaison de ces trois fréquences pour créer un planning d'instantané complet. 
   - Horaire
      - Indiquez la minute de chaque heure à laquelle l'instantané doit être pris ; la minute en cours est indiquée par défaut. 
      - Indiquez le nombre d'instantanés horaires à conserver avant la suppression du plus ancien d'entre eux. 
   - Quotidienne
      - Indiquez l'heure et la minute auxquelles un instantané doit être réalisé ; l'heure et la minute en cours sont indiquées par défaut.
      - Indiquez le nombre d'instantanés quotidiens à conserver avant la suppression du plus ancien d'entre eux. 
   - Hebdomadaire
      - Indiquez le jour de la semaine, l'heure et la minute de la prise de l'instantané ; le jour, l'heure et la minute en cours sont indiqués par défaut.
      - Sélectionnez le nombre d'instantanés hebdomadaires à conserver avant la suppression du plus ancien d'entre eux. 
3. Cliquez sur **Enregistrer** et créez un autre planning avec une fréquence différente. Si le nombre total d'instantanés planifiés est supérieur à 50, vous recevez un message d'avertissement et vous ne pouvez pas procéder à l'enregistrement. 

La liste des instantanés s'affiche au fur et à mesure qu'ils sont pris dans la section Instantanés de la page Détails. 

## Comment prendre un instantané manuel ?

Il est possible de prendre des instantanés manuels à différents points d'une opération de mise à niveau ou de maintenance d'une application. Vous pouvez également prendre des instantanés sur plusieurs machines temporairement désactivées au niveau de l'application. 

Vous disposez d'un maximum de 50 instantanés manuels par volume de stockage. 

1. Cliquez sur votre volume de stockage.
2. Cliquez sur le menu déroulant Actions.
3. Sélectionnez **Prendre un instantané manuel**.
L'instantané est pris et s'affiche dans la sections Instantanés de la page Détails. Son planning est indiqué comme Manuel.

## Comment afficher la liste des instantanés avec l'espace consommé et les fonctions de gestion ? 

La liste des instantanés conservés et de l'espace consommé s'affiche sur la page Détails (**Stockage** > **{{site.data.keyword.filestorage_short}}**). Les fonctions de gestion (édition des plannings et ajout d'espace supplémentaire) figurent sur la page **Détail** dans le menu déroulant **Actions** ou les liens des différentes sections de la page. 

## Comment afficher la liste des instantanés conservés ? 

Les instantanés conservés dépendent du nombre que vous avez saisi dans la zone **Conserve les n derniers** lors de la configuration de vos plannings. Vous pouvez afficher les instantanés pris dans la section **Instantané**. Les instantanés sont indiqués par planning. 

## Comment afficher la quantité d'espace d'image instantanée utilisée ? 

Le graphique circulaire en haut de la page **Détails** indique la quantité d'espace utilisé et la quantité d'espace restant. Vous recevez des notifications lorsque vous commencez à atteindre les seuils d'espace suivants : 75 %, 90 % 95 %. 
## Comment modifier la quantité d'espace d'image instantanée pour mon volume ? 

Il se peut que vous deviez ajouter de l'espace d'image instantanée à un volume qui n'en disposait pas auparavant ou qui en requiert davantage. Vous pouvez ajouter entre 5 Go et 4 000 Go, selon vos besoins. Remarque : Il est uniquement possible d'accroître l'espace, et pas de le réduire.  Vous pouvez sélectionner une quantité peu élevée d'espace jusqu'à ce que vous déterminiez vos besoins réels. Gardez à l'esprit que les instantanés automatisés et manuels partagent le même espace. 

Pour modifier l'espace d'image instantanée, accédez à **Stockage** > **{{site.data.keyword.filestorage_short}}**.

1. Cliquez sur vos volumes de stockage, puis sur le menu déroulant **Actions** et sélectionnez **Ajouter de l'espace instantané supplémentaire**.
2. Effectuez votre choix dans la plage de tailles présentée par l'invite. Les tailles vont généralement de 0 à la taille de votre volume.
3. Cliquez sur **Continuer** pour mettre à disposition l'espace supplémentaire. 
4. Entrez un code promo le cas échéant et cliquez sur **Recalculer**. Des valeurs par défaut sont indiquées dans les zones **Prix pour cette commande** et **Vérification de la commande**.
5. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**. Votre espace d'image instantanée supplémentaire est mis à disposition en quelques minutes.

## Comment recevoir les notifications lorsque la limite d'espace d'image instantanée est presque atteinte et en cas de suppression d'instantanés ? 

Des notifications sont envoyées via des tickets de demande de service par le support à l'utilisateur maître sur votre compte lorsque vous atteignez trois seuils d'espace distincts : 75 %, 90 % et 95 %. 

- 75 % de la capacité : un avertissement indiquant que l'utilisation de l'espace d'image instantanée a dépassé 75 % de la capacité est envoyé. Si vous tenez compte de l'avertissement et que vous ajoutez de l'espace ou que vous supprimez des instantanés conservés et inutiles, l'action est notée et le ticket est fermé. Si vous ne faites rien, vous devez manuellement accuser réception du ticket, qui est ensuite fermé.
- 90 % de la capacité : un second avertissement est envoyé lorsque l'utilisation de l'espace d'image instantanée dépasse 90 % de la capacité. Comme lors du dépassement de 75 % de la capacité, si vous prenez les actions nécessaires pour réduire l'espace utilisé, l'action est notée et le ticket est fermé. Si vous ne faites rien, vous devez manuellement accuser réception du ticket, qui est ensuite fermé.
- 95 % de la capacité : un avertissement final est envoyé. Si vous ne faites rien pour ramener votre espace sous le seuil, une notification est générée et une suppression automatique est instaurée empêchant la création de futurs instantanés. Les instantanés planifiés sont supprimés, en commençant par le plus ancien, jusqu'à ce que l'utilisation soit ramenée au-dessous de 95 % ; cette suppression est effectuée à chaque fois que l'utilisation dépasse 95 % de la capacité jusqu'à ce qu'elle retombe sous le seuil. Si l'espace est réduit manuellement ou que des instantanés sont supprimés, l'avertissement est réinitialisé et émis à nouveau en cas de nouveau dépassement du seuil. Si aucune action n'est effectuée, il s'agit du seul avertissement que l'utilisateur reçoit. 

## Comment supprimer un planning d'instantané ?

Vous pouvez annuler des plannings d'instantané en accédant à **Stockage** > **{{site.data.keyword.filestorage_short}}**.

1. Cliquez sur le planning à supprimer dans le cadre **Plannings d'instantané** sur la page **Détails**.
2. Cochez la case située en regard du planning à supprimer et cliquez sur **Enregistrer**.<br/>
Attention : SI vous utilisez la fonctionnalité de réplication, vérifiez que le planning à supprimer n'est pas celui qui est employé par la réplication. Cliquez [ici](replication.html) pour plus d'informations sur la suppression d'un planning de réplication. 

## Comment supprimer un instantané ?

Il est possible de supprimer manuellement de instantanés inutiles afin de libérer de l'espace pour de futurs instantanés. Pour ce faire, accédez à **Stockage** > **{{site.data.keyword.filestorage_short}}**.

1. Cliquez sur le volume de stockage et faites défiler l'écran jusqu'à la section Instantané pour voir la liste des instantanés existants. 
2. Cliquez sur le menu déroulant Actions en regard d'un instantané en particulier et sélectionnez **Supprimer** pour supprimer l'instantané. Cela n'affecte pas les instantanés futurs ou passés du même planning puisqu'il n'existe pas de dépendance entre les instantanés. 

Les instantanés manuels qui ne sont pas supprimés comme indiqué précédemment sont automatiquement supprimés (le plus ancien d'abord) lorsque vous atteignez les limites en termes d'espace. 

## Comment restaurer mon volume de stockage à un point de cohérence spécifique à l'aide d'un instantané ? 

Il se peut que vous ayez à ramener votre volume de stockage à un point de cohérence précis en raison d'une erreur d'utilisateur ou d'un endommagement des données. 

1. Démontez et déconnectez le volume de stockage de l'hôte. 
   - Cliquez [ici](accessing-file-storage-linux.html) pour obtenir les instructions liées à {{site.data.keyword.filestorage_short}} sur Linux.
2. Cliquez sur **Stockage**, **{{site.data.keyword.filestorage_short}}** dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
3. Faites défiler l'écran et cliquez sur le volume à restaurer. La section Instantanés de la page Détails affiche la liste de tous les instantanés enregistrés, ainsi que leur taille et leur date de création. 
4. Cliquez sur le bouton **Actions** de l'instantané à utiliser, puis sur **Restaurer**. 

   **Remarque** : Une restauration entraîne la perte des données créées ou modifiés depuis la prise de l'instantané utilisé. Une fois la restauration terminée, le volume de stockage revient à l'état dans lequel il se trouvait lors de la prise de l'instantané. Une invite apparaît et vous en informe.
5. Cliquez sur **Oui** pour lancer la restauration. Un message s'affiche en haut de la page indiquant que le volume a été restauré à l'aide de l'instantané sélectionné. De plus, une icône apparaît en regard du volume dans {{site.data.keyword.filestorage_short}} indiquant qu'une transaction active est en cours. Survolez cette icône pour ouvrir une boîte de dialogue précisant la transaction. L'icône disparaît une fois la transaction terminée. 
6. Montez et reconnectez le volume de stockage à l'hôte. 
   - Cliquez [ici](accessing-file-storage-linux.html) pour obtenir les instructions liées à {{site.data.keyword.filestorage_short}} sur Linux.
   
**Remarque** : La restauration d'un volume entraîne la suppression de tous les instantanés antérieurs à l'instantané restauré. 

