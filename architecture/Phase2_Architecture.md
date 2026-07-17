# Architecture IoT Banking — Phase 2
                Internet
                   |
                   |
     +------+------+------+
     |                    |
 User Phone          TP-Link Router
 (InsecureBank)       (Archer C50)
     |                    |
     |                    |
+----+----+          +----+----+
| DoLogin |          | httpd   |
| HTTP    |          | /cgi/   |
| Port ?  |          | Port 80 |
+----+----+          +----+----+
     |                    |
     +----+  Backend  +---+
          |  Server?  |
          |  (Test)   |
          +-----------+

**Flow Authentification** :
1. User → App DoLogin
2. App → HTTP POST username:password
3. Router httpd → /login endpoint
4. Backend response

**Problèmes détectés** :
- HTTP non-chiffré (pas HTTPS)
- Credentials en clair dans POST
- Pas de SSL pinning
- Firmware RCE via /cgi/softup

