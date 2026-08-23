Case Report: Phishing Campaign — hrconnex.thm & Amazon Impersonation

**Alerts Covered in This Report:**

8814, 8818, 8815, 8816
- 8814 & 8818: Duplicate detections of the same phishing campaign (hrconnex.thm → j.garcia)
- 8815 & 8816: Related detections of the same incident (amazon.biz phishing email → blocked click attempt by h.harris)

**SUMMARY / VERDICT**

This alert targeted phishing attempt on a new employee (j.garcia), using a personalized link and an urgent langauge to impersonating HR onboarding. No evidence the link was accessed.

**IOC (Indicator of Compromise)**

- amazon.biz (spoofed domain)
- Blocked malicious URL	http://bit.ly/3sHkX3da12340
- Destination IP	67.199.248.11
- Source (victim) IP	10.20.2.17 (h.harris's workstation)
- bit.ly/3sHkX3da12340 (malicious shortened link)
- urgents@amazon.biz (phishing sender)
- Sender Domain: hrconnex.thm
- Sender Address: onboarding@hrconnex.thm
- Sender uses urgency-based social engineering language
- Personalized URL pattern: hrconnex.thm/onboarding/15400654060/j.garcia
<img width="981" height="378" alt="image" src="https://github.com/user-attachments/assets/170f5cb6-eb8e-4f93-baf2-12bbe90568ac" />

**Evidence / Investigation Steps**

- Reviewed alert content - noted the link included Garcia's name, indicating a personalized/targeted phishing attempt, and urgent language pressuring immediate action.
<img width="641" height="78" alt="image" src="https://github.com/user-attachments/assets/105c4d47-7c97-4928-a509-968265df8629" />

- Searched datasource=firewall URL="hrconnex" - 0 result - indicate that the link was not accessed by anyone.
Scope / Impact
<img width="585" height="189" alt="image" src="https://github.com/user-attachments/assets/3de091aa-03ea-4234-b7f6-0c2dcdc3e71a" />

- Searched sender=onboarding@hrconnex.thm recipient=j.garcia@thetrydaily.thm → found 3 emails sent between 03:52–04:58 AM → indicates repeated, persistent targeting of Garcia.
<img width="1073" height="630" alt="image" src="https://github.com/user-attachments/assets/efe36c02-307a-44c7-b3d5-9c125a3d2fd4" />

- Broadened search to hrconnex.thm across all datasources → returned 3 total results: 2 phishing emails to Garcia, plus 1 internal email from h.harris to j.carter describing hrconnex.thm as a legitimate new third-party HR vendor. 
<img width="1053" height="336" alt="image" src="https://github.com/user-attachments/assets/42ff932c-4616-40a3-b5e9-080ebf6b2a63" />

- Checked company vendor/asset documentation for hrconnex.thm → domain not found in approved vendor records → indicates h.harris's internal claim is inaccurate, raising questions about the reliability of that email.
<img width="1064" height="609" alt="image" src="https://github.com/user-attachments/assets/5a638c24-1dc2-4485-b5e1-dcb46b13eccd" />

  Searched h.harris's full mailbox (sender=h.harris OR recipient=h.harris) to assess account reliability → found:

- A phishing email impersonating Amazon (spoofed .biz domain, shortened link, urgency language)

- An outbound email to a personal Gmail address (le@gmail.com) discussing business matters — flagged as a policy concern requiring follow-up

- An outbound email to an external domain (modishmillinery.com) referencing an "outlined plan" and "next phase" with no business context — flagged as suspicious, possible insider activity or compromised-account coordination
<img width="1060" height="557" alt="image" src="https://github.com/user-attachments/assets/193d967a-8b87-432c-99b8-382d5a49f62b" />
<img width="1067" height="467" alt="image" src="https://github.com/user-attachments/assets/8a102d03-f806-48c5-ba4d-1cf9c473b236" />

- Attempted to locate an authentication/login datasource to check for account compromise indicators (unusual login times/locations) → none available in this environment → noted as an investigation gap.

- Searched firewall logs for the exact malicious URL from the Amazon phishing email (bit.ly/3sHkX3da12340) → found a blocked outbound connection attempt from 10.20.2.17 (h.harris's workstation) at 04:58:29 → confirms h.harris's machine attempted to reach the malicious link, but the firewall blocked it before any payload/data transfer occurred.
<img width="756" height="328" alt="image" src="https://github.com/user-attachments/assets/d605573e-c88e-4b7c-902a-860f724882dd" />

Scope / Impact**

This phishing campaign was confirmed against one primary target (j.garcia); no evidence was found that the malicious link was accessed or that credentials/data were submitted. During investigation, a second and separate issue was identified in h.harris's account (HR department), including a blocked attempt to reach an unrelated phishing link and unresolved suspicious external correspondence. No evidence was found of lateral spread to additional employees or systems based on available logs, though the authentication log gap means account compromise for h.harris cannot be fully ruled out. Overall impact is currently assessed as low (no confirmed data loss or system compromise), but Harris's account represents an unresolved risk pending further investigation.

**Context on the Target**

**J Garcia**

Garcia was likely targeted under the assumption that she is a new employee, since the phishing email centered on completing an onboarding profile. However, her internal email history shows involvement in interview scheduling, a product launch, and a team event — activity that suggests she may be more established than "brand new." Her exact hire date could not be confirmed, as no HR/personnel datasource is available in this environment.

**H Harris**

Harris works in Human Resources, which explains why her internal claim vouching for hrconnex.thm as a legitimate vendor was believable — HR staff routinely handle vendor and onboarding coordination. This role likely also gives her access to employee records, meaning if her account is compromised, it raises the risk of exposing sensitive staff data. It's also plausible that Harris's access to hiring information is how the attacker (or compromised account) knew Garcia was a new hire, which would connect the two incidents rather than leaving them as coincidental.

Recommended Actions

- Reset password, run an EDR/endpoint scan on 10.20.2.17, and review login history if auth logs become available. Do not presume guilt — frame as investigation, not surveillance.
- Recommend adding an authentication/identity log source, since this investigation could not confirm or rule out account compromise without one.
- Focus refresher training on new hires (Garcia's exposure) and on verifying new vendors before internal endorsement (Harris's exposure).
- Inform Garcia's manager of the targeted attempt; advise Garcia and Harris to remain alert and report anything unusual.
- Add hrconnex.thm, amazon.biz, modishmillinery.com, and bit.ly/3sHkX3da12340 to the firewall/email gateway blocklist.
- Per the SOC Lead's original note, refine the rule so it doesn't over-flag purely on unfamiliar TLDs, reducing false positives on legitimate new vendors.
