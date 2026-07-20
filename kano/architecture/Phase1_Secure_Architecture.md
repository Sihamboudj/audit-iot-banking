# Phase 1 — Architecture Sécurisée

## Architecture VULNÉRABLE (Ce qu'on a trouvé)

| Couche | Composant | Problème |
|---|---|---|
| Edge | App InsecureBank | HTTP en clair, credentials non chiffrés |
| Gateway | TP-Link Archer C50 V4 | Firmware non signé, RCE possible |
| Cloud | Serveur bancaire | APIs non sécurisées |

## Architecture SÉCURISÉE (Recommandée)

| Couche | Composant | Solution |
|---|---|---|
| Edge | App InsecureBank | HTTPS TLS 1.3 + SSL Pinning |
| Gateway | TP-Link Archer C50 V4 | Firmware signé RSA 2048 |
| Cloud | Serveur bancaire | mTLS + authentification forte |

## Comparaison

| Aspect | Vulnérable ❌ | Sécurisé ✅ |
|---|---|---|
| Transport | HTTP | HTTPS TLS 1.3 |
| Firmware | Non signé | RSA 2048 signé |
| App Debug | Activé | Désactivé |
| Données | En clair | Chiffrées |
| SSL Pinning | Aucun | ECC P-256 |
