# Phase 1 — Threat Model

## Acteurs Menaces

| Acteur | Objectif | Risque |
|---|---|---|
| Attaquant réseau local | Vol credentials via MITM | CRITIQUE |
| Attaquant internet | Reverse engineering API | ÉLEVÉ |
| Insider malveillant | Accès firmware routeur | CRITIQUE |
| App malveillante | Bypass authentification | CRITIQUE |

## Scénarios d'attaque

### Scénario 1 — RCE Firmware
- **Attaque** : Upload firmware malveillant sur le routeur
- **Impact** : Contrôle total du routeur
- **Mitigation** : Vérifier signature RSA avant flasher

### Scénario 2 — Vol de credentials
- **Attaque** : Intercepter le trafic HTTP (MITM)
- **Impact** : Voler mots de passe bancaires
- **Mitigation** : Forcer HTTPS + SSL Pinning

### Scénario 3 — Bypass authentification
- **Attaque** : Appeler directement DoTransfer sans se connecter
- **Impact** : Faire un virement sans authentification
- **Mitigation** : Rendre les composants privés + vérification permissions
