# Phase 3 — STRIDE & CVSS Scoring

## Vecteurs d'Entrée Identifiés

### 1. FIRMWARE (TP-Link Archer C50 V4)

| ID | Vecteur | STRIDE | Description | CVSS | Priorité |
|----|---------|--------|-------------|------|----------|
| FW-01 | `/cgi/softup` POST | Tampering (T) | CVE-2024-54126 : Upload firmware sans signature → RCE root | **8.5** | 🔴 CRITIQUE |
| FW-02 | HTTP 80/443 | Tampering (T) + Information Disclosure (I) | HTTP non-chiffré, Basic Auth en clair | **6.5** | 🟠 ÉLEVÉ |
| FW-03 | passwd.bak | Information Disclosure (I) | Credentials admin (MD5 hash) en dur | **7.2** | 🟠 ÉLEVÉ |

### 2. MOBILE (InsecureBank v2)

| ID | Vecteur | STRIDE | Description | CVSS | Priorité |
|----|---------|--------|-------------|------|----------|
| M-01 | HTTP POST `/login` | Tampering (T) + I.D (I) | Credentials en clair dans HTTP POST | **8.1** | 🔴 CRITIQUE |
| M-02 | Logcat logs | Information Disclosure (I) | Credentials loggés en clair via Log.d() | **7.8** | 🔴 ÉLEVÉ |
| M-03 | SharedPreferences | Information Disclosure (I) | Fichier non-chiffré (Base64 username + AES faible) | **7.2** | 🟠 ÉLEVÉ |
| M-04 | WebView | Tampering (T) | Pas de SSL pinning, accept tous certificats | **6.5** | 🟠 MOYEN |
| M-05 | Activités exportées | Elevation of Privilege (E) | 7 composants sans protection (DoTransfer, PostLogin, etc.) | **7.6** | 🔴 ÉLEVÉ |
| M-06 | ContentProvider | Information Disclosure (I) | TrackUserContentProvider public → liste utilisateurs | **6.8** | 🟠 MOYEN |
| M-07 | Debuggable=true | Elevation of Privilege (E) | Injection code via Frida/debugger | **7.1** | 🟠 ÉLEVÉ |
| M-08 | Backup enabled | Information Disclosure (I) | Extraction données via `adb backup` | **6.2** | 🟠 MOYEN |

---

## Matrice STRIDE Complète
Spoofing (S)          : M-05 (activités exportées permettent usurpation)
Tampering (T)         : FW-01, FW-02, M-01, M-04
Repudiation (R)       : Faible
Information Disclosure(I) : FW-03, M-02, M-03, M-06, M-08
Denial of Service (D) : FW-01 (reboot possible via firmware malveillant)
Elevation of Privilege(E) : M-05, M-07

---

## TOP 3 — Scénarios Critiques à Exploiter

### 🔴 SCÉNARIO 1 : CVE-2024-54126 — Firmware RCE (CVSS 8.5)

**Vecteur d'attaque** : `/cgi/softup` (POST multipart/form-data)

**Étapes exploitation** :
1. Créer binaire malveillant `.bin` (ARM MIPS, shellcode)
2. Upload vers `http://192.168.0.1/cgi/softup` avec auth Basic (admin:password)
3. Routeur redémarre et exécute le payload
4. Attaquant = root du routeur

**Impact** : 
- Accès total au routeur
- Interception Wi-Fi
- MITM sur trafic réseau
- Accès à l'infrastructure cloud

**Preuve** : httpd n'appelle jamais verify_signature()

---

### 🔴 SCÉNARIO 2 : Credentials HTTP POST en Clair (CVSS 8.1)

**Vecteur d'attaque** : Réseau (MITM)

**Étapes exploitation** :
1. Se positionner en MITM (ARP spoofing, WiFi malveillant, etc.)
2. Capturer trafic HTTP vers `/login`
3. Extraire username:password en clair
4. Accès directement à l'app (les credentials sont les mêmes)

**Impact** :
- Vol de credentials bancaires (contexte)
- Accès à tous les transferts de l'utilisateur
- Escalade vers routeur (combo avec FW-01)

**Preuve** : `new BasicNameValuePair("password", password)` ligne 53

---

### 🔴 SCÉNARIO 3 : Activités Exportées + Debuggable = Injection Code (CVSS 7.6 + 7.1)

**Vecteur d'attaque** : Application Android

**Étapes exploitation** :
1. Installer malicious app sur le téléphone
2. Appeler `PostLogin` ou `DoTransfer` directement (exportées)
3. Bypass authentification, effectuer transfert direct
4. OU utiliser Frida (debuggable=true) pour injector du code

**Impact** :
- Transferts bancaires non-autorisés
- Exécution de code avec permissions app (SMS, location, etc.)

**Preuve** : `android:exported="true"` sur DoTransfer + `android:debuggable="true"`

---

## Justification des 3 Priorités

| Scénario | CVSS | Raison | Impact Business |
|----------|------|--------|-----------------|
| **1. Firmware RCE** | 8.5 | Accès root total, non-contrable | Perte totale du routeur |
| **2. Credentials HTTP** | 8.1 | Acquisition directe secrets | Usurpation identité utilisateur |
| **3. Activities Export** | 7.6+7.1 | Exécution privilèges app | Fraude bancaire directe |

---

## Autres Vulnérabilités (à tracker mais pas exploiter live)

- FW-02 : HTTP non-chiffré (mitigation : activer HTTPS)
- FW-03 : Credentials en dur (mitigation : changer mot de passe)
- M-02, M-03 : Logging + SharedPrefs (mitigation : supprimer logs, chiffrer)
- M-04 : Pas SSL pinning (mitigation : implémenter pinning)
- M-06, M-08 : Backup + ContentProvider (mitigation : désactiver)


---

## EXECUTIVE SUMMARY

**Total Vulnérabilités Identifiées** : 12  
**Critiques (CVSS ≥ 8)** : 2  
**Élevées (CVSS 6.5-8)** : 7  
**Moyennes (CVSS < 6.5)** : 3  

**Top 3 Attacks** : 
1. CVE-2024-54126 (Firmware RCE, 8.5)
2. HTTP Credentials (8.1)
3. Exported Activities (7.6)

**Risk Level** : 🔴 **CRITIQUE** (Score moyen 7.2)

