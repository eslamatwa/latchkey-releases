# Privacy policy

*Latchkey — effective 6 October 2026*

Latchkey is a password manager that runs on your computer. It has no
account, no server and no analytics.

## What Latchkey collects

Nothing. Latchkey does not collect, send, sell or share any personal
data, usage data or telemetry.

## What stays on your computer

Your vault — passwords, notes, keys and everything else you store — is
kept in a file in your Windows profile (`%APPDATA%\Latchkey`), encrypted
with your master password. Its settings and a log are kept beside it; the
log never contains a password, a secret, or the titles of your entries or
windows. Nobody but you can open the vault: not the developer, not
Microsoft, not anyone else. If you choose a backup folder, copies of the
encrypted file go there, and only there.

## When Latchkey goes online

Only in these cases, each of which you can avoid:

- **Checking for updates** (the copy installed from GitHub; turned off in
  Settings → General → Updates). It asks GitHub for a small file naming
  the latest version. Nothing about you or your vault is sent; GitHub sees
  the request as it sees any download. Copies from the Microsoft Store do
  not check: the Store updates them.
- **Checking passwords against known breaches**, only when you start it in
  the security dashboard. It uses Have I Been Pwned's range search: the
  first five characters of each password's SHA-1 hash are sent, never the
  password or the full hash, and the match is made on your computer.
- **What you open yourself**: a website from an entry, or an SSH or
  Remote Desktop session to a server you choose.

## Children

Latchkey is a tool for system administrators and is not directed at
children.

## Changes

Changes to this policy are published in this file, with the date above.

## Contact

Questions about privacy:
[open an issue](https://github.com/eslamatwa/latchkey-releases/issues) on GitHub.
Security problems: see [SECURITY.md](SECURITY.md).
