# Architecture

```
Browser (claude.ai artifact: index.html)
  ├─ Database ......... shared state (assets, threats, learned patterns, incidents, readiness, score history)
  ├─ Sentinel AI ...... Claude model reviews suspicious emails (quick tier)
  └─ Make connector ─▶ Scenario 6810288 "HURF CyberSecurity · Security Scan"
                         Router by `mode`:
                         ├─ domain (default): Google DNS (SPF, DMARC, MX, DKIM ×4, NS) + homepage fetch
                         │    + malware-pattern extraction + exposure checks (WP users, XML-RPC, .git)
                         │    → data store key  scan:<domain>
                         ├─ inbox: Zoho Mail API → Inbox (limit = `ids`, default 50, max 200)
                         │    + Spam folder (25) → data store key  inbox:<mailbox>
                         └─ quarantine | release: Zoho updatemessage moveMessage
                              → "HURF Quarantine" folder (else Spam) or back to Inbox
```

## Scoring
- Each scan scores 100 − 20 per failed check − 6 per warning. The overall score is the average.
- Email verdicts: risk 60 or more is a scam, 25 or more is suspicious. The final risk is the higher of the pattern-check score and the Sentinel AI score.

## Detection layers (email)
1. Sender and reply-to analysis, look-alike domains (hurfgr0up.com and similar), free-mail impersonation
2. Scam language patterns (payroll, wire, gift cards, credentials, extortion)
3. Link checks (shorteners, raw IPs, punycode, look-alike hosts)
4. Team memory (the "This is a scam" / "This is safe" buttons)
5. Header forensics when raw headers are pasted (SPF/DKIM/DMARC results, alignment, origin, mailer)
6. Sentinel AI review

## Frameworks
- Each issue is tagged with NIST CSF 2.0 subcategories, MITRE ATT&CK techniques and OWASP Top 10 (2021).
- The readiness checklist follows CISA Cybersecurity Performance Goals 2.0 and is grouped by NIST CSF 2.0 function.
