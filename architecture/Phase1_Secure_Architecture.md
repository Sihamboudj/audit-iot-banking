# Phase 1 — Security by Design

## Architecture VULNÉRABLE (Ce qu'on a trouvé)

<img width="1250" height="672" alt="image" src="https://github.com/user-attachments/assets/d9e94e85-5cb9-46ee-9f73-fbedd3f95f72" />


## Architecture SÉCURISÉE (Recommandée)

<img width="936" height="722" alt="image" src="https://github.com/user-attachments/assets/1d5780d8-25be-4e38-bca5-95258d3b6aa5" />


## Matrice Comparaison

| Aspect | Vulnérable ❌ | Sécurisé ✅ |
|--------|--------------|----------|
| Transport | HTTP | HTTPS TLS 1.3 |
| Credentials | Clair en POST | Hashed + salt |
| Firmware | Non-signé | RSA 2048 signé |
| App Debug | Enabled | Disabled |
| Components | Exportés publics | Privés + auth |
| Pinning | Aucun | ECC P-256 pinning |
| Logging | Plaintext | Sécurisé + anonymisé |

