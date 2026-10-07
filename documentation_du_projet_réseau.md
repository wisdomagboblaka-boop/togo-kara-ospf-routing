# Documentation du Projet Réseau

Ce dépôt contient les fichiers de configuration pour les équipements réseaux (routeurs et commutateurs) d'une architecture d'entreprise segmentée.

---

## 🏗️ Architecture et Plan d'Adressage (VLANs)

L'infrastructure réseau repose sur plusieurs VLANs pour isoler les différents départements et services :

| ID VLAN | Nom du VLAN | Sous-réseau IP | Passerelle par défaut (Gateway) | Rôle / Description |
| :---: | :--- | :--- | :--- | :--- |
| **10** | `DIRECTION` | `10.10.0.224/27` | `10.10.0.225` | Réseau dédié à la direction |
| **20** | `COMPTA` | `10.10.0.192/27` | `10.10.0.193` | Réseau du service comptabilité |
| **30** | `RH` | `10.10.1.32/28` | `10.10.1.33` | Réseau des ressources humaines |
| **40** | `IT` | `10.10.1.48/28` | `10.10.1.49` | Réseau du service informatique |
| **50** | `SERVEURS` | `10.10.1.0/27` | `10.10.1.1` | Hébergement des services internes (DNS, etc.) |
| **60** | `WIFI-CORP` | `10.10.0.128/26` | `10.10.0.129` | Réseau Wi-Fi corporatif |
| **70** | `WIFI-GUEST` | `10.10.0.0/25` | `10.10.0.1` | Réseau Wi-Fi invités (isolé) |
| **99** | `MGMT` | `10.10.1.64/29` | `10.10.1.65` | Réseau de gestion / administration |
| **100** | `DMZ` | `10.10.1.72/29` | `10.10.1.73` | Zone démilitarisée (Serveur Web) |
| **999** | `NATIVE-TRASH` | - | - | VLAN natif et poubelle pour la sécurité |

---

## ⚙️ Description des Fichiers de Configuration

* **`ROUTER-EDGE.txt`** : Configuration du routeur de bordure principal. Gère le routage inter-VLAN (Router-on-a-Stick), l'attribution dynamique des adresses IP via les serveurs DHCP intégrés, les listes de contrôle d'accès (ACL) pour l'isolation des invités, ainsi que le routage dynamique OSPF vers le lien WAN.
* **`ROUTER-KARA.txt`** : Configuration du second routeur distant (site de Kara). Assure la connectivité WAN et intègre les réseaux locaux dans le domaine de routage OSPF global.
* **`SWITCH-CORE.txt`** : Configuration du commutateur central (Core Switch). Élu racine pour le Spanning-Tree (Rapid-PVST), il centralise les liens Trunk, gère l'agrégation de liens (EtherChannel LACP) vers la distribution et connecte la zone DMZ.
* **`SWITCH-DISTRBUTION.txt`** : Configuration du commutateur de distribution. Fait la liaison entre le cœur de réseau et les couches d'accès via EtherChannel et des liens Trunk sécurisés.
* **`SWITCH-ACCESS.txt`** : Configuration du commutateur d'accès. Assure la connexion des utilisateurs finaux, la mise en place de la sécurité des ports (Port Security) et la désactivation des ports inutilisés vers le VLAN poubelle.
* **`SWITCH-DMZ.txt`** : Configuration du commutateur dédié à la DMZ pour l'accueil sécurisé des serveurs exposés.