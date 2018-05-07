---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-24"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_short}} - Foire aux questions

## Comment les IOPS sont-elles mesurées ?

Les IOPS sont mesurées en fonction d'un profil de chargement de blocs de 16 ko avec une répartition aléatoire de 50 % de lectures et 50 % d'écritures. Les charges de travail qui diffèrent de ce profil sont susceptibles de connaître des performances moins élevées.

## Que se passe-t-il si j'utilise une taille de bloc plus petite lors de la mesure des performances ?

Il est possible d'atteindre le nombre maximal d'IOPS en utilisant des tailles de bloc inférieures, mais le débit est alors diminué. Par exemple, un volume doté de 6 000 IOPS présente les débits suivants en fonction des tailles de bloc :

- 16 ko * 6 000 IOPS == ~93,75 Mo/sec
- 8 ko * 6 000 IOPS == ~46,88 Mo/sec
- 4 ko * 6 000 IOPS == ~23,44 Mo/sec


## Le volume doit-il être préchauffé pour obtenir le débit prévu ?

Il n'est pas nécessaire de préchauffer le volume. Le débit indiqué peut être observé immédiatement après la mise à disposition du volume.

## Comment savoir si tel ou tel numéro d'unité logique/volume {{site.data.keyword.filestorage_short}} est chiffré ?

Lorsque vous affichez la liste des stockages {{site.data.keyword.filestorage_short}} dans le portail client, une icône en forme de verrou apparaît à droite du nom d'unité logique ou de volume chiffré.

## Si je dispose d'un stockage {{site.data.keyword.filestorage_short}} non chiffré mis à disposition dans un centre de données ayant été mis à jour pour le chiffrement, puis-je chiffrer mon stockage {{site.data.keyword.filestorage_short}} ?

Il n'est pas possible de chiffrer un stockage {{site.data.keyword.filestorage_short}} mis à disposition avant la mise à niveau du centre de données. Un nouveau stockage {{site.data.keyword.filestorage_short}} mis à disposition dans des centres de données mis à niveau est automatiquement chiffré ; le chiffrement est automatique et ne requiert aucune action de votre part pour le paramétrage. Il est possible de chiffrer les données figurant sur un stockage non chiffré dans un centre de données mis à niveau en créant un nouveau volume de fichier, puis en copiant les données sur ce nouveau volume chiffré ou en procédant à une migration basée sur l'hôte. Pour obtenir les instructions de migration, reportez-vous à  [cet article](/docs/infrastructure/FileStorage/migrate-file-storage-encrypted-file-storage.html).

## Comment savoir si je mets à disposition un stockage {{site.data.keyword.filestorage_short}} dans un centre de données mis à niveau ?

Lors de la mise à disposition de {{site.data.keyword.filestorage_short}}, tous les centres de données mis à niveau sont marqués d'un astérisque (`*`) dans le formulaire de commande et une indication précise que vous effectuez une mise à disposition de stockage avec chiffrement. Une fois le stockage mis à disposition, une icône apparaît dans la liste des stockages pour indiquer que le volume est chiffré. Les volumes chiffrés sont uniquement mis à disposition dans des centres de données mis à niveau. Vous trouverez la liste complète des centres de données mis à niveau et des fonctionnalités disponibles [ici](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## Pourquoi puis-je mettre à disposition un stockage {{site.data.keyword.filestorage_short}} de type Endurance avec un niveau de 10 IOPS dans certains centres de données et pas dans d'autres ?

Le type de stockage {{site.data.keyword.filestorage_short}} Endurance avec un niveau de 10 IOPS/Go est disponible uniquement dans des centres de données sélectionnés, auxquels vont bientôt s'ajouter de nouveaux centres de données.  Vous trouverez la liste complète des centres de données mis à niveau et des fonctionnalités disponibles [ici](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## Comment faire pour trouver le point de montage correct de mon stockage {{site.data.keyword.filestorage_short}} ?

Tous les volumes {{site.data.keyword.filestorage_short}} chiffrés mis à disposition dans ces centres de données ont un point de montage différent de celui des volumes non chiffrés. Pour vérifier que vous utilisez le bon point de montage pour les volumes {{site.data.keyword.filestorage_short}} chiffrés et non chiffrés, vous pouvez afficher les informations sur le point de montage sur la page **Détails du volume** de l'interface utilisateur ou accéder au point de montage correct via un appel d'API : `SoftLayer_Network_Storage::getNetworkMountAddress()`.

## Combien de partages de fichiers sont-ils autorisés par taille de volume de fichier ? Quelle est la taille maximale de partage de fichiers autorisée par taille de volume ?
Voici le nombre maximal d'i-nodes ou de partages de fichiers autorisés en fonction de la taille du volume :

<table>
        <tbody>
          <tr>
            <th>Taille de volume</th>
            <th>I-nodes/fichiers</th>
          </tr>
          <tr>
            <td>20 Go </td>
            <td>622 484</td>
          </tr>
          <tr>
            <td>40 Go </td>
            <td>1 245 084</td>
          </tr>          
          <tr>
            <td>80 Go</td>
            <td>2 490 263</td>
          </tr>          
          <tr>
            <td>100 Go</td>
            <td>3 112 863</td>
          </tr>          
          <tr>
            <td>250 Go</td>
            <td>7 782 300</td>
          </tr>          
          <tr>
            <td>500 Go</td>
            <td>15 564 695</td>
          </tr>
          <tr>
            <td>1 To et +</td>
            <td>31 876 593</td>
          </tr>
        </tbody>
</table>

## Combien de volumes puis-je mettre à disposition ?

Par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes de bloc et de stockage de fichier.  Veuillez contacter votre ingénieur commercial si vous souhaitez augmenter ce nombre de volumes.

## Combien d'instances peuvent partager l'utilisation d'un volume {{site.data.keyword.filestorage_short}} mis à disposition ?

Le nombre d'autorisations par volume de fichier est limité par défaut à 64. Veuillez contacter votre ingénieur commercial si vous souhaitez augmenter cette limite.

## Lors de la mise à disposition d'un stockage {{site.data.keyword.filestorage_short}} de type Performance ou Endurance, les IOPS allouées sont-elles appliquées par instance ou par volume ?

Les IOPS sont appliquées au niveau du volume. Autrement dit, deux hôtes connectés à un volume doté de 6 000 IOPS partagent ces 6 000 IOPS.

## Est-il possible d'atteindre un débit plus élevé si j'utilise une connexion Ethernet plus rapide ?

Les limites de débit sont configurées par volume ou par numéro d'unité logique. Par conséquent, une connexion Ethernet plus rapide ne permet pas d'augmenter la limite définie. Une connexion Ethernet plus lente risque toutefois de générer un goulot d'étranglement.

## Les pare-feux et groupes de sécurité ont-ils un impact sur les performances ?

Il est recommandé d'exécuter le trafic de stockage sur un réseau local virtuel qui ignore le pare-feu. L'exécution du trafic de stockage via des pare-feux logiciels augmente le temps d'attente et a un impact négatif sur les performance de stockage.

## Quel temps d'attente lié aux performances puis-je attendre de mon stockage {{site.data.keyword.filestorage_short}} ?   

Le temps d'attente cible du stockage est < 1 ms. Notre stockage est connecté à des instances de traitement sur un réseau partagé ; le temps d'attente exact des performances dépend donc du trafic réseau sur une période donnée.

## Que deviennent les données en cas de suppression des volumes {{site.data.keyword.filestorage_short}} ?

Lorsque le stockage est supprimé, tous les pointeurs dirigés vers les données de ce volume sont supprimés et les données deviennent donc inaccessibles. Si le stockage physique est à nouveau mis à disposition sur un autre compte, un nouvel ensemble de pointeurs est affecté. Le nouveau compte ne peut pas accéder aux données susceptibles de s'être trouvées sur le stockage physique et le nouvel ensemble de pointeurs indique 0. Lorsque de nouvelles données sont écrites sur le volume ou le numéro d'unité logique, les données inaccessibles toujours existantes sont écrasées. 

## Qu'arrive-t-il aux unités déclassées du centre de données de cloud ?

Lorsque des unités sont déclassées, IBM les détruit avant de les supprimer, ce qui les rend inutilisables. Toutes les données écrites sur ces unités deviennent inaccessibles.

