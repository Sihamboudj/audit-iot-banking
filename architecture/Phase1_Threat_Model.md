# Phase 1 — Threat Model

## Acteurs Menaces

| Acteur | Capacité | Objectif | Risque |
|--------|----------|----------|--------|
| Attaquant Réseau Local | MITM, ARP spoofing | Vol credentials | CRITIQUE |
| Attaquant Internet | Reverse engineering | Découvrir API | ÉLEVÉ |
| Insider Malveillant | Accès routeur | Obtenir firmware | CRITIQUE |
| App Malveillante | Call exported components | Effectuer transfert | CRITIQUE |

## Scénarios Attaque → Mitigation

### Scénario 1 : RCE Firmware
**Attaque** : Upload firmware malveillant via `/cgi/softup`  
**Mitigation** : Vérifier signature RSA avant flasher  
**Coût** : ~20 heures dev  

### Scénario 2 : Capture Credentials
**Attaque** : MITM HTTP POST `/login`  
**Mitigation** : HTTPS + TLS pinning  
**Coût** : ~15 heures dev  

### Scénario 3 : Bypass Authentification
**Attaque** : Appeler DoTransfer directement (exported)  
**Mitigation** : Rendre privé + ajouter permission check  
**Coût** : ~5 heures dev  

