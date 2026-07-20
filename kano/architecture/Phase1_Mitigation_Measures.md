# Phase 1 — Mesures de Mitigation

## Firmware TP-Link C50

**Vulnérabilité** : CVE-2024-54126 (Upload non-signé)
**Impact** : RCE root

| Délai | Action |
|---|---|
| 🔴 Court terme | Vérification signature RSA 2048 + checksum SHA-256 |
| 🟠 Moyen terme | Certificat chiffrement firmware + Secure Boot |
| 🟡 Long terme | TPM + Attestation firmware distante |

## Application Mobile InsecureBank

**Vulnérabilité** : Credentials en HTTP clair
**Impact** : Vol credentials via MITM

| Délai | Action |
|---|---|
| 🔴 Court terme | Forcer HTTPS + TLS 1.3 + SSL Pinning |
| 🟠 Moyen terme | Chiffrement bout-en-bout |

**Vulnérabilité** : App en mode debug
**Impact** : Injection code via Frida

| Délai | Action |
|---|---|
| 🔴 Court terme | android:debuggable="false" |
| 🔴 Court terme | android:allowBackup="false" |

**Vulnérabilité** : SharedPreferences non chiffré
**Impact** : Extraction données facile

| Délai | Action |
|---|---|
| 🟠 Moyen terme | EncryptedSharedPreferences + AndroidKeyStore |

## Tableau de Priorités

| Priorité | Tâche | Délai | Impact |
|---|---|---|---|
| 🔴 P0 | Signature firmware | 1 semaine | Bloque RCE |
| 🔴 P0 | Forcer HTTPS | 3 jours | Bloque MITM |
| 🟠 P1 | Debuggable OFF | 1 jour | Bloque Frida |
| 🟠 P1 | SSL Pinning | 5 jours | Renforce HTTPS |
| 🟡 P2 | Chiffrement SharedPrefs | 1 semaine | Sécurité données |
