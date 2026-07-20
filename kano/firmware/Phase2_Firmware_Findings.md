# Phase 2 — Analyse Firmware TP-Link Archer C50 V4

## Informations Firmware

| Champ | Valeur |
|---|---|
| Modèle | TP-Link Archer C50 V4 |
| Version | 0.9.1_0.2 (250117) |
| Bootloader | U-Boot 1.1.3 (17 Jan 2025) |
| Filesystem | SquashFS 4.0 (XZ) |
| Inodes | 812 |
| Signature | ❌ Absente |

## Outil utilisé
- Binwalk 2.x — extraction et analyse statique

## Commandes exécutées

### 1. Analyse structure
```bash
binwalk firmware.bin
```

### 2. Extraction filesystem
```bash
binwalk -Me firmware.bin
```

### 3. Analyse fichiers sensibles
```bash
cat squashfs-root/etc/passwd.bak
cat squashfs-root/etc/init.d/rcS
```

## Vulnérabilités Identifiées

### VULN-01 — Firmware non signé (CVE-2024-54126)
- **Description** : Aucune signature RSA dans le firmware
- **Impact** : Upload firmware malveillant possible → RCE root
- **CVSS** : 8.5 (Critique)
- **Preuve** : Binwalk ne détecte aucun header de signature

### VULN-02 — Hash mot de passe exposé
- **Description** : Hash SHA-256 du compte admin visible dans passwd.bak
- **Hash trouvé** : $5$CdwYcBZ/FaI10oN2$oyxBoCW4r8CzvVoVIT91Ncq8Ox5luDDMfdKPCC7mA57
- **Impact** : Attaque par dictionnaire possible → compromission admin
- **CVSS** : 7.5 (Élevé)

### VULN-03 — Telnet activé par défaut
- **Description** : Service telnetd lancé au boot sans restriction
- **Preuve** : Ligne "telnetd" dans /etc/init.d/rcS
- **Impact** : Credentials transitent en clair sur le réseau
- **CVSS** : 9.0 (Critique)

### VULN-04 — Admin = root (UID 0)
- **Description** : Compte admin a les privilèges root (UID=0)
- **Preuve** : admin:$5$...:0:0:root:/:/bin/sh
- **Impact** : Compromission admin = accès root total
- **CVSS** : 8.0 (Élevé)

## Résumé

| ID | Vulnérabilité | CVSS | Criticité |
|---|---|---|---|
| VULN-01 | Firmware non signé | 8.5 | 🔴 Critique |
| VULN-02 | Hash mot de passe exposé | 7.5 | 🟠 Élevé |
| VULN-03 | Telnet activé par défaut | 9.0 | 🔴 Critique |
| VULN-04 | Admin = root | 8.0 | 🟠 Élevé |
