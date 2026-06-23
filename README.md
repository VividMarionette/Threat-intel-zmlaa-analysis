# Threat Analysis Report: Deconstructing Evasion & Cloaking on zmlaa[.]buzz
##Summaru
This report is a independant documentation of the suspicious domain 'zmlaa[.]buzz'. The host has multiple layers of defence including network-layer port spoofing, application-layer wildcard deception, and Content Delivery Network (CDN) edge routing-to hide and active phishing landing page from automated security crawlers.

All tests was conducted via a isolated Kali Linux network environment usuing 'proxychains4' over the Tor network to preserve operational security (OPSEC)

----------------

##Technical Methods and Findings

### Phase 1: Port Enumeration & Honeypot Identification
**Observation:** Initial aggressive Nmap scans indicate that all 65,535 TCP ports were in ab 'open' state
**Analysis:** This uniform response indicates an active defense mechanism (honeypot/port spoofing) designed to exhaust scanner resources, flood log systems, and trigger automated analyst timeouts.
**Resolution:** Switched to targeted manual socket checks using 'netcat' via Proxychains. True socket connection erros ('COnnection Refused / Timeout' ) were successfully isolated , providing the honeypot layer could be bypassed to locate the actual active endpoints on ports '80', '443', '8080', and '8443'.

### Phase 2: Directory Enumaration & Wildcard Deception
**Observation:** Automated directory fuzzing via Gobuster initially returned massive of false-positive '200 OK' status codes for arbitrary paths (eg., '/CV/Root'), stalling standard tool logic.
**Ananlysis:** The web server was identified as utilizing a Custom Application-Layer Path andleer. For standard probes, the server returned an anomalous 9-byte plain-text string containing 'Not Found' masked behind a legitimate '200 OK' HTTP status code
**Resolution:** Optimized tool parameters by disabling the default status blacklist ('-b'""') to capture ra server responses symmetrically.

### Phase 3: Payload Extraction & Cloakig Verification
**Observation:** Targeted quries to speciic hidden structures (such as legacy CVS directories) returned a massive expansion in payload size 3.132 bytes.
**Analysis** Command-line data streams were extracted using raw 'curl' headers to bypass  browser-side '308 Parmanent Redirect' loops '[-> /]'.
**Execution:** Running binary analysis utilities ('strings' and 'grep') on the cached  3,132-byte file revealed that the payload contained structured HTML code for an active phishing template rather than actual legacy version control records. This behaviour confirms **application-layer cloaking**, where the infrastructure serves generic text errors to basic security scanners but perserves the phishing application for targeted victims.

### Phase 4: CDN Edge Interaction
**Observation:** Deep-path directory enumeration triggered consistent '.cahce', '.passwd', 'admin.php' etc, headers and ero-byte redirections.
**Analysis:** The domain reilies entirely on CLoudflare edge proxies to mask its true origin IP address. When aggressive scanning patterns are detected , the global CDN limits traffic by trapping automated scanners within infinite edge verification loops and zero-byte redirec responses.

---------------

### INDICATORS OF COMPROMISES (IoCs)
**Domain:** 'zmlaa[.]buzz'
**Active Ports: '80', '443', '8080', '8443'\
**Observed Threat Payload Size:** 3.132 bytes (HTML Phishing Template)
**Default Response Payload Size:** 9 byres (Plain-text "Not found")

## Remediation & Actions Taken
1. Submitted malicious application-layer signature to Google Safe Browsing to block the domain globally across consumer browsers.
____________________________________________________________

-*Note: This investigation was compiled independantly for an entry-level security analyst portfilio to demonstrate proficiency in HTTP protocol analysis, traffic proxing, network troubleshooting, and threat reporting.*
