# Phase 4 — Recommandations et Sécurisation

## Priorités de correction

### 🔴 URGENT — Court terme (1-2 semaines)

#### Firmware TP-Link
| Action | Détail |
|---|---|
| Vérifier signature firmware | Ajouter vérification RSA 2048 avant tout upload |
| Désactiver Telnet | Remplacer par SSH avec authentification forte |
| Changer hash MD5 | Utiliser SHA-256 minimum pour les mots de passe |

#### Application Mobile
| Action | Détail |
|---|---|
| Forcer HTTPS | Remplacer http:// par https:// dans DoLogin.java |
| Désactiver debug | android:debuggable="false" dans AndroidManifest |
| Protéger composants | android:exported="false" sur DoTransfer, PostLogin |
| Supprimer /devlogin | Supprimer l'endpoint backdoor développeur |
| Supprimer logs credentials | Supprimer Log.d() qui affiche username:password |

---

### 🟠 IMPORTANT — Moyen terme (1-2 mois)

#### Firmware TP-Link
| Action | Détail |
|---|---|
| Implémenter Secure Boot | Vérification intégrité au démarrage |
| Activer HTTPS | Forcer TLS sur interface d'administration |
| Mettre à jour SDK | Passer à une version récente du noyau Linux |

#### Application Mobile
| Action | Détail |
|---|---|
| SSL Pinning | Vérifier certificat serveur dans MyWebViewClient.java |
| Chiffrement fort | Remplacer Base64 par chiffrement AES-256 |
| Supprimer permissions inutiles | Retirer SEND_SMS, READ_CONTACTS, READ_CALL_LOG |
| Désactiver backup | android:allowBackup="false" |

---

### 🟡 LONG TERME (3-6 mois)

#### Firmware TP-Link
| Action | Détail |
|---|---|
| TPM | Trusted Platform Module pour stockage clés |
| Attestation distante | Vérification firmware depuis serveur central |
| Audit régulier | Pentest firmware tous les 6 mois |

#### Application Mobile
| Action | Détail |
|---|---|
| Mettre à jour SDK | targetSdkVersion minimum 33 (Android 13) |
| MFA | Authentification multi-facteurs |
| Chiffrement bout-en-bout | Chiffrer toutes les communications |

---

## Tableau de priorités global

| Priorité | Action | Délai | Impact |
|---|---|---|---|
| 🔴 P0 | Signature firmware RSA | 1 semaine | Bloque RCE |
| 🔴 P0 | Désactiver Telnet | 3 jours | Bloque accès non chiffré |
| 🔴 P0 | Forcer HTTPS mobile | 3 jours | Bloque MITM |
| 🔴 P0 | Supprimer /devlogin | 1 jour | Supprime backdoor |
| 🔴 P0 | Composants privés | 1 jour | Bloque bypass auth |
| 🟠 P1 | Debuggable OFF | 1 jour | Bloque Frida |
| 🟠 P1 | SSL Pinning | 5 jours | Renforce HTTPS |
| 🟠 P1 | Chiffrement SharedPrefs | 1 semaine | Sécurise données |
| 🟡 P2 | Mise à jour SDK | 1 mois | Protections modernes |
| 🟡 P2 | MFA | 2 mois | Sécurité renforcée |

---

## Impact Business

| Vulnérabilité | Impact Client | Impact Banque |
|---|---|---|
| RCE Firmware | Données interceptées | Perte confiance clients |
| MITM credentials | Vol compte bancaire | Responsabilité légale |
| Bypass auth | Virement frauduleux | Pertes financières |
| Backdoor /devlogin | Accès non autorisé | Violation RGPD |
