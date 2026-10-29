# HURF CyberSecurity

A security console for HURF Group. It scans websites and email domains, scans and quarantines a Zoho Mail inbox, investigates reported emails, and produces leadership reports.

## What's in this repo

| Path | What it is |
|---|---|
| `index.html` | The whole console: one self-contained page (HTML, CSS, JS and the logo inline). |
| `engine/make-blueprint.json` | The Make.com scenario "HURF CyberSecurity · Security Scan" (ID 6810288) that runs the scans. |
| `docs/ARCHITECTURE.md` | How the pieces fit together. |
| `THIRD_PARTY.md` | Open-source material this project uses, and its licenses. |

## How it runs

The console is published as a Claude artifact. When you open it from claude.ai, it gets these capabilities:

- **Database** (assets, threats, learned, inboxscans, incidents, history, training, config), shared by everyone with access
- **User**, for names and admin checks
- **Sentinel AI**, a Claude model call that reviews emails
- **Make connector**, which runs scenario 6810288 and reads data store 161148

If you open `index.html` straight from GitHub or from disk, it goes into **offline mode**. It shows its layout but can't scan, save or use AI. To run it outside Claude, it would need its own backend.

## Editing

1. Edit `index.html`.
2. Bring the changed file back to Claude to republish it to the same artifact. Pushing to GitHub doesn't update the live console by itself.
3. Engine changes: import `engine/make-blueprint.json` in Make (Scenarios › ⋯ › Import Blueprint), or edit the scenario in Make and re-export it here.

## Safety rules (don't loosen without review)

- Inbox scanning reads sender, subject and a short preview. It never reads attachments and never deletes anything.
- The default mode is **Review first**: nothing moves until someone presses Quarantine.
- The optional auto-move only takes **confident scams**: risk 85 or more and at least 2 strong signals (or a look-alike or previously reported sender), and never mail from company domains.
- Quarantine is a Zoho folder ("HURF Quarantine", falling back to Spam). Release puts the email back in one click.
- Self-tests are simulations. They're never written to Threats.
- Scanning other employees' mailboxes needs Zoho admin approval and a notice to staff, plus a patient-data agreement (BAA) where patient information may be involved.

## Keep this repo private

It contains internal IDs (Make scenario, data store and connection). They aren't passwords, but there's no reason to publish them.
