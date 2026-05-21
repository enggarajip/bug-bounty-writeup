# Sensitive Third-Party API Key Exposure (Segment Write Key)

## Vulnerability Details
* **Target Domain:** admin.atlassian.com
* **Vulnerability Type:** Sensitive Data Exposure -> Disclosure of Secrets
* **Status:** Duplicated (Technically Valid)
* **Severity:** Medium (CVSS 5.3)

---

## Executive Summary
During a security reconnaissance asset review on `admin.atlassian.com`, a hardcoded third-party API key associated with the Segment analytics platform was discovered within the publicly accessible client-side JavaScript assets. 

While the report was marked as a duplicate, the engineering triage team validated the finding and updated the vulnerability classification taxonomy from *Insecure Data Storage* to *Sensitive Data Exposure (Disclosure of Secrets)*.

---

## Methodology & Proof of Concept (PoC)
1. **Target Crawling:** Automated spidering was conducted against the target platform using custom crawling tools to map out hidden endpoints and frontend dependencies.
2. **Static Code Analysis:** Client-side JavaScript bundles were extracted and analyzed for hidden secrets, hardcoded configurations, and sensitive environmental variables.
3. **The Leak:** Inside the localized script configurations, the following active token pattern was exposed:
   ```text
   Segment-Write-Key: 31591329-xxxx-xxxx-xxxx-abc674cf10d0 (Redacted)

- Security Impact
An exposed Segment Write Key allows unauthorized third parties to interact with the organization's Segment API endpoint. Potential risks include:

* Data Poisoning: Attackers can transmit spoofed analytics events or forge user behavior logs, corrupting critical business intelligence metrics.

* Denial of Service / Billing Infiltration: Malicious actors could flood the data collection endpoint, causing operational data spikes and triggering artificial billing overages on the corporate account.

- Remediation Strategy
Token Revocation: Immediately cycle and invalidate the compromised Segment Write Key via the Segment dashboard.

* Backend Proxying: Move analytics tracking logic to a backend proxy architecture rather than exposing raw ingestion keys directly to the client-side DOM.

* Automated Secrets Scanning: Implement continuous CI/CD pipeline scanning tools to detect hardcoded high-entropy tokens before deployments hit production environments.
