# Audit Sécurité IoT — Banking
## Tableau des Vulnérabilités Identifiées

| ID | Composant | Vecteur d'Attaque | STRIDE | Description | CVSS | Priorité |
|----|-----------|-------------------|--------|-------------|------|----------|
| FW-01 | Firmware TP-Link C50 | Upload Firmware Non Signé | Tampering | CVE-2024-54126 : RCE via /cgi/softup | 8.5 | 🔴 CRITIQUE |
| M-02 | Mobile HTTP POST | Credentials en Clair | Tampering | Username:password en HTTP POST | 8.1 | 🔴 CRITIQUE |
| M-03 | Mobile Logging | Credentials Loggés | Information Disclosure | Log.d() expose username:password | 7.8 | 🔴 ÉLEVÉ |
| M-07 | Mobile Composants | Activités Exportées | Elevation of Privilege | 7 composants sans protection | 7.6 | 🔴 ÉLEVÉ |
| M-01 | Mobile HTTP | Protocol Non-Chiffré | Tampering | String protocol = "http://" | 7.5 | 🔴 ÉLEVÉ |
| FW-03 | Firmware Credentials | Credentials En Dur | Information Disclosure | Admin hash MD5 en passwd.bak | 7.2 | 🟠 ÉLEVÉ |
| M-04 | Mobile SharedPrefs | Stockage Non-Chiffré | Information Disclosure | Base64 username + AES faible | 7.2 | 🟠 ÉLEVÉ |
| M-06 | Mobile Debuggable | App Debuggable=true | Elevation of Privilege | Injection code via Frida | 7.1 | 🟠 ÉLEVÉ |
| M-08 | Mobile ContentProvider | Provider Ouvert | Information Disclosure | Accès public aux utilisateurs | 6.8 | 🟠 ÉLEVÉ |
| M-05 | Mobile SSL | Pas de SSL Pinning | Tampering | Accept tous certificats | 6.5 | 🟠 MOYEN |
| FW-02 | Firmware HTTPS | Pas de HTTPS Enforced | Tampering | HTTP Basic Auth en clair | 6.5 | 🟠 MOYEN |
| M-09 | Mobile Backup | Backup Enabled=true | Information Disclosure | Extraction via adb backup | 6.2 | 🟠 MOYEN |

---

## Détails par Vulnérabilité

### FW-01 : CVE-2024-54126 (8.5 CRITIQUE)
**Endpoint** : `/cgi/softup` (POST)  
**Cause** : Aucune vérification de signature firmware  
**Impact** : RCE root  
**Preuve** : `/usr/bin/httpd` n'appelle pas verify_signature()

### M-02 : Credentials HTTP POST (8.1 CRITIQUE)
**Classe** : DoLogin.java:52-53  
**Cause** : Protocol HTTP + nameValuePairs en clair  
**Impact** : Capture MITM  
**Preuve** : `new BasicNameValuePair("password", password)`

### [Autres détails dans Phase2_Complete_Findings.md]

