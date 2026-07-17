# Phase 2 — MÉTHODOLOGIE

## Outils Utilisés

| Outil | Version | Usage | Résultat |
|-------|---------|-------|----------|
| Binwalk | 2.x | Extraction firmware | ✅ Succès |
| jadx | 1.4.7 | Décompilation APK | ✅ Succès |
| grep/strings | - | Analyse statique | ✅ Succès |

## Étapes Réalisées

### 1. Firmware Analysis
1. Téléchargement TP-Link C50 V4 (7.2 MB)
2. Binwalk extraction → SquashFS + LZMA
3. Analyse filesystem (8 inodes)
4. Identification services (httpd, dropbear)
5. Recherche vulnérabilités (CVE-2024-54126 confirmée)

### 2. Mobile Analysis
1. Téléchargement InsecureBank v2 APK (3.4 MB)
2. jadx décompilation → 14 fichiers .java
3. Analyse classes critiques (DoLogin, DoTransfer, etc.)
4. Extraction permissions AndroidManifest
5. Identification endpoints (/login, /devlogin)

### 3. Vulnérabilités Identifiées
- 3 vulnérabilités firmware (dont 1 RCE critique)
- 9 vulnérabilités mobile (dont 2 critiques)
- Total : 12 vulnérabilités documentées avec CVSS


