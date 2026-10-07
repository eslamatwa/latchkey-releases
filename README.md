# Latchkey

**A password manager for Windows, built for people who administer
machines.** One encrypted file on your own computer — no server, no
account, no administrator rights — and the credentials it holds typed
straight into terminals, Remote Desktop and sign-in pages.

> مدير كلمات مرور لـ Windows، مصمَّم لمديري الأنظمة: خزنة مشفّرة على جهازك
> بلا سيرفر ولا حساب، وكتابة تلقائية في التيرمنال وسطح المكتب البعيد وصفحات
> الدخول، وSSH وRDP، ومزامنة بين أجهزتك عبر OneDrive.

This repository holds the **downloads**, the **update feed** that installed
copies read, the **privacy policy** and the **issue tracker**. The source
code is kept privately.

## Download

- **Microsoft Store** — Store ID `9PGSSFT92PGP` (coming). Signed by
  Microsoft, so Smart App Control and SmartScreen let it run, and the Store
  keeps it up to date.
- **[Latest release](https://github.com/eslamatwa/latchkey-releases/releases)** —
  `Latchkey_<version>_x64-setup.exe`. Installs for the current user, no
  administrator rights; later versions install from inside Latchkey, their
  signature checked. Not code-signed: SmartScreen warns about the first
  download (*More info → Run anyway*, or right-click → Properties →
  **Unblock**), and Smart App Control blocks it where it is on — use the
  Store version there. `SHA256SUMS.txt` lists the checksums.

Windows 10 or 11, 64-bit.

## What it does

- **Auto-type** with one shortcut, which knows a browser's real address
  from a page pretending to be the site, a terminal from a sign-in page,
  and types two-page sign-ins in one press.
- **SSH** in MobaXterm, PuTTY or OpenSSH — one server or many at once —
  with keys in the vault and host keys checked; **Remote Desktop** that
  signs itself in.
- **Two-factor codes**, a password generator, a security dashboard, quick
  search over any window, a floating bubble and Mini Vault.
- **Sync between your computers** through OneDrive or a network folder,
  encrypted, merged entry by entry.
- Argon2id and XChaCha20-Poly1305, a recovery key, Windows Hello.
- Arabic and English, light and dark.

## Read more

| | |
|---|---|
| [User guide](USER_GUIDE.md) (العربية) | Using Latchkey, step by step |
| [Features](FEATURES.md) (العربية) | Everything it does |
| [Privacy policy](PRIVACY.md) | Latchkey collects nothing |
| [Security](SECURITY.md) | The cryptography, the threat model, reporting a vulnerability |
| [Changelog](CHANGELOG.md) | What changed in each version |

## Problems and ideas

[Open an issue](https://github.com/eslamatwa/latchkey-releases/issues).
Security problems: privately, as [SECURITY.md](SECURITY.md) says — not in
a public issue.

© Eslam Atwa. All rights reserved. Free to use.
