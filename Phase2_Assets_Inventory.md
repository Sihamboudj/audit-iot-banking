# Phase 2 — Inventaire Complet des Assets

## Assets Identifiés

### Firmware Assets
| Asset | Type | Détails | Risque |
|-------|------|---------|--------|
| Admin User | Credential | `admin` / MD5 hash | CRITIQUE |
| httpd | Service | Port 80, /cgi/softup | CRITIQUE |
| dropbear | Service | Port 22, SSH | ÉLEVÉ |
| BusyBox | Tool | Interpréteur shell | MOYEN |
| SquashFS | Filesystem | Rootfs complet | MOYEN |

### Mobile Assets
| Asset | Type | Détails | Risque |
|-------|------|---------|--------|
| DoLogin Activity | Component | Credentials HTTP | CRITIQUE |
| DoTransfer Activity | Component | Transferts non-auth | CRITIQUE |
| TrackUserContentProvider | Database | Users publics | ÉLEVÉ |
| SharedPreferences | Storage | Chiffrement faible | ÉLEVÉ |
| mySharedPreferences | File | Base64 username | ÉLEVÉ |

### Network Assets
| Asset | Type | Détails | Risque |
|-------|------|---------|--------|
| HTTP POST | Protocol | Credentials clair | CRITIQUE |
| SSH | Protocol | Accès shell | ÉLEVÉ |
| PostgreSQL | Database | Port 5432 | ÉLEVÉ |

### API Endpoints
| Endpoint | Method | Auth | Risk |
|----------|--------|------|------|
| `/cgi/softup` | POST | Basic Auth | RCE |
| `/login` | POST | None | Credentials |
| `/devlogin` | POST | None | Bypass possible |

