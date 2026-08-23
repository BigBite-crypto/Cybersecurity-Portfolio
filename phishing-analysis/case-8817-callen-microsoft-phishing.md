**SUMMARY / VERDICT**
C. Allen received a phishing email impersonating Microsoft's account security team, using fear-based urgency language and a typosquatted domain (m1crosoftsupport.co) to lure him into clicking a fake login link. Unlike the Garcia and Harris cases, this link was successfully accessed — the firewall allowed the connection rather than blocking it. This is assessed as a True Positive with confirmed access, the most severe of the three cases investigated, as credential compromise cannot be ruled out.

**IOC (Indicator of Comprimise)**

- Phishing URL: m1crosoftsupport.co/login
- Fabricated IP (in email content): 102.89.222.143
- Sender Domain: m1crosoftsupport.co
- Sender Address: no-reply@m1crosoftsupport.co
- SourceIP: 10.20.2.25 (Victim's IP address)
- DestinationIP: 45.148.10.131
- Malicious URL: https://m1crosoftsupport.co/login
<img width="1061" height="433" alt="image" src="https://github.com/user-attachments/assets/0575a6f5-ff80-4a9b-9f9d-442484dcb496" />


**Evidence / Investigation Steps**
- Reviewed alert content - indetified senders domain m1crosoftsupport.co as a typosquat of "microsoft", using number "1" in place of "i" to visually mimic the legitimate brand.
- Noted the email employes fear-based langauge ("secure your account immediately").
- Noted the email includes fabricated specific details (IP Address, location, and timestamp) to manufactor a false sense of legitimaacy, A technique to lower victem suspicion.
<img width="1061" height="433" alt="image" src="https://github.com/user-attachments/assets/4f913a59-a682-4510-808c-965227e9e072" />

- Searched datasource=firewall URL=m1crosoftsupport.co - found an allowed (not blocked)outbound connection from 10.20.2.25 to https://m1crosoftsupport.co/login at 05:00:42 - indicates the phishing link was successfully accessed, unlike the Garcia and Harris cases. This significantly raises the severity of this incident, as credential compromise cannot be ruled out.
<img width="721" height="351" alt="image" src="https://github.com/user-attachments/assets/4299a26f-ed5c-44be-ae13-86b0261159fc" />

- Confirmed 10.20.2.25 belongs to c.allen's workstation. This confirms the phishing link was successfully accessed — a materially different outcome from the Garcia case (0 access) and Harris case (blocked access). Credential compromise cannot be ruled out, though no direct evidence of submitted credentials was found in available logs.
<img width="898" height="37" alt="image" src="https://github.com/user-attachments/assets/bce679a9-2d51-47e1-8ab7-0df96959b12b" />

- Searched parrish@stylewatchjournal.com i didnt find any suspicion on this company business
<img width="1062" height="311" alt="image" src="https://github.com/user-attachments/assets/52571b8e-33c4-4a59-bc7a-dce9e19fd19d" />

- Searched datasource=firewall SourceIP="10.20.2.25" (c.allen's workstation) to review all outbound activity around the time of the phishing click - returned 7 events total. Aside from the single connection to m1crosoftsupport.co/login, all other activity (internal company tools, Google searches, Asana, Facebook Ads Manager) appears consistent with normal work behavior. No evidence of follow-on suspicious activity (e.g., unusual outbound connections, lateral movement, or data exfiltration indicators) was found in available firewall logs.
<img width="1009" height="641" alt="image" src="https://github.com/user-attachments/assets/8824359f-3255-490e-8266-9492a14e7673" />
<img width="813" height="632" alt="image" src="https://github.com/user-attachments/assets/3497374a-7b0c-4aaa-b6a7-8ea6e88f63be" />
<img width="813" height="632" alt="image" src="https://github.com/user-attachments/assets/db0162ec-f6df-469f-9f8b-953f41006356" />
<img width="906" height="319" alt="image" src="https://github.com/user-attachments/assets/42db668f-00f0-48fb-b07a-04f50933861b" />


**Scope / Impact**

This incident is confirmed to be isolated to c.allen's workstation and account, based on available logs. Unlike Garcia and Harris, c.allen's browser successfully reached the phishing page. However, review of all outbound traffic from his workstation shows no follow-on suspicious activity after the click — no unusual connections, lateral movement, or signs of data exfiltration were observed. Credential submission on the fake login page itself cannot be confirmed or ruled out from firewall logs alone, which is the key unresolved risk in this case. Given c.allen's role in Web Development and potential access to elevated systems, the potential impact if credentials were compromised is higher than in the other two cases.

**Context on the Target**

c.allen works in Web Development, a role that often carries elevated system access (code repositories, deployment tools, internal infrastructure). Given that the phishing link was successfully accessed — unlike the Garcia and Harris cases — the potential impact of credential compromise is higher for this account than for a standard user account, warranting priority escalation.

**Recommended Actions**

- Immediate password reset for C. Allen's account, treated as potentially compromised.
- Force re-authentication if that capability exists.
- Endpoint scan on 10.20.2.25
- Block m1crosoftsupport.co to the firewall immediately.
- Add m1crosoftsupport.co to the blacklist/threat intel feed (you noted the block URL action but should also explicitly close the gap that let it through in the first place)
- Notify c.allen and his manager, similar to what you did for Garcia's case — this is missing here

