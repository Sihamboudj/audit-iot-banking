# Phase 3 — Threat Model STRIDE

## Méthodologie
STRIDE est une méthode d'analyse des menaces qui classe
chaque vulnérabilité selon son type d'attaque.

## Analyse STRIDE — Firmware TP-Link C50 V4

| ID | Vulnérabilité | S | T | R | I | D | E | CVSS |
|---|---|---|---|---|---|---|---|---|
| VULN-FW-01 | Firmware non signé (RCE) | | ✅ | | | | ✅ | 8.5 🔴 |
| VULN-FW-02 | Hash mot de passe exposé | ✅ | | | ✅ | | ✅ | 7.5 🟠 |
| VULN-FW-03 | Telnet activé par défaut | ✅ | | | ✅ | | | 9.0 🔴 |
| VULN-FW-04 | Admin = root (UID 0) | | | | | | ✅ | 8.0 🟠 |

## Analyse STRIDE — Application Mobile InsecureBank v2

| ID | Vulnérabilité | S | T | R | I | D | E | CVSS |
|---|---|---|---|---|---|---|---|---|
| MOB-01 | App debuggable | | | | ✅ | | ✅ | 8.0 🔴 |
| MOB-02 | Backup activé | | | | ✅ | | | 6.0 🟠 |
| MOB-03 | Composants exportés | ✅ | | ✅ | | | ✅ | 9.0 🔴 |
| MOB-04 | Permissions excessives | | | | ✅ | | | 5.0 🟡 |
| MOB-05 | SDK obsolète | | | | | | ✅ | 6.5 🟠 |
| MOB-06 | HTTP en clair | ✅ | | | ✅ | | | 8.5 🔴 |
| MOB-07 | Backdoor /devlogin | ✅ | | ✅ | | | ✅ | 9.5 🔴 |
| MOB-08 | Credentials dans logs | | | ✅ | ✅ | | | 7.0 🟠 |
| MOB-09 | Username Base64 | | | | ✅ | | | 5.5 🟡 |

## 3 Scénarios d'attaque les plus critiques

### Scénario 1 — RCE via Upload Firmware (CVSS 8.5)
- **Type STRIDE** : Tampering + Elevation of Privilege
- **Étapes** :
  1. Attaquant crée un firmware malveillant
  2. Upload via /cgi/softup sans vérification
  3. Routeur installe le firmware
  4. Attaquant obtient accès root total
- **Impact** : Contrôle total du réseau bancaire

### Scénario 2 — Vol credentials via MITM (CVSS 8.5)
- **Type STRIDE** : Spoofing + Information Disclosure
- **Étapes** :
  1. Attaquant sur le même réseau
  2. Intercepte le trafic HTTP avec Wireshark
  3. Lit username et password en clair
  4. Se connecte au compte bancaire
- **Impact** : Vol de données bancaires

### Scénario 3 — Bypass authentification (CVSS 9.5)
- **Type STRIDE** : Spoofing + Repudiation + Elevation of Privilege
- **Étapes** :
  1. App malveillante installée sur le téléphone
  2. Appelle DoTransfer directement (exported=true)
  3. Effectue un virement sans se connecter
  4. Aucune trace dans les logs
- **Impact** : Virement frauduleux sans authentification

## Priorités

| Priorité | Scénario | Action immédiate |
|---|---|---|
| 🔴 P0 | RCE Firmware | Vérifier signature RSA avant upload |
| 🔴 P0 | MITM credentials | Forcer HTTPS + SSL Pinning |
| 🔴 P0 | Bypass auth | Rendre composants privés |
