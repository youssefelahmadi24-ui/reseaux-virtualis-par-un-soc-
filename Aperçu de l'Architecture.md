
## 🔍 Aperçu de l'Architecture

L'infrastructure est entièrement virtualisée sur un hyperviseur **Proxmox VE** (IP : `192.168.6.136`). Le cœur du routage et de la sécurité est confié à une machine virtuelle **pfSense** qui interconnecte quatre zones distinctes (WAN, LAN, DMZ, SOC).

### Schéma Conceptuel

* **Attaquant Extérieur** (VMware / Kali Linux) → Attaque le **WAN** (pfSense)
* **pfSense** → Filtre et distribue les flux vers le **LAN**, la **DMZ** et le **SOC**
* **Serveur DMZ** → Héberge un serveur web Apache surveillé
* **Serveur SOC** → Centralise les métriques de performance et supervise l'activité
* **Client LAN** → Machine utilisateur standard pour les accès légitimes


![Architecture SOC](architecture-soc.jpeg)