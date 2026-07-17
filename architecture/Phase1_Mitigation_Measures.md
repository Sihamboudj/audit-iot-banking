# Phase 1 — Mesures de Mitigation

## Par Domaine

### Firmware (TP-Link C50)

**Vulnérabilité** : CVE-2024-54126 (Upload non-signé)
**Impact** : RCE root
**Mitigation Court Terme** (1-2 semaines)
- Ajouter vérification signature RSA 2048 dans httpd
- Implémenter checksum SHA-256 obligatoire
- Rejeter firmware non-signé

**Mitigation Moyen Terme** (1-2 mois)
- Certificat chiffrement du firmware
- Signature digitale des binaires
- Secure boot implementation

**Mitigation Long Terme** (3-6 mois)
- TPM (Trusted Platform Module)
- UEFI Secure Boot
- Attestation firmware distante

---

### Mobile (InsecureBank v2)

**Vulnérabilité** : HTTP credentials en clair
**Impact** : Vol credentials via MITM
**Mitigation Court Terme** (1 semaine)
- Forcer HTTPS uniquement
- Activer TLS 1.3 minimum
- Ajouter SSL pinning (Certificate pinning)

**Vulnérabilité** : App debuggable
**Impact** : Injection code via Frida
**Mitigation Court Terme** (1 jour)
- android:debuggable="false"
- Désactiver backup (android:allowBackup="false")
- Rendre composants privés (android:exported="false")

**Vulnérabilité** : SharedPreferences non-chiffré
**Impact** : Extraction données facilement
**Mitigation Moyen Terme** (2 semaines)
- Encrypter avec EncryptedSharedPreferences (Android Security)
- Utiliser AndroidKeyStore pour clés
- Chiffrement bout-en-bout

---

## Priorités Implémentation

| Priorité | Tâche | Temps | Impact |
|----------|-------|-------|--------|
| 🔴 P0 | Firmware signature | 1 sem | Bloque RCE |
| 🔴 P0 | Forcer HTTPS | 3 jours | Bloque MITM |
| 🟠 P1 | Debuggable OFF | 1 jour | Bloque Frida |
| 🟠 P1 | SSL Pinning | 5 jours | Renforce HTTPS |
| 🟠 P1 | Composants privés | 2 jours | Bloque bypass |
| 🟡 P2 | Chiffrement SharedPrefs | 1 sem | Sécurité données |

---

## Contraintes Réelles

**Coût** :
- Développement : ~120 heures (3 devs * 4 semaines)
- Infrastructure : ~5k€ (certificats, HSM, etc.)
- Testing : ~40 heures

**Complexité** :
- Firmware : Modification binaire + recompilation ARM
- Mobile : Integration AndroidKeyStore + testing cross-platform
- Backend : Refactor authentification + certificats

**Facteur Humain** :
- Formations sécurité requises
- Code review obligatoire
- Security testing en CI/CD

