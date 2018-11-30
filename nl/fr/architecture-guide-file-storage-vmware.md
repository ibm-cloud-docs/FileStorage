---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:pre: .pre}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Mise à disposition de {{site.data.keyword.filestorage_short}} avec VMware

Les étapes décrites ci-après vous permettent de commander et configurer {{site.data.keyword.filestorage_full}} dans un environnement vSphere 5.5 et vSphere 6.0 sur {{site.data.keyword.BluSoftlayer_full}}.

{{site.data.keyword.filestorage_short}} est conçu pour prendre en charge des applications avec un volume élevé d'entrées-sorties nécessitant des niveaux prévisibles de performance. La performance prévisible est atteinte grâce à l'allocation d'opérations d'entrée-sortie par seconde (IOPS) au niveau du protocole à des volumes individuels.

Il est recommandé d'utiliser {{site.data.keyword.filestorage_short}} via NFS si vous avez besoin de plus de huit connexions vers votre hôte VMware.
{:tip}

L'offre {{site.data.keyword.filestorage_short}} est accessible et montée via une connexion NFS. Dans un déploiement VMware, il est possible de monter un seul volume sur un maximum de 64 hôtes ESXi sous forme de stockage partagé. Cela dit, vous pouvez également monter plusieurs volumes afin de créer un cluster de stockage et ainsi utiliser la fonction DRS (Distributed Resource Scheduling) de vSphere Storage.

Les options de tarification et de configuration de {{site.data.keyword.filestorage_short}} Endurance et Performance sont facturées en fonction d'une combinaison de l'espace réservé et des IOPS proposées.

## Considérations relatives à la commande

Lorsque vous commandez {{site.data.keyword.filestorage_short}}, tenez compte des informations suivantes :

- Lors du choix de la taille, tenez compte de la taille de la charge de travail et des besoins en débit. La taille est essentielle pour le service Endurance qui met à l'échelle les performances de manière linéaire par rapport à la capacité (IOPS/Go). A l'inverse, le service Performance permet à l'administrateur de choisir la capacité et la performance indépendamment l'une de l'autre. Vous devez prendre en compte les exigences en matière de débit avec le service Performance.

  Le calcul du débit s'effectue comme suit : IOPS x 16 ko. Les IOPS sont mesurées sur la base d'une taille de bloc de 16 Ko avec un mélange 50/50 de lecture/écriture.<br/>L'augmentation du taille de bloc a pour conséquence d'augmenter le débit et de réduire les IOPS. Par exemple, si vous doublez la taille de bloc pour atteindre des blocs de 32 Ko, le débit maximal est conservé, mais les IOPS sont diminuées de moitié.
  {:note}
- NFS utilise des opérations de contrôle de fichier supplémentaires, comme `lookup`, `getattr` et `readdir`. Ces opérations, en plus des opérations de lecture/écriture, peuvent compter comme des IOPS et varier selon le type d'opération et la version de NFS.
- Les volumes {{site.data.keyword.filestorage_short}} sont exposés aux unités, aux sous-réseaux ou aux adresse IP autorisés.
- Pour éviter toute déconnexion du stockage lors du basculement du chemin, {{site.data.keyword.IBM}} recommande d'installer des outils VMware qui définissent une valeur de délai appropriée. Il n'est pas nécessaire de modifier la valeur, le paramètre par défaut est suffisant pour garantir le maintien de la connectivité de l'hôte VMware.
- NFSv3 et NFSv4.1 sont pris en charge dans l'environnement {{site.data.keyword.BluSoftlayer_full}}. Toutefois, {{site.data.keyword.IBM}} recommande d'utiliser NFSv3. En effet, NFSv4.1 est un protocole avec état (et non sans état comme NFSv3) et donc susceptible de générer des anomalies lors des événements de réseau. NFSv4.1 doit mettre au repos toutes les opérations, puis effectuer la réclamation de verrou. Des interruptions peuvent survenir lors de telles opérations.

Pour plus d'informations, consultez le livre blanc sur VMware [Best Practices for running
VMware vSphere on Network Attached Storage ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/vmware-nfs-bestpractices-white-paper-en.pdf){:new_window}
{:tip}

**Matrice de prise en charge des fonctions VMware avec le protocole NFS**
<table>
  <caption>Le tableau 1 présente les fonctions vSphere spécifiques des deux versions de NFS.</caption>
 <thead>
  <tr>
   <th>Fonctions vSphere</th>
   <th>NFS version 3</th>
   <th>NFS version 4.1</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>vMotion et Storage vMotion</td>
   <td>Oui</td>
   <td>Oui</td>
  </tr>
  <tr>
   <td>Haute disponibilité (HA)</td>
   <td>Oui</td>
   <td>Oui</td>
  </tr>
  <tr>
   <td>Tolérance aux pannes (FT)</td>
   <td>Oui</td>
   <td>Oui</td>
  </tr>
  <tr>
   <td>Planificateur de ressources distribuées (DRS)</td>
   <td>Oui</td>
   <td>Oui</td>
  </tr>
  <tr>
   <td>Profils d'hôte</td>
   <td>Oui</td>
   <td>Oui</td>
  </tr>
  <tr>
   <td>DRS de stockage</td>
   <td>Oui</td>
   <td>Non</td>
  </tr>
  <tr>
   <td>SIOC (Storage I/O Control)</td>
   <td>Oui</td>
   <td>Non</td>
  </tr>
  <tr>
   <td>Site Recovery Manager</td>
   <td>Oui</td>
   <td>Non</td>
  </tr>
  <tr>
   <td>Volumes virtuels</td>
   <td>Oui</td>
   <td>Non</td>
  </tr>
 </tbody>
</table>
*Source - [VMware - NFS Protocols and ESXi ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){:new_window}*



### Utilisation d'instantanés

{{site.data.keyword.filestorage_short}} permet aux administrateurs de définir des plannings d'instantané qui créent et suppriment automatiquement les copies d'image instantanée pour chaque volume de stockage. Ils peuvent également créer des plannings d'instantané supplémentaires (horaire, quotidien, hebdomadaire) pour les instantanés automatiques et créer manuellement des instantanés ad hoc pour les scénarios de continuité opérationnelle et de reprise après incident (BCDR). Des alertes automatiques sont transmises via le portail [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window} au propriétaire du volume afin de leur indiquer les instantanés conservés et l'espace utilisé.

Un espace d'image instantanée est requis pour l'utilisation d'instantanés. Vous pouvez acquérir de l'espace lors de la commande de volume initiale ou après la mise à disposition initiale via la page **Détails du volume** en cliquant sur **Actions** et en sélectionnant **Ajouter de l'espace d'instantané**.

Il est important de noter que les environnements VMware n'ont pas connaissance des instantanés. La fonctionnalité d'instantané de {{site.data.keyword.filestorage_short}} Endurance ne doit pas être confondue avec les instantanés VMware. Toute reprise effectuée avec la fonction d'instantané de {{site.data.keyword.filestorage_short}} doit être traitée à partir du portail [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.

La restauration du volume {{site.data.keyword.filestorage_short}} requiert la mise hors tension de toutes les machines virtuelles qui se trouvent sur {{site.data.keyword.filestorage_short}}. Le volume doit être temporairement démonté depuis les hôtes ESXi afin d'éviter que les données ne soient endommagées au cours du processus.

Pour plus d'information sur la configuration des instantanés, voir l'article sur les [instantanés](snapshots.html).


### Utilisation de la réplication

La réplication utilise l'un de vos plannings d'instantané pour copier automatiquement les instantanés sur un volume de destination situé dans un centre de données distant. Les copies peuvent être récupérées sur le site distant en cas d'endommagement des données ou de catastrophe.

Les répliques vous permettent :

- d'effectuer rapidement une reprise après un échec du site et d'autres incidents en basculant sur le volume de destination ;
- d'effectuer un basculement vers un point précis dans le temps dans la copie de reprise après incident.

La réplication permet de synchroniser vos données entre deux emplacements différents. Si vous souhaitez cloner votre volume et l'utiliser indépendamment du volume d'origine, voir [Création d'un volume de fichier en double](how-to-create-duplicate-volume.html).
{:tip}

Avant d'effectuer une réplication, vous devez créer un planning d'instantané.

Lorsque vous effectuez un basculement, vous "basculez l'interrupteur" depuis votre volume de stockage du centre de données principal vers le volume de destination du centre de données distant. Par exemple, votre centre de données principal peut se situer à Londres et votre centre de données secondaire à Amsterdam. Dans le cas d'un événement d'échec, vous basculez vers Amsterdam. Autrement dit, vous vous connectez au volume qui est désormais devenu principal à partir d'une instance de cluster vSphere à Amsterdam. Une fois votre volume de Londres réparé, un instantané du volume d'Amsterdam est pris. Vous pouvez ensuite effectuer une reprise par restauration à Londres avec le volume de Londres à nouveau considéré comme le volume principal à partir d'une instance de traitement située à Londres.

Vous devez arrêter le volume utilisé sur le site distant avant de reprendre par restauration le volume sur le centre de données principal. Un instantané de toute information nouvelle ou modifiée est pris et répliqué sur le centre de données principal avant le nouveau montage sur les hôtes ESXi du site de production.

Pour plus d'informations sur la configuration des répliques, voir [Réplication](replication.html).

Les données non valides, qu'il s'agisse de données endommagées, détournées ou infectées, sont répliquées conformément au planning d'instantané et à la conservation des instantanés. Des fenêtres de réplication plus petites peuvent constituer un meilleur objectif de point de reprise. Néanmoins, cela peut également réduire le temps de réaction en cas de réplication de données non valides.
{:note}


## Commande de {{site.data.keyword.filestorage_short}}

Vous pouvez commander et configurer {{site.data.keyword.filestorage_short}} pour un environnement VMware ESXi 5. Utilisez les informations suivantes ainsi que celles de l'article [Advanced Single-Site VMware Reference Architecture](https://{DomainName}/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window} pour configurer l'une des options de stockage suivantes dans votre environnement VMware.

Vous pouvez commander {{site.data.keyword.filestorage_short}} via le portail [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window} en accédant à la page {{site.data.keyword.filestorage_short}} via **Stockage** > **{{site.data.keyword.filestorage_short}}**.


### 1. Commande de

Suivez les étapes ci-après pour commander {{site.data.keyword.filestorage_short}} :
1. Cliquez sur **Stockage** > **{{site.data.keyword.filestorage_short}}** sur la page d'accueil du portail [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.
2. Cliquez sur **Commander {{site.data.keyword.filestorage_short}}** sur la page **{{site.data.keyword.filestorage_short}}**.
3. Sélectionnez **Endurance**/**Performance** dans la liste **Sélectionner le type de stockage**.
4. Sélectionnez l'emplacement. Les centres de données dotés de fonctionnalités améliorées sont signalés par un astérisque. Vérifiez que le nouveau stockage est ajouté au même emplacement que l'hôte ESXi précédemment commandé.
5. Sélectionnez le mode de facturation. Les options de facturation mensuelle et horaire sont disponibles.
6. Sélectionnez la quantité d'espace de stockage en Go. Pour les téraoctets, 1 To est égal à 1 000 Go et 12 To sont égaux à 12 000 Go.
7. Entrez le nombre souhaité d'IOPS par intervalles de 100 ou sélectionnez un niveau d'IOPS.
8. Indiquez la taille de l'espace d'image instantanée.
9. Cliquez sur **Continuer**.
10. Entrez un code promo le cas échéant et cliquez sur **Recalculer**.
11. Vérifiez votre commande.
12. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**.
13. Cliquez sur **Valider la commande** pour soumettre la commande ou sur **Annuler** pour fermer le formulaire sans soumettre de commande.

Le stockage est mis à disposition en moins d'une minute et est visible sur la page **{{site.data.keyword.filestorage_short}}** du portail [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.



### 2. Autorisation des hôtes pour l'utilisation de {{site.data.keyword.filestorage_short}}

Une fois qu'un volume est mis à disposition, les serveurs {{site.data.keyword.BluBareMetServers_full}} ou {{site.data.keyword.BluVirtServers_full}} qui vont utiliser le volume doivent être autorisés à accéder au stockage. Suivez les étapes ci-après pour autoriser le volume :

1. Cliquez sur **Stockage** > **{{site.data.keyword.filestorage_short}}**.
2. Sélectionnez **Accès à l'hôte** dans le menu Actions du volume **Endurance** ou **Performance**.
3. Cliquez sur **Sous-réseaux**.
4. Effectuez votre choix dans la liste des sous-réseaux disponibles affectés aux ports VMkernel sur les hôtes ESXi, puis cliquez sur **Soumettre**.<br/>

   Les sous-réseaux affichés sont des sous-réseaux souscrits dans le même centre de données que le volume de stockage.{:note}


Une fois les sous-réseaux autorisés, notez le nom d'hôte du serveur de stockage Endurance ou Performance que vous souhaitez utiliser lors du montage du volume. Vous pouvez trouver ces informations sur la page de détails de {{site.data.keyword.filestorage_short}} en cliquant sur un volume spécifique.


##  Configuration de l'hôte de machine virtuelle VMware

### Respect des conditions requises

Avant de commencer le processus de configuration de VMware, assurez-vous que les conditions requises suivantes sont remplies :

- Les serveurs {{site.data.keyword.BluBareMetServers}} dotés de VMware ESXi sont mis à disposition avec une configuration de stockage adéquate et des données d'identification ESXi.
- Une instance {{site.data.keyword.BluSoftlayer_full}} Windows physique ou des serveurs {{site.data.keyword.virtualmachinesshort}} se trouvent dans le même centre de données que les serveurs {{site.data.keyword.BluBareMetServers}}, avec l'adresse IP publique de la machine virtuelle {{site.data.keyword.BluSoftlayer_full}} Windows et les données d'identification associées.
- Vous possédez un ordinateur disposant d'un accès internet, d'un navigateur Web et d'un client Remote Desktop Protocol (RDP).


### 1. Configuration de l'hôte VMware

1. A partir d'un ordinateur connecté à internet, lancez un client RDP et établissez une session RDP avec les serveurs {{site.data.keyword.BluVirtServers_full}} qui sont mis à disposition dans le même centre de données que celui où vSphere vCenter est installé.
2. A partir des {{site.data.keyword.BluVirtServers_short}}, démarrez un navigateur Web et connectez-vous à VMware vCenter via le client Web vSphere.
3. A partir de l'écran **HOME**, sélectionnez **Hosts and Clusters**. Développez le panneau sur la gauche et sélectionnez le**serveur VMware ESXi** qui doit être utilisé pour ce déploiement.
4. Vérifiez que le port de pare-feu du client NFS est ouvert sur tous les hôtes pour pouvoir configurer le client NFS sur l'hôte vSphere. Ce port est ouvert automatiquement dans les versions les plus récentes de vSphere. Pour vérifier qu'il est bien ouvert, accédez à l'onglet **ESXi host Manage** dans VMware® vCenter™ et sélectionnez **Settings**, puis **Security Profile**. Dans la section **Firewall**, cliquez sur **Edit** et faites défiler l'écran jusqu'à **NFS Client**.
5. Vérifiez que l'option **Allow connection from any IP address or a list of IP addresses** est bien sélectionnée. <br/>
   ![Autorisation de la connexion](/images/1_4.png)
6. Configurez les trames Jumbo en accédant à l'onglet **ESXi host Manage** et en sélectionnant **Manage**, puis **Networking**.
7. Sélectionnez **VMkernel adapters**, mettez en évidence **vSwitch** et cliquez sur **Edit** (icône en forme de crayon).
8. Sélectionnez **NIC setting** et vérifiez que NIC MTU a pour valeur 9000.
9. **Facultatif**. Validez les paramètres de trame Jumbo.
   - Sous Windows : `ping -f -l 8972 a.b.c.d`
   - Sous UNIX : `ping -s 8972 a.b.c.d`
     Où a.b.c.d est l'interface des {{site.data.keyword.BluVirtServers_short}} voisine.
     Exemple
     ```
     ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
     8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
     ```

Pour plus d'informations sur VMware et les trames Jumbo, cliquez [ici ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://kb.vmware.com/s/article/1003712){:new_window}.
{:tip}


### 2. Ajout d'un adaptateur de liaison montante à un commutateur virtuel

1. Configurez un nouvel adaptateur de liaison montante en accédant à l'onglet **ESXi host Manage** et en sélectionnant **Manage**, puis **Networking**.
2. Sélectionnez l'onglet **Physical adapters**.
3. Cliquez sur **Add host networking** (icône en forme de globe avec un signe plus).
4. Sélectionnez **Physical Network Adapter** comme type de connexion, puis cliquez sur **Next**.
5. Sélectionnez le **vSwitch** existant et cliquez sur **Next**.
6. Sélectionnez **Unused adapters** et cliquez sur **Add adapters** (signe Plus).
7. Cliquez sur l'autre adaptateur "Connected", puis sur **OK**. <br/>
   ![Ajout d'adaptateurs physiques à un commutateur](/images/2_3.png)
8. Cliquez sur **Next**, puis sur **Finish**.
9. Revenez à l'onglet **Virtual switches** et sélectionnez **Edit setting** (icône en forme de crayon) sous l'en-tête **Virtual Switches**.
10. Sur la gauche, sélectionnez l'entrée vSwitch **Teaming and failover**.
11. Vérifiez que l'option **Load balancing** est définie sur **Route based on the originating virtual port** et cliquez sur **OK**.


### 3. Configuration du routage statique ESXi (facultatif)

La configuration de réseau de ce guide d'architecture utilise un nombre minimal de groupes de ports. Si vous disposez d'un groupe de ports VMkernel pour le stockage NFS, des étapes supplémentaires doivent être exécutées. Par défaut, ESXi utilise le port vmkernel qui figure sur le même sous-réseau qu'un volume NFS pour monter sur le stockage Endurance/Performance. Etant donné qu'un routage de couche 3 est utilisé pour monter le volume NFS, ESXi doit être forcé à employer le port VMkernel qui a été configuré pour monter le volume NFS. Pour ce faire, une route statique vers la grappe de stockage Endurance/Performance doit être créée.

1. Pour configurer une route statique, établissez une liaison SSH vers chaque hôte ESXi qui utilise le stockage Performance ou Endurance et exécutez les commandes ci-après. Prenez note de l'adresse IP qui résulte de la première commande `ping` et utilisez-la avec la commande réseau `esxcli`.
   ```
   ping <host name of the endurance/performance array>
   ```
   {: pre}

   >**Remarque** : le nom d'hôte DNS du stockage NFS est une zone d'acheminement à laquelle plusieurs adresses IP sont affectées. Ces adresses IP sont statiques et appartiennent à ce nom d'hôte DNS spécifique. Il est possible d'utiliser n'importe laquelle de ces adresses IP pour accéder à un volume spécifique.

   ```
   esxcli network ip route ipv4 add –gateway GATEWAYIP –network <result of ping command>/32
   ```
   {: pre}

2. Les routes statiques ne sont pas permanentes si vous effectuez des réamorçages sur ESXi 5.0 et versions antérieures. Pour garantir le caractère permanent des routes statiques ajoutées, vous devez ajouter ces commandes sur chaque hôte dans le fichier `local.sh` qui se trouve dans le répertoire `/etc/rc.local.d/`. Ouvrez le fichier `local.sh` à l'aide de l'éditeur visuel et ajoutez la seconde commande de l'étape 4.1 devant la ligne `exit 0`.

Notez l'adresse IP car elle peut être utilisée pour le montage du volume lors de l'étape suivante.<br/>Vous devez procéder de la sorte pour chaque volume NFS que vous envisagez de monter sur votre hôte ESXi.<br/>Pour plus d'informations, voir l'article de la base de connaissances de VMware, [Configuring static routes for VMkernel ports on an ESXi host ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://kb.vmware.com/s/article/2001426){:new_window}.
{:tip}


##  Création et montage de volume {{site.data.keyword.filestorage_short}} sur des hôtes ESXi

1. Cliquez sur l'icône **Go to vCenter**, puis sur **Hosts and Clusters**.
2. Sur l'onglet **Related Object**, cliquez sur **Datastores**.
3. Cliquez sur l'icône **Create a new datastore**.
4. Sur l'écran **New Datastore**, sélectionnez l'emplacement du magasin de données WMware (votre hôte ESXi) et cliquez sur **Next**.
5. Sur l'écran **Type**, sélectionnez **NFS**, puis cliquez sur **Next**.
6. Ensuite, sélectionnez la version de NFS. NFSv3 et NFSv4.1 sont pris en charge, mais le premier est recommandé. Prenez soin d'utiliser une seule version de NFS pour accéder à un magasin de données spécifique. Si vous montez un ou plusieurs hôtes sur le même magasin de données en utilisant différentes versions, les données peuvent être endommagées.
7. Sur l'écran **Name and configuration**, entrez le nom que vous souhaitez donner au magasin de données WMware. Indiquez également le nom d'hôte du serveur NFS. L'utilisation du nom de domaine complet du serveur NFS permet d'optimiser la répartition du trafic vers le serveur sous-jacent. L'adresse IP est également valide mais elle est moins fréquemment employée et uniquement dans des instances spécifiques. Entrez le nom de dossier au format `/foldername`.
8. Sur l'écran **Host accessibility**, sélectionnez un ou plusieurs hôtes que vous souhaitez monter sur le magasin de données NFS WMware et cliquez sur **next**.
9. Vérifiez les entrées sur l'écran suivant et cliquez sur **Finish**.
10. Répétez ces étapes pour tous les volumes {{site.data.keyword.filestorage_short}} supplémentaires.

{{site.data.keyword.BluSoftlayer_full}} recommande d'utiliser les noms de domaine complets pour la connexion au magasin de données WMware. L'utilisation de l'adressage IP direct est susceptible d'ignorer le mécanisme d'équilibrage de charge qui est fourni par le nom de domaine complet.
{:important}

Pour employer l'adresse IP au lieu du nom de domaine complet, exécutez simplement une commande ping vers le serveur afin d'obtenir l'adresse IP :
```
ping <host name of the endurance/performance array>
```
{: pre}

Pour obtenir l'adresse IP auprès d'un hôte ESXi, utilisez la commande ci-après. Le résultat 10.2.125.80 obtenu correspond à l'adresse IP avec le nom de domaine complet.
```
~ # vmkping nfsdal0902a-fz.service.softlayer.com
PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
```


## Activation de la fonction ESXi SIOC (facultatif)

SIOC (Storage I/O Control) est une fonctionnalité disponible pour les clients qui utilisent une licence Enterprise Plus. Lorsque la fonction SIOC est activée dans l'environnement, elle modifie la longueur de la file d'attente de l'unité pour une seule machine virtuelle. Cela permet de réduire la file d'attente de la grappe de stockage de toutes les machines virtuelles afin d'obtenir un partage égal. La fonction SIOC est implémentée uniquement si des ressources sont contraintes et que le temps d'attente d'entrée-sortie du stockage est supérieur au seuil défini.


Vous devez définir un seuil pour que la fonction SIOC puisse déterminer quand une unité de stockage est saturée ou contrainte. Le temps d'attente du seuil de surcharge est différent selon les types de stockage. Les sélections par défaut atteignent jusqu'à 90 % du débit en période de pic. Le pourcentage de la valeur de débit en période de pic indique le seuil de temps d'attente estimé lorsque le magasin de données WMware utilise ce pourcentage de débit estimé en période de pic.


Une configuration incorrecte de la fonction SIOC d'un magasin de données WMware ou d'un disque de machine virtuelle (VMDK) peut affecter les performances de manière significative.{:important}


### Configuration de la fonction SIOC pour un magasin de données WMware

Suivez les étapes ci-après pour activer la fonction SIOC avec les valeurs recommandées pour le stockage Endurance et Performance :

1. Accédez au magasin de données WMware dans le navigateur du client Web vSphere.
2. Cliquez sur l'onglet **Manage**.
3. Cliquez sur **Settings**, puis sur **General**.
4. Cliquez sur **Edit** pour **Datastore Capabilities**.
5. Cochez la case **Enable Storage I/O Control**.<br/>
   ![Magasin de données NSF WMware](/images/3_0.png)
6. Cliquez sur **OK**.

Ce paramètre est propre au magasin de données WMware et non à l'hôte.{:note}


### Configuration de la fonction SIOC pour des {{site.data.keyword.BluVirtServers_short}}

Vous pouvez également limiter des disques virtuels individuels de machines virtuelles individuelles ou leur accorder des partages différents grâce à la fonction SIOC. La limitation des disques et l'octroi de partages différents vous permettent de faire correspondre et d'aligner l'environnement avec la charge de travail grâce au nombre d'IOPS du volume {{site.data.keyword.filestorage_short}} acquis. Cette limite est définie par le nombre d'IOPS et il est possible de définir une pondération différente des partages. Les partages de disque virtuel définis sur la valeur High (2 000 partages) reçoivent deux fois plus d'entrées-sorties qu'un disque défini sur la valeur Normal (1 000 partages) et quatre fois plus qu'un disque défini sur la valeur Low (500 partages). Normal est la valeur par défaut de toutes les machines virtuelles ; vous devez donc ajuster les valeurs au-dessus ou au-dessous de la valeur Normal pour les machines virtuelles qui en ont besoin.

Suivez les étapes ci-après pour modifier les partages et la limite de disques virtuels.

1. Choisissez des {{site.data.keyword.BluVirtServers_short}} dans l'inventaire **VMs and Templates**.
2. Sélectionnez les {{site.data.keyword.BluVirtServers_short}} pour lesquels activer la fonction SIOC.
3. Cliquez sur l'onglet **Manage**, puis sur **Settings**. Cliquez sur **Edit**.
4. Développez la flèche **hard disk**. Sélectionnez **Modify the Shares** ou **Limit - IOPs** en fonction de votre environnement. Choisissez un disque dur virtuel dans la liste et modifiez la sélection Shares pour indiquer le nombre relatif de partages à allouer aux serveurs {{site.data.keyword.BluVirtServers_short}} (Low, Normal ou High). Vous pouvez également cliquer sur **Custom** et saisir une valeur de partage définie par l'utilisateur.
5. Cliquez dans la colonne **Limit - IOPS** et entrez le nombre maximal de ressources de stockage à allouer à la machine virtuelle.
6. Cliquez sur **OK**.


Ce processus permet de définir les limites de consommation des ressources des disques virtuels individuels dans des {{site.data.keyword.BluVirtServers_short}} même si la fonction SIOC n'est pas activée. Ces paramètres sont propres à l'invité individuel et non à l'hôte, même s'ils sont utilisés par la fonction SIOC.
{:important}


## Configuration des paramètres côté hôte ESXi

Certains paramètres supplémentaires sont requis pour la configuration d'hôtes ESXi 5.x en vue du stockage NFS. Ce tableau présente les valeurs à affecter à chaque paramètre.

|Paramètre | A définir sur... |
|----------|------------|
|Net.TcpipHeapSize |	32 |
|Net.TcpipHeapMax |	Pour vSphere 5.0/5.1, définir sur 128 <br/> Pour vSphere 5.5 ou version ultérieure, définir sur 512 |
|NFS.MaxVolumes |	256 |
|NFS41.MaxVolumes |	256 (vSphere 6.0 ou version ultérieure uniquement) |
|NFS.HeartbeatMaxFailures |	10 |
|NFS.HeartbeatFrequency |	12 |
|NFS.HeartbeatTimeout |	5 |
|NFS.MaxQueueDepth|	64 |


### Mise à jour des paramètres de configuration avancée sur un hôte ESXi 5.x à l'aide de l'interface de ligne de commande

Les exemples suivants font appel à l'interface de ligne de commande pour définir les paramètres de configuration avancée et les vérifier. L'outil `esxcfg-advcfg` employé dans ces exemples se trouve dans le répertoire `/usr/sbin` sur les hôtes ESXi 5.x.

   - Définition des paramètres de configuration avancée à partir de l'interface de ligne de commande ESXi 5.x

     ```
     #esxcfg-advcfg -s 32 /Net/TcpipHeapSize
     #esxcfg-advcfg -s 128 /Net/TcpipHeapMax(For vSphere 5.0/5.1)
     #esxcfg-advcfg -s 512 /Net/TcpipHeapMax(For vSphere 5.5 and above)
     #esxcfg-advcfg -s 256 /NFS/MaxVolumes
     #esxcfg-advcfg -s 256 /NFS41/MaxVolumes (ESXi 6.0 and above)
     #esxcfg-advcfg -s 10 /NFS/HeartbeatMaxFailures
     #esxcfg-advcfg -s 12 /NFS/HeartbeatFrequency
     #esxcfg-advcfg -s 5 /NFS/HeartbeatTimeout   
     #esxcfg-advcfg -s 64 /NFS/MaxQueueDepth
     #esxcfg-advcfg -s 32 /Disk/QFullSampleSize
     #esxcfg-advcfg -s 8 /Disk/QFullThreshold
     ```

  - Vérification des paramètres de configuration avancée à partir de l'interface de ligne de commande ESXi 5.x

    ```
    #esxcfg-advcfg -g /Net/TcpipHeapSize
   #esxcfg-advcfg -g /Net/TcpipHeapMax
   #esxcfg-advcfg -g /NFS/MaxVolumes
   #esxcfg-advcfg -g /NFS41/MaxVolumes (ESXi 6.0 et version ultérieure)
   #esxcfg-advcfg -g /NFS/HeartbeatMaxFailures
   #esxcfg-advcfg -g /NFS/HeartbeatFrequency
   #esxcfg-advcfg -g /NFS/HeartbeatTimeout
   #esxcfg-advcfg -g /NFS/MaxQueueDepth
   #esxcfg-advcfg -g /Disk/QFullSampleSize
   #esxcfg-advcfg -g /Disk/QFullThreshold
    ```

## Activation des trames Jumbo dans {{site.data.keyword.BluSoftlayer_notm}} pour Windows et Linux

Une trame Jumbo est une trame Ethernet dotée d'un contenu supérieur à l'unité de transmission maximale (MTU) standard de 1 500 octets. Les trames Jumbo sont utilisées sur des réseaux locaux pouvant prendre en charge au moins 1 Gbps et atteindre 9 000 octets.

Les trames Jumbo doivent être configurées de manière identique sur la totalité du chemin réseau depuis l'unité source <-> commutateur <-> routeur <-> commutateur <-> jusqu'à l'unité de destination. Si la définition n'est pas identique sur la totalité de la chaîne, elle prend par défaut le paramètre le moins élevé sur l'ensemble de la chaîne. Actuellement, la valeur définie pour les périphériques réseau d'{{site.data.keyword.BluSoftlayer_full}} est 9 000. Tous les périphériques de client doivent être définis de manière identique.
{:important}

### Activation des trames Jumbo dans Windows

1. Ouvrez le **Centre Réseau et partage**.
2. Cliquez sur **Modifier les paramètres de la carte**.
3. Cliquez avec le bouton droit de la souris sur la carte d'interface réseau pour laquelle vous voulez activer les trames Jumbo et sélectionnez **Propriétés**.
4. Sur l'onglet **Gestion de réseau**, cliquez sur **Configurer** pour l'adaptateur de réseau.
5. Sélectionnez l'onglet **Avancé**.
6. Sélectionnez **Trame Jumbo** et remplacez la valeur **désactivé** par la valeur souhaitée. La valeur, telle que 9 ko ou 9014 octets, dépend de la carte d'interface réseau.
7. Cliquez sur **OK** dans toutes les fenêtres.

Lorsque vous effectuez cette modification, la carte d'interface réseau perd la connectivité du réseau pendant quelques secondes. Redémarrez le périphérique pour confirmer que la modification a été appliquée.
{:tip}


### Activation des trames Jumbo dans Linux

1. Editez le fichier de configuration réseau pour l'interface eth0.
   - Pour les utilisateurs CentOS/RHEL/Fedora Linux, éditez `/etc/sysconfig/network-script/ifcfg-eth0`.
     ```
     # vi /etc/sysconfig/network-script/ifcfg-eth0
     ```
     {: pre}

   - Pour les utilisateurs Debian/Ubuntu Linux, éditez `/etc/network/interfaces`.

2. Ajoutez l'instruction de configuration suivante qui précise la taille de la trame en octets :
   - CentOS/RHEL/Fedora Linux
     ```
     MTU 9000
     ```
     {: pre}

   - Debian/Ubuntu Linux
     ```
     MTU=9000
     ```
     {: pre}

3. Fermez et enregistrez le fichier. Redémarrez l'interface eth0.
   ```
   # /etc/init.d/networking restart
   ```
   {: pre}

   Cette action provoque une courte perte de connectivité réseau.

Pour en savoir plus sur l'architecture de référence d'un environnement VMware mono-site avancé, cliquez [ici](https://{DomainName}/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window}.
{:tip}
