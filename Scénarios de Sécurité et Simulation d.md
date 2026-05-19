🔒 Scénarios de Sécurité et Simulation d'Attaque

### Scénario A : Supervision en Temps Réel
L'**Agent Netdata** de la DMZ pousse ses métriques système en continu vers le **SOC**. En cas de pic de charge ou d'anomalie réseau sur le serveur Apache, l'administrateur peut l'identifier instantanément sur le Dashboard centralisé du SOC.

### Scénario B : Attaque et Protection Active
1. La machine **Kali Linux** initie une attaque (ex: force brute SSH ou scan agressif sur le serveur Apache).
2. **Fail2ban** sur le serveur SOC analyse l'activité anormale ou les échecs d'authentification à travers les logs collectés.
3. **Fail2ban** applique automatiquement une règle de blocage pour protéger les services (SSH, Apache...) contre l'adresse IP de l'attaquant.