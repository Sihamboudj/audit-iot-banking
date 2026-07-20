# Phase 2 — Analyse Mobile InsecureBank v2

## Informations APK

| Champ | Valeur |
|---|---|
| Package | com.android.insecurebankv2 |
| Version | 1.0 |
| SDK cible | Android 5.1.1 (API 22) |
| Taille | 3.3 MB |
| Outil utilisé | jadx 1.4.7 |

## Commandes exécutées

### 1. Téléchargement APK
```bash
wget https://github.com/dineshshetty/Android-InsecureBankv2/raw/master/InsecureBankv2.apk
```

### 2. Décompilation
```bash
jadx -d insecurebank-decompiled insecurebank.apk
```

### 3. Analyse AndroidManifest
```bash
cat insecurebank-decompiled/resources/AndroidManifest.xml
```

### 4. Analyse code source
```bash
cat insecurebank-decompiled/sources/com/android/insecurebankv2/DoLogin.java
```

## Vulnérabilités Identifiées

### MOB-01 — App en mode Debug
- **Fichier** : AndroidManifest.xml
- **Preuve** : android:debuggable="true"
- **Impact** : Injection code via Frida, lecture mémoire, bypass auth
- **CVSS** : 8.0 🔴

### MOB-02 — Backup activé
- **Fichier** : AndroidManifest.xml
- **Preuve** : android:allowBackup="true"
- **Impact** : Extraction complète des données via USB
- **CVSS** : 6.0 🟠

### MOB-03 — Composants exportés sans protection
- **Fichier** : AndroidManifest.xml
- **Preuve** :
  - DoTransfer → exported="true"
  - PostLogin → exported="true"
  - ViewStatement → exported="true"
  - ChangePassword → exported="true"
- **Impact** : Virement possible sans authentification
- **CVSS** : 9.0 🔴

### MOB-04 — Permissions excessives
- **Fichier** : AndroidManifest.xml
- **Preuve** : SEND_SMS, READ_CONTACTS, READ_CALL_LOG, ACCESS_COARSE_LOCATION
- **Impact** : Collecte données personnelles non nécessaires
- **CVSS** : 5.0 🟡

### MOB-05 — SDK obsolète
- **Fichier** : AndroidManifest.xml
- **Preuve** : targetSdkVersion="22" (Android 5.1 - 2015)
- **Impact** : Protections modernes Android non appliquées
- **CVSS** : 6.5 🟠

### MOB-06 — HTTP en clair
- **Fichier** : DoLogin.java
- **Preuve** : String protocol = "http://"
- **Impact** : Credentials interceptables via MITM
- **CVSS** : 8.5 🔴

### MOB-07 — Endpoint /devlogin (Backdoor)
- **Fichier** : DoLogin.java
- **Preuve** : HttpPost("/devlogin") si username = "devadmin"
- **Impact** : Accès admin via porte dérobée
- **CVSS** : 9.5 🔴

### MOB-08 — Credentials dans les logs
- **Fichier** : DoLogin.java
- **Preuve** : Log.d("Successful Login:", "account=" + username + ":" + password)
- **Impact** : Mot de passe visible dans les logs Android
- **CVSS** : 7.0 🟠

### MOB-09 — Username en Base64
- **Fichier** : DoLogin.java
- **Preuve** : Base64.encodeToString(username.getBytes())
- **Impact** : Encodage ≠ chiffrement, facilement décodable
- **CVSS** : 5.5 🟡

## Résumé

| ID | Vulnérabilité | CVSS | Criticité |
|---|---|---|---|
| MOB-01 | App debuggable | 8.0 | 🔴 Critique |
| MOB-02 | Backup activé | 6.0 | 🟠 Élevé |
| MOB-03 | Composants exportés | 9.0 | 🔴 Critique |
| MOB-04 | Permissions excessives | 5.0 | 🟡 Moyen |
| MOB-05 | SDK obsolète | 6.5 | 🟠 Élevé |
| MOB-06 | HTTP en clair | 8.5 | 🔴 Critique |
| MOB-07 | Backdoor /devlogin | 9.5 | 🔴 Critique |
| MOB-08 | Credentials dans logs | 7.0 | 🟠 Élevé |
| MOB-09 | Username Base64 | 5.5 | 🟡 Moyen |
