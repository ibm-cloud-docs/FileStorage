---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-11"

---
{:pre: .pre}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Activation des trames Jumbo dans {{site.data.keyword.BluSoftlayer_notm}} pour Windows et Linux

Une trame Jumbo est une trame Ethernet dotée d'un contenu supérieur à l'unité de transmission maximale (MTU) standard de 1 500 octets. Les trames Jumbo sont utilisées sur des réseaux locaux pouvant prendre en charge au moins 1 Gbps et atteindre 9 000 octets.

Les trames Jumbo doivent être configurées de manière identique sur la totalité du chemin réseau depuis l'unité source <-> commutateur <-> routeur <-> commutateur <-> jusqu'à l'unité de destination. Si la définition n'est pas identique sur la totalité de la chaîne, elle prend par défaut le paramètre le moins élevé sur l'ensemble de la chaîne. Actuellement, la valeur définie pour les périphériques réseau d'{{site.data.keyword.BluSoftlayer_full}} est 9 000. Tous les périphériques de client doivent être définis de manière identique.
{:important}

## Activation des trames Jumbo dans Windows

1. Ouvrez le **Centre Réseau et partage**.
2. Cliquez sur **Modifier les paramètres de la carte**.
3. Cliquez avec le bouton droit de la souris sur la carte d'interface réseau pour laquelle vous voulez activer les trames Jumbo et sélectionnez **Propriétés**.
4. Sur l'onglet **Gestion de réseau**, cliquez sur **Configurer** pour l'adaptateur de réseau.
5. Sélectionnez l'onglet **Avancé**.
6. Sélectionnez **Trame Jumbo** et remplacez la valeur **désactivé** par la valeur souhaitée. La valeur, telle que 9 ko ou 9014 octets, dépend de la carte d'interface réseau.
7. Cliquez sur **OK** dans toutes les fenêtres.

Lorsque vous effectuez cette modification, la carte d'interface réseau perd la connectivité du réseau pendant quelques secondes. Redémarrez le périphérique pour confirmer que la modification a été appliquée.
{:tip}


## Activation des trames Jumbo dans Linux

1. Editez le fichier de configuration réseau pour l'interface eth0.
   - Pour les utilisateurs CentOS, RHEL, Fedora Linux, éditez `/etc/sysconfig/network-script/ifcfg-eth0`
     ```
     # vi /etc/sysconfig/network-script/ifcfg-eth0
     ```
     {: pre}

   - Pour les utilisateurs Debian et Ubuntu Linux, éditez `/etc/network/interfaces`.

2. Ajoutez l'instruction de configuration suivante qui précise la taille de la trame en octets :
   - CentOS, RHEL, Fedora Linux
     ```
     MTU 9000
     ```
     {: pre}

   - Debian et Ubuntu Linux
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
