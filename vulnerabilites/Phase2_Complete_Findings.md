# Phase 2 — FINDINGS COMPLETS

## 1. FIRMWARE (TP-Link Archer C50 V4)

### Architecture & Infrastructure
- **Architecture** : MIPS mipsel (identifiée via Binwalk)
- **Filesystem** : SquashFS (xz compression)
- **Bootloader** : LZMA compressed kernel
- **Extraction** : Complète via Binwalk

### Services Identifiés
| Service | Port | Type | Vulnérabilité |
|---------|------|------|---------------|
| httpd | 80/443 | HTTP/HTTPS | Pas de SSL pinning, HTTP Basic Auth faible |
| dropbear | 22 | SSH | Compte système sans restrictions |
| BusyBox | - | Shell | Utilisé par linuxrc |

### Credentials Trouvés
Admin User: admin
Hash: 11
1$iC.dUsGpxNNJGeOm1dFio/ (MD5 crypt)
Location: /etc/passwd.bak
Status: Hardcoded dans le firmware

### Vulnérabilités Firmware

#### VULN-FW-01 : CVE-2024-54126 — Upload Firmware Non Vérifié (RCE)
- **Endpoint** : `/cgi/softup` (POST multipart/form-data)
- **Page Frontend** : `/web/main/softup.htm`
- **Vérification Côté Client** : Regex `.bin` uniquement
- **Vérification Côté Serveur** : AUCUNE (pas de signature, pas de checksum)
- **Binaire Responsable** : `/usr/bin/httpd` (157 KB)
- **Impact** : RCE avec privilèges root
- **CVSS v3.1** : 8.5 (AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H)

#### VULN-FW-02 : Pas de HTTPS Enforced
- **Protocol par défaut** : HTTP (non-chiffré)
- **Authentification** : HTTP Basic Auth (Base64)
- **Risque** : Interception des credentials en clair
- **CVSS v3.1** : 6.5

#### VULN-FW-03 : Credentials En Dur
- **Utilisateur** : admin
- **Hash** : Stocké en clair dans passwd.bak
- **Faiblesse** : MD5 (algorithme obsolète)
- **CVSS v3.1** : 7.2

### Fichiers Clés Analysés
- `/web/main/softup.htm` — formulaire upload
- `/usr/bin/httpd` — serveur web
- `/etc/passwd.bak` — credentials
- `/etc/init.d/` — scripts démarrage

---

## 2. MOBILE (InsecureBank v2)

### Application Info
- **Package** : `com.android.insecurebankv2`
- **Min SDK** : 15
- **Target SDK** : 22
- **Debuggable** : YES ⚠️
- **Backup Enabled** : YES ⚠️

### Permissions Dangereuses
```xml
android.permission.SEND_SMS              ⚠️ Peut envoyer des SMS
android.permission.USE_CREDENTIALS       ⚠️ Accès aux credentials
android.permission.GET_ACCOUNTS          ⚠️ Liste des comptes
android.permission.READ_CONTACTS         ⚠️ Accès aux contacts
android.permission.READ_CALL_LOG         ⚠️ Historique d'appels
android.permission.ACCESS_COARSE_LOCATION ⚠️ Géolocalisation
android.permission.WRITE_EXTERNAL_STORAGE ⚠️ Écriture disque
android.permission.INTERNET              ⚠️ Accès réseau
```

### Vulnérabilités Mobile

#### VULN-M-01 : HTTP Non-Chiffré
- **Code** : `String protocol = "http://"`
- **Classe** : DoLogin.java:17
- **Impact** : Credentials en clair sur le réseau
- **CVSS v3.1** : 7.5

#### VULN-M-02 : Credentials en POST Clair
- **Code** : 
```java
nameValuePairs.add(new BasicNameValuePair("username", username));
nameValuePairs.add(new BasicNameValuePair("password", password));
```
- **Classe** : DoLogin.java:52-53
- **Impact** : Capture facile via MITM
- **CVSS v3.1** : 8.1

#### VULN-M-03 : Logging des Credentials
- **Code** : `Log.d("Successful Login:", account=username:password)`
- **Classe** : DoLogin.java:65
- **Impact** : Credentials visibles dans logcat
- **CVSS v3.1** : 7.8

#### VULN-M-04 : SharedPreferences Non-Chiffré
- **Fichier** : `mySharedPreferences`
- **Stockage** : Base64(username) + AES(password) — chiffrement faible
- **Classe** : DoLogin.java:85-90
- **Impact** : Extraction facile via extraction APK
- **CVSS v3.1** : 7.2

#### VULN-M-05 : Pas de SSL Pinning
- **Classe** : MyWebViewClient.java
- **Code** : Accepte tous certificats (override vide)
- **Impact** : MITM possible
- **CVSS v3.1** : 6.5

#### VULN-M-06 : App Debuggable
- **Manifest** : `android:debuggable="true"`
- **Impact** : Injection de code, reverse via Frida
- **CVSS v3.1** : 7.1

#### VULN-M-07 : Composants Exportés Sans Protection
```xml
<activity ... android:exported="true">
  - PostLogin
  - DoTransfer
  - ViewStatement
  - ChangePassword
<provider ... android:exported="true">
  - TrackUserContentProvider
<receiver ... android:exported="true">
  - MyBroadCastReceiver
```
- **Impact** : N'importe quelle app peut lancer ces activités
- **CVSS v3.1** : 7.6

#### VULN-M-08 : Content Provider Ouvert
- **URI** : `content://com.android.insecurebankv2.TrackUserContentProvider/trackerusers`
- **Données** : Liste des utilisateurs loggés
- **Classe** : TrackUserContentProvider.java
- **Impact** : N'importe quelle app peut lire les utilisateurs
- **CVSS v3.1** : 6.8

#### VULN-M-09 : Backup Enabled
- **Manifest** : `android:allowBackup="true"`
- **Impact** : Extraction de données via `adb backup`
- **CVSS v3.1** : 6.2

### Endpoints API Identifiés
/login      — authentification standard
/devlogin   — account développeur (bypass possible)

### Fichiers Critiques Analysés
- `DoLogin.java` (197 lignes) — authentification
- `DoTransfer.java` (336 lignes) — transferts
- `TrackUserContentProvider.java` (123 lignes) — données exposées
- `MyWebViewClient.java` (12 lignes) — gestion SSL
- `CryptoClass.java` (54 lignes) — chiffrement faible

---

## 3. RÉSUMÉ COMPLET DES VULNÉRABILITÉS

### Critiques (CVSS > 8)
- CVE-2024-54126 (Firmware RCE) — 8.5
- Credentials HTTP POST — 8.1

### Élevées (CVSS 6.5-8)
- Logging credentials — 7.8
- Composants exportés — 7.6
- HTTP non-chiffré — 7.5
- Credentials SharedPreferences — 7.2
- App debuggable — 7.1
- Content Provider ouvert — 6.8

### Moyennes (CVSS < 6.5)
- Pas SSL pinning — 6.5
- Backup enabled — 6.2

---

## 4. ASSETS ET DONNÉES SENSIBLES

### Firmware
- Admin credentials (MD5 hash)
- Service httpd (port 80)
- Service dropbear (port 22)

### Mobile
- Username/Password utilisateurs
- Historique des logins (Content Provider)
- Contacts, SMS, location (permissions)

---

## 5. MÉTHODOLOGIE PHASE 2

| Tâche | Statut | Détails |
|-------|--------|---------|
| Binwalk extraction | ✅ | Firmware extrait, FS complet |
| Services identifiés | ✅ | httpd, dropbear, BusyBox |
| Credentials trouvés | ✅ | Admin hash MD5 |
| APK décompilation | ✅ | Jadx complet, 14 fichiers .java |
| Endpoints API | ✅ | /login, /devlogin trouvés |
| Permissions Android | ✅ | 8 permissions dangereuses |
| Composants exportés | ✅ | 7 composants non-protégés |
| QEMU émulation | ❌ | Firmadyne incompatible |
| nmap ports | ❌ | Non-critique (services identifiés) |

