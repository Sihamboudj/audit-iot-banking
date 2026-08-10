# Phase 1 — Security by Design

## Architecture VULNÉRABLE (Ce qu'on a trouvé)

```mermaid
graph LR
    A[User Phone] -->|HTTP clair| B[App InsecureBank]
    B -->|HTTP credentials| C[API Backend]
    C -->|HTTP| D[TP-Link Router]
    D -->|HTTP/20400| E[Firmware]
    
    style A fill:#ffcccc
    style B fill:#ffcccc
    style C fill:#ffcccc
    style D fill:#ffcccc
    style E fill:#ff6666
    
    F["VULNÉRABILITÉS:<br/>- Pas de HTTPS<br/>- Credentials clair<br/>- Pas SSL pinning<br/>- Upload firmware non-signé<br/>- RCE root possible"]
    style F fill:#ff9999
```
<img width="1250" height="672" alt="image" src="https://github.com/user-attachments/assets/d9e94e85-5cb9-46ee-9f73-fbedd3f95f72" />


## Architecture SÉCURISÉE (Recommandée)

```mermaid
graph LR
    A[User Phone] -->|HTTPS TLS 1.3| B[App InsecureBank]
    B -->|HTTPS + SSL Pinning| C[API Backend]
    C -->|HTTPS + mTLS| D[TP-Link Router]
    D -->|Firmware Signé| E[Firmware Sécurisé]
    
    B1["Sécurité App:<br/>- Debuggable: OFF<br/>- Composants: PRIVÉS<br/>- SharedPrefs: Chiffré<br/>- Backup: DISABLED"]
    C1["Sécurité Backend:<br/>- Input validation<br/>- Rate limiting<br/>- Logging sécurisé"]
    D1["Sécurité Firmware:<br/>- Signature RSA 2048<br/>- Verification OBLIGATOIRE<br/>- Rollback protection"]
    
    style A fill:#ccffcc
    style B fill:#ccffcc
    style C fill:#ccffcc
    style D fill:#ccffcc
    style E fill:#66ff66
```

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

