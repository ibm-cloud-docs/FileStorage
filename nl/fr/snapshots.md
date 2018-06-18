---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Instantanés

Les instantanés sont une fonction d'{{site.data.keyword.filestorage_full}}. Un instantané représente le contenu d'un volume à un point précis dans le temps. Les instantanés vous permettent de protéger vos données sans impact sur les performances et avec une consommation minimale de l'espace. Ils sont considérés comme votre première ligne de défense dans le cadre de la protection des données. La fonction d'instantané permet de restaurer facilement et rapidement les données à partir d'une copie d'image instantanée si un utilisateur modifie ou supprime accidentellement des données essentielles d'un volume.

{{site.data.keyword.filestorage_short}} propose deux manières de prendre vos instantanés. La première méthode consiste à utiliser un planning d'instantané configurable qui crée et supprime automatiquement les copies d'image instantanée de chaque volume de stockage. Vous pouvez également créer des plannings d'instantané supplémentaires, supprimer des copies manuellement et gérer les plannings en fonction de vos besoins. La seconde méthode consiste à prendre un instantané manuel.

Une copie d'image instantanée est une image en lecture seule d'un volume {{site.data.keyword.filestorage_short}} qui capture l'état du volume à un point dans le temps. Les copies d'image instantanée sont extrêmement efficaces en termes de durée de création et d'espace de stockage. La création d'une copie d'image instantanée {{site.data.keyword.filestorage_short}} ne prend que quelques seconde, généralement moins d'une seconde, quel que soit la taille du volume ou le niveau d'activité sur le stockage. Une fois la copie d'image instantanée créée, les modifications apportées aux objets sont reflétées dans les mises à jour de la version en cours des objets, comme s'il n'existait pas de copies d'image instantanée. Pendant ce temps, la copie des données reste complètement stable. 

Une copie d'image instantanée ne génère pas de surcharge des performances ; les utilisateurs peuvent facilement stocker jusqu'à 50 instantanés planifiés et 50 instantanésmanuels par volume {{site.data.keyword.blockstorageshort}}, tous accessibles en lecture seule et sous forme de versions en ligne des données.

Vous pouvez utiliser des instantanés pour :

- créer des points de récupération à un point de cohérence sans interruption,
- rétablir des volumes à des points de cohérence.
- Vous devez acheter une certaine quantité d'espace d'image instantanée pour votre volume pour pouvoir en prendre des instantanés. Il est possible d'ajouter de l'espace d'image instantanée lors de la commande initiale du volume ou après sa mise à disposition initiale via la page des **détails sur le volume**. Gardez à l'esprit que les instantanés planifiés et manuels partagent l'espace d'image instantanée ; veillez à commander l'espace en conséquence. Pour plus de détails et pour obtenir de l'aide, voir [Commande d'instantanés](ordering-snapshots.html).

## Meilleures pratiques concernant les instantanés
La conception des instantanés dépend de l'environnement du client. Prenez en compte les éléments suivants pour planifier et implémenter les copies d'image instantanée : 
- Il est possible de créer jusqu'à 50 instantanés via un planning et jusqu'à 50 instantanés manuels sur chaque volume ou numéro d'unité logique. 
- Ne prenez pas trop d'instantanés. Veillez à ce que la fréquence des instantanés planifiés corresponde à vos besoins en termes d'objectif de temps de reprise et d'objectif de point de reprise, ainsi qu'à vos exigences professionnelles liées aux applications et planifiez des instantanés à un rythme horaire, quotidien ou hebdomadaire. 
- La fonction de suppression automatique des instantanés permet de contrôler la croissance de la consommation d'espace de stockage. <br/>
    **Remarque** : le seuil de suppression automatique est fixé à 95 %.
    
## Comment les copies d'image instantanée utilisent-elles l'espace disque ?

Les copies d'image instantanée minimisent la consommation du disque en conservant des blocs individuels plutôt que des fichiers entiers. Elles commencent à utiliser de l'espace supplémentaire uniquement en cas de modification ou de suppression des fichiers dans le système de fichiers actif. Dans ce cas, les blocs de fichier d'origine sont toujours conservés dans une ou plusieurs copies d'image instantanée.

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
        <td style="border: 0.0px;">Avant la création d'une copie d'image instantanée, l'espace disque est utilisé uniquement par le système de fichiers actif.</td>
        <td style="border: 0.0px;">Après la création d'une copie d'image instantanée, le système de fichiers actif et la copie d'image instantanée pointent vers les mêmes blocs disque. La copie d'image instantanée n'utilise pas d'espace disque supplémentaire.</td>
        <td style="border: 0.0px;">Même après la suppression de <i>myfile.txt</i> du système de fichiers actif, la copie d'image instantanée inclut toujours le fichier et fait référence à ses blocs disque. Par conséquent, la suppression de données appartenant au système de fichiers actif ne libère pas toujours de l'espace disque.</td>
      </tr>
    </tbody>
</table>

Pour afficher la quantité d'espace d'image instantanée utilisée, suivez les instructions fournies dans le document [Gestion des instantanés](working-with-snapshots.html).
