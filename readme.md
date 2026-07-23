# Infrastructure Réseau PME CK_237 - Documentation Technique

**Auteur :** JIDJOU Serena  
**Date :** 23 Juillet 2026  
**Version :** 1.0

---

## 1. Contexte

L’entreprise **CK_237** est une PME en pleine croissance disposant de deux sites :

- **Bafoussam** (Siège social)
- **Douala** (Agence secondaire)

Cette documentation décrit la conception, la configuration et les choix techniques de l’infrastructure réseau Cisco mise en place.

---

## 2. Cahier des Charges (Résumé)

### Site de Bafoussam (Siège)
- **VLAN 10** : Direction (10 postes)
- **VLAN 20** : Comptabilité (128 postes)
- **VLAN 30** : Informatique (20 postes)

### Site de Douala (Agence)
- **VLAN 40** : Commercial (68 postes)
- **VLAN 50** : Support Technique (10 postes)

**Exigences principales :**
- Plan d’adressage VLSM
- Routage inter-VLAN
- Services DHCP par VLAN
- Serveur DNS interne
- Interconnexion des sites via liaison WAN (OSPF)
- Politique de sécurité (ACL)
- Tests de connectivité

---

## 3. Topologie Réseau

![Topologie Complète](Capture%20d%E2%80%99%C3%A9cran%202026-07-23%20131536.png)

*(Image de la topologie Packet Tracer fournie)*

---

## 4. Plan d’Adressage VLSM

### Site Bafoussam – Réseau 192.168.10.0/24

| VLAN | Département       | Postes | Sous-réseau          | Masque              | Passerelle       | Plage DHCP                    |
|------|-------------------|--------|----------------------|---------------------|------------------|-------------------------------|
| 10   | Direction         | 10     | 192.168.10.0/28     | 255.255.255.240     | 192.168.10.1     | 192.168.10.2 – 14             |
| 20   | Comptabilité      | 128    | 192.168.10.16/25    | 255.255.255.128     | 192.168.10.17    | 192.168.10.18 – 142           |
| 30   | Informatique      | 20     | 192.168.10.144/27   | 255.255.255.224     | 192.168.10.145   | 192.168.10.146 – 174          |

### Site Douala – Réseau 192.168.11.0/24

| VLAN | Département         | Postes | Sous-réseau          | Masque              | Passerelle       | Plage DHCP                    |
|------|---------------------|--------|----------------------|---------------------|------------------|-------------------------------|
| 40   | Commercial          | 68     | 192.168.11.0/25     | 255.255.255.128     | 192.168.11.1     | 192.168.11.2 – 70             |
| 50   | Support Technique   | 10     | 192.168.11.128/28   | 255.255.255.240     | 192.168.11.129   | 192.168.11.130 – 142          |

---

## 5. Configurations Serveurs

### Serveur DHCP – Bafoussam
- **Nom** : bafoussam-dhcp
- **IP** : 192.168.10.130
- **Masque** : 255.255.255.224
- **Passerelle** : 192.168.10.129
- **DNS** : 192.168.10.131

### Serveur DNS – Bafoussam
- **Nom** : bafoussam-dns
- **IP** : 192.168.10.131
- **Masque** : 255.255.255.224
- **Passerelle** : 192.168.10.129

---

## 6. Politique de Sécurité

- Le **VLAN 10 (Direction)** n’est accessible que depuis le **VLAN 30 (Informatique)**
- Utilisation d’**ACL** sur le routeur pour filtrer les accès
- Les autres départements ne peuvent pas accéder directement aux équipements de la Direction

---

## 7. Tests Effectués

- Attribution DHCP réussie sur tous les VLANs
- Résolution DNS interne
- Routage inter-VLAN fonctionnel
- Connectivité complète entre Bafoussam et Douala via OSPF
- Tests de restriction d’accès (sécurité)

---

## 8. Recommandations

1. Mettre en place de la redondance (HSRP/VRRP)
2. Implémenter un monitoring centralisé (SolarWinds, PRTG ou Zabbix)
3. Sauvegarde automatique des configurations (TFTP/FTP)
4. Formation continue du personnel réseau
5. Préparer la migration progressive vers IPv6
6. Documentation des configurations dans un outil comme NetBox

---

**Auteur :** JIDJOU Serena  
**Date de livraison :** Juillet 2026