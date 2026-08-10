# Audit de sécurité IoT — Domaine Banking
Équipe : Mehdi BOUAZRA, Siham BOUDJAIDI, Kano COULIBALY, Maxime VOISIN

## Cibles
- Firmware : TP-Link Archer C50 V4
- Application : InsecureBank v2

## Structure du repo
### /architecture - Phase 1 : Architecture cible & modèle de menace
- Phase1_Threat_Model.md - modélisation des menaces initiale
- Phase1_Secure_Architecture.md - architecture sécurisée recommandée
- Phase1_Mitigation_Measures.md - mesures de mitigation associées
      
### /firmware - Analyse du firmware TP-Link (extraction Binwalk)
- extractions/, raw/, fat - systèmes de fichiers extraits du firmware
- httpd — binaire du serveur web embarqué (contient la faille d'upload non signé, cf. FW-01)
- vmlinux-2.6.32-5-4kc-malta - noyau Linux extrait
      
### /mobile - Reverse engineering de InsecureBank v2 (jadx)
- Android-InsecureBankv2 - code source décompilé (contenu analysé)
- bin/, lib/, jadx-1.4.7.zip, LICENSE, NOTICE - outil jadx utilisé pour la décompilation
  
### /vulnerabilites - Analyse et classification des vulnérabilités
- Phase2_Methodology.md - méthodologie d'audit (Phase 2)
- Phase2_Assets_Inventory.md - inventaire des assets testés
- Phase2_QEMU_Results.md - résultats de l'émulation QEMU (scan réseau, ports ouverts)
- Phase2_Evidence.md - preuves collectées durant la Phase 2
- Phase2_Complete_Findings.md - liste complète des findings
- CVSS_Findings.md - tableau CVSS partagé (12 vulnérabilités notées)
- Phase3_Attack_Scenarios.md - scénarios d'attaque détaillés (preuves d'exploitation)
- Phase3_STRIDE_CVSS.md - modélisation STRIDE croisée avec le scoring CVSS

### /proofs - Preuves matérielles (fichiers sources analysés)
- EVIDENCE_MAPPING.md - table de correspondance entre chaque vulnérabilité et sa preuve
- firmware/ - extraits bruts issus du firmware
- mobile/ - fichiers sources vulnérables extraits de l'APK

