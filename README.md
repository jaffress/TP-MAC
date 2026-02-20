# TP5 - Identification d'adresses MAC et IP

## 📝 Objectif du TP
Ce TP a pour but d'analyser le déplacement des paquets de données (PDU) à l'intérieur d'un réseau local et à travers un routeur. L'objectif principal est de comprendre le comportement des adresses MAC (Couche 2) par rapport aux adresses IP (Couche 3) au fur et à mesure que les paquets transitent sur les différents équipements réseau.

## 🏗️ Topologie et Adressage
La topologie de ce laboratoire étudié sous Cisco Packet Tracer inclut :
- **1 Routeur** pour relier les différents réseaux (`10.0.0.0` et `172.16.0.0`).
- **2 Commutateurs (Switches)** (Switch0 et Switch1).
- **1 Concentrateur (Hub)**.
- **1 Point d'accès sans fil (Access Point)**.
- **Plusieurs postes clients** répartis dans les deux réseaux (ex : `10.10.10.2`, `10.10.10.3`, `172.16.31.2` etc.).

## ⚙️ Travail réalisé
- [x] **Topologie :** Prise en main de l'architecture réseau existante.
- [x] **Simulation :** Exécution de la commande `ping` mode simulation.
- [x] **Analyse PDU :** Inspection de la trame OSI sur le parcours complet pour collecter les informations des paquets.
- [x] **Réponses aux questions :** Analyse des adresses MAC et IPv4, ainsi que du comportement des équipements.

## 💻 Explication des Commandes
- `ping <IP_Dest>` : Envoie des paquets ICMP "Echo Request" (requête d'écho) vers une adresse IP distante. Si l'appareil est joignable, il renvoie un paquet ICMP "Echo Reply" (réponse d'écho). Cette commande permet de valider la connectivité réseau de bout en bout (Couche 3).
- **Mode Simulation** : Fonctionnalité sur Packet Tracer qui met le temps en pause et permet d'avancer paquet par paquet (PDU) afin d'inspecter l'encapsulation (en-têtes, couches du modèle OSI) à chaque saut (par exemple, regarder la MAC source/destination et l'IP source/destination sur un switch ou un routeur).

---

## ❓ Réponses aux Questions de Réflexion (Partie B)

**A-t-on utilisé différents types de supports pour connecter les périphériques ?**
> Oui, on a utilisé des supports filaires (câbles à paire torsadée) pour connecter les ordinateurs aux Switchs, Hubs et Routeur, ainsi qu'un support sans fil (ondes radio Wi-Fi) pour le point d'accès.

**Les différents types de support ont-ils modifié le traitement de la PDU de quelque manière que ce soit ?**
> Non. Bien que le signal physique soit différent, les données (PDU), y compris les adresses MAC et IP, ne changent pas en raison du support. Seule la technique de transmission du signal varie.

**Le concentrateur (Hub) a-t-il perdu certaines informations ?**
> Non. Le hub fonctionne comme un simple répéteur. Il n'altère ni ne perd aucune information contenue dans la PDU, il se contente de recopier le signal sur tous ses autres ports.

**Que fait le concentrateur des adresses MAC et IP ?**
> Il ne fait rien. Il travaille au niveau de la Couche 1 (Physique) du modèle OSI. Il ne lit ni les adresses MAC (Couche 2), ni les adresses IP (Couche 3).

**Le point d'accès sans fil a-t-il utilisé les informations qui lui ont été communiquées ?**
> Oui, un point d'accès classique se comporte comme un "Hub sans fil" ou un pont (bridge). Il transforme les trames **802.11 (Wi-Fi)** en trames **802.3 (Ethernet)**. Il ne regarde pas l'adresse IP.

**Des adresses MAC ou IP ont-elles été perdues durant la transmission sans fil ?**
> Non, aucune adresse n'a été perdue, sinon la communication de bout en bout échouerait.

**Quelle est la couche OSI la plus élevée utilisée par le concentrateur et le point d'accès ?**
> - Le **Concentrateur (Hub)** : Couche 1 (Physique).
> - Le **Point d'accès** : Couche 2 (Liaison de données) la plupart du temps pour faire le pont entre 802.11 (Wi-Fi) et 802.3 (Ethernet).

**Lors de l'examen de l'onglet PDU Details, quelle adresse MAC est apparue en premier lieu ? L'adresse source ou l'adresse de destination ?**
> L'adresse MAC de **destination** apparaît en premier dans l'en-tête Ethernet.

**Pourquoi les adresses MAC doivent-elles apparaître dans cet ordre ?**
> Cela permet aux commutateurs (switches) de commencer à déterminer sur quel port envoyer la trame (et donc de commencer la transmission) le plus rapidement possible, sans même avoir besoin de lire le reste de la trame (technique de *Cut-Through Switching*). Cela permet aussi à la carte réseau (NIC) de l'hôte de rejeter immédiatement la trame si l'adresse de destination ne lui correspond pas, sans consommer de ressources pour lire le reste.

**Chaque fois que la PDU a été envoyée entre le réseau 10 et le réseau 172, les adresses MAC changeaient soudainement à un certain point. Où cela s'est-il produit ?**
> Cela s'est produit au niveau du **Routeur**. Lors du passage d'un réseau à un autre, le routeur procède à une étape de **Désencapsulation / Réencapsulation**. Il retire l'enveloppe MAC du réseau A pour encapsuler le paquet IP dans une nouvelle trame adaptée au réseau B avec la nouvelle adresse MAC de l'interface de sortie et la nouvelle adresse MAC de destination.

**Quel périphérique utilise des adresses MAC commençant par 00D0 ?**
> Sur cet exercice, ce sont les interfaces du **Routeur**.

**À quels périphériques les autres adresses MAC appartenaient-elles ?**
> Aux ordinateurs sources et destinations du ping. Par exemple, la MAC Source initiale était celle du PC envoyant le ping, et la MAC destination finale était celle du PC qui le recevait. 

**Les adresses IPv4 d'émission et de réception ont-elles varié dans l'une des PDU ?**
> **Non**. Contrairement aux adresses MAC qui changent à chaque routeur traversé, les adresses IP identifient les machines de bout en bout et ne changent jamais durant le trajet.

**Lors du suivi de la réponse à une requête ping, les adresses IPv4 d'émission et de réception ont-elles varié ?**
> Les adresses sont les mêmes, mais elles sont **interverties**. L'adresse de destination de l'aller devient l'adresse source du retour, et inversement.

**Pourquoi différents réseaux IP doivent être affectés à différents ports d'un routeur ?**
> Un routeur a pour rôle principal d'interconnecter des sous-réseaux logiquement différents. S'il avait deux ports dans le même réseau, il ne saurait pas de quel côté router les paquets de ce réseau. Chaque interface de routeur est donc la passerelle par défaut d'un réseau IP unique.
