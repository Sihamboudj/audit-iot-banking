# Mapping Vulnérabilités ↔ Captures

| Vulnérabilité | Capture | Preuve Visuelle |
|---|---|---|
| VULN-M-01 | vul1.PNG | String protocol = "http://"; |
| VULN-M-02 | vul2.PNG | BasicNameValuePair("password", ...) |
| VULN-M-03 | vul3.PNG | Log.d("Successful Login:", account=...) |
| VULN-M-04 | vul4.PNG | SharedPreferences + Base64 encoding |
| VULN-M-05 | vul_5.PNG | MyWebViewClient class (vide, pas pinning) |
| VULN-M-06 | vul_6_et_9.PNG | android:debuggable="true" |
| VULN-M-07 | vuml_7_et_8.PNG | android:exported="true" (DoTransfer, PostLogin) |
| VULN-M-08 | vuml_7_et_8.PNG | TrackUserContentProvider exported=true |
| VULN-M-09 | vul_6_et_9.PNG | android:allowBackup="true" |
| VULN-FW-01 | VULN_1_CVE_2014-54126.PNG | strings httpd \| grep signature (VIDE) |
| VULN-FW-02/03 | VULN-FW-02_03.PNG | admin:$1$$iC.dUsGpxNNJGeOm1dFio/ (MD5) |
| QEMU+nmap | quemu_nmap.PNG | ports 2222, 8080, 5432, 631 ouverts |

