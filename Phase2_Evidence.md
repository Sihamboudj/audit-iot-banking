# Phase 2 — EVIDENCE (Extraits de Code)

## FW-01 : CVE-2024-54126 — Pas de vérification signature

**Fichier** : `/usr/bin/httpd`  
**Strings trouvées** : Aucune mention de "verify", "signature", "check"
```bash
strings httpd | grep -iE "signature|verify|check"
# Retour : AUCUN résultat → vulnérabilité confirmée
```

---

## M-02 : Credentials en HTTP POST

**Fichier** : `DoLogin.java` ligne 52-53
```java
nameValuePairs.add(new BasicNameValuePair("username", DoLogin.this.username));
nameValuePairs.add(new BasicNameValuePair("password", DoLogin.this.password));
```
**Protocole** : `String protocol = "http://";` (ligne 17)

---

## M-03 : Logging des Credentials

**Fichier** : `DoLogin.java` ligne 65
```java
Log.d("Successful Login:", ", account=" + DoLogin.this.username + ":" + DoLogin.this.password);
```

---

## M-06 : App Debuggable

**Fichier** : `AndroidManifest.xml`
```xml
<application android:debuggable="true" ...>
```

---

## M-07 : Composants Exportés

**Fichier** : `AndroidManifest.xml`
```xml
<activity ... android:name="com.android.insecurebankv2.PostLogin" android:exported="true"/>
<activity ... android:name="com.android.insecurebankv2.DoTransfer" android:exported="true"/>
<provider android:name="com.android.insecurebankv2.TrackUserContentProvider" android:exported="true"/>
```

