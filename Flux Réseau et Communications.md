## 🔀 Flux Réseau et Communications

L'infrastructure s'appuie sur quatre types de flux majeurs, strictement contrôlés par pfSense :

1. **Accès Internet :** Flux sortants pour les mises à jour et accès web légitimes.
2. **Communication Interne (Légitime) :** Flux d'administration ou d'accès autorisés entre les réseaux locaux.
3. **Tentatives d'Attaque (Kali ➔ WAN) :** Flux offensifs simulés ciblant les infrastructures à travers l'interface externe du pare-feu.
4. **Netdata Stream (Agent DMZ ➔ Stream Receiver SOC) :** Flux de monitoring continu permettant d'acheminer les métriques de la DMZ vers le réseau SOC de manière isolée.