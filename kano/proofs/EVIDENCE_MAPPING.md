# Kano — Mapping Vulnérabilités ↔ Preuves

## Mobile (InsecureBank v2)

| ID | Vulnérabilité | Preuve Code | Ligne |
|---|---|---|---|
| M-01 | HTTP en clair | String protocol = "http://" | DoLogin.java:51 |
| M-02 | Credentials en POST | BasicNameValuePair("password", password) | DoLogin.java:102 |
| M-03 | Credentials dans logs | Log.d("Successful Login:", account=username:password) | DoLogin.java:115 |
| M-04 | App debuggable | android:debuggable="true" | AndroidManifest.xml:32 |
| M-05 | Backup activé | android:allowBackup="true" | AndroidManifest.xml:33 |
| M-06 | Composants exportés | android:exported="true" sur DoTransfer, PostLogin | AndroidManifest.xml:52-78 |
| M-07 | Backdoor /devlogin | HttpPost("/devlogin") si username=devadmin | DoLogin.java:99 |

## Firmware (TP-Link Archer C50 V4)

| ID | Vulnérabilité | Preuve | Fichier |
|---|---|---|---|
| FW-01 | Firmware non signé | Binwalk ne détecte aucune signature RSA | binwalk firmware.bin |
| FW-02 | Hash admin exposé | admin:$5$CdwYcBZ...0:0:root:/:/bin/sh | /etc/passwd.bak |
| FW-03 | Telnet activé | telnetd dans script de démarrage | /etc/init.d/rcS |
| FW-04 | Admin = root | UID=0 sur compte admin | /etc/passwd.bak |

## Commandes utilisées pour obtenir les preuves

### Mobile
```bash
grep -n "protocol" DoLogin.java
# → ligne 51: String protocol = "http://"

grep -n "BasicNameValuePair" DoLogin.java  
# → ligne 102: password envoyé en clair

grep -n "Log.d" DoLogin.java
# → ligne 115: username:password dans les logs

grep -n "debuggable\|allowBackup" AndroidManifest.xml
# → ligne 32-33: debug=true, backup=true

grep -n "exported" AndroidManifest.xml
# → 6 composants exportés sans protection
```

### Firmware
```bash
binwalk firmware.bin
# → Aucune signature RSA détectée

cat /etc/passwd.bak
# → admin hash SHA-256 exposé, UID=0

cat /etc/init.d/rcS
# → telnetd lancé au boot
```
