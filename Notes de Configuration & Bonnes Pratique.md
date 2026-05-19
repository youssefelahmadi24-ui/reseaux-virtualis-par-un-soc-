 📌 Notes de Configuration & Bonnes Pratiques

* **Segmentation stricte :** Les flux entre les réseaux (WAN / LAN / DMZ / SOC) sont hermétiques par défaut.
* **Moindre privilège :** Seuls les ports strictement nécessaires aux applications (ex: Port 80 pour Apache) sont explicitement autorisés via pfSense.
* **Supervision isolée :** L'usage du streaming Netdata garantit que le tableau de bord de supervision n'est pas exposé directement sur internet ou dans la zone publique (DMZ).