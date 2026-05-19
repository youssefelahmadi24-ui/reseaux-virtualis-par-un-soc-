Plan d'Adressage IP & Segmentation

La segmentation est configurée via des interfaces distinctes portées par le pare-feu pfSense :

| Zone Réseau | Sous-réseau / IP CIDR | IP Interface pfSense | Rôle / Description |
| :--- | :--- | :--- | :--- |
| **WAN** | `192.168.6.0/24` | `192.168.6.138` | Accès Internet / Interface externe exposée |
| **LAN** | `192.168.10.0/24` | `192.168.10.1` | Réseau utilisateur interne et de confiance |
| **DMZ** | `192.168.20.0/24` | `192.168.20.1` | Zone Démilitarisée (Services accessibles publics) |
| **SOC** | `192.168.30.0/24` | `192.168.30.1` | Réseau SOC / Monitoring et supervision |
