# Security

Latchkey holds the credentials that open servers. This document says what
it protects, how, and — as plainly — what it does not. The authoritative
detail is in [SPEC.md](SPEC.md); section numbers below point there.

## Reporting a vulnerability

Please report privately through GitHub's
[security advisories](https://github.com/eslamatwa/latchkey-releases/security/advisories/new)
for this repository, not in a public issue. Include what you found, how to
reproduce it, and which version.

## What Latchkey protects against

| Threat | Protection |
|---|---|
| The vault file is copied or the laptop is stolen | The file is encrypted; without the master password, the recovery key or this machine's Windows Hello it is random bytes, and guessing the password is slowed by Argon2id. |
| Another Windows account on the same machine | Every file Latchkey writes is restricted to the owner's account (an ACL with only that user). |
| Someone at an unlocked, unattended machine | Auto-lock after idle time (5 minutes by default); every window, dialog and the clipboard are cleared on lock. |
| Repeated password guessing at the lock screen | A lockout after failed attempts that lengthens with each streak (SPEC 3.7, 4.2). |
| A phishing page that titles itself like a real site | Auto-type matches on the browser's real address bar, not the page title; another domain is no match. When the address bar cannot be read, a title match is only offered in the picker, never typed unasked. |
| Typing a password into the wrong place | Auto-type checks the target window before every keystroke, waits for modifier keys to be released, and refuses to type a password into a plain text field. |
| A password left on the clipboard | Copies are excluded from Windows clipboard history and cloud sync, and cleared after 30 seconds — only if still Latchkey's. |
| Secrets leaking into logs | Logging never receives a secret; titles of windows and entries are not logged either. |
| A tampered vault or header | Every byte of the file is authenticated; any change fails to open. |
| A hostile import file or vault file | Every parser is fuzzed in CI with random input and must refuse, never crash. |

## What it does not protect against

- **Malware running as your Windows user.** It can read the screen, log the
  keyboard as you type the master password, and read process memory. No
  password manager on the same account survives that; keep the machine
  clean.
- **A machine left unlocked with the vault open** before auto-lock fires.
- **Memory paged to disk.** Secrets are wiped from memory when no longer
  needed, but Latchkey does not lock pages (`VirtualLock`); the page file
  can hold copies. Use **BitLocker** (SPEC 11.5).
- **Losing every way in.** Forget the master password, lose the recovery
  key, lose Windows Hello and every backup, and the vault cannot be
  opened — by anyone. There is no back door, no recovery server and no
  security question, on purpose (SPEC 4.18b).
- **Another terminal right after a session.** For three minutes after an
  SSH or Remote Desktop session is opened from Latchkey, the auto-type
  shortcut in *any* terminal or remote console types that session's
  password without asking — the prompt it was opened for is usually the
  next window in front. Pressing the shortcut in an unrelated terminal in
  that window of time sends it there. Browsers and other programs never
  get it (SPEC 4.12).
- **Keeping the second factor with the password.** A TOTP secret stored in
  the vault turns two factors into one thing to steal. That is the user's
  choice per entry (SPEC 4.18).

## Cryptography

**Vault format v2** (SPEC 11.4):

- The contents are JSON, padded to a multiple of 4 KiB, encrypted with
  **XChaCha20-Poly1305** under a random 32-byte **data key**.
- The data key is stored **wrapped** once per way in — a *slot* — each
  with its own **Argon2id**-derived key, salt and nonce:
  - the **master password**, with parameters calibrated to about 500 ms on
    the machine and never below 64 MiB, 3 iterations;
  - the **recovery key**, 160 random bits shown as eight groups of four
    Crockford base32 characters.
- Each slot's wrapping authenticates the magic, the version and the slot's
  own kind and cost, so a slot cannot be relabelled or cheapened. The body
  authenticates the whole header.
- Parameters are checked before anything is derived (at most 1 GiB and 20
  iterations), so a hostile file cannot ask for gigabytes.
- Changing the master password rewraps the data key; the recovery key is
  issued and **confirmed by the user before anything is written**.

**Windows Hello** (SPEC 4.19): a key in Windows Hello — in the TPM where
there is one — signs a random 32-byte challenge after the user's
fingerprint, face or PIN. HMAC-SHA256 of that signature wraps the data key
in `hello.lkd`. The file opens nothing without the signature; the private
key never leaves Hello. The master password is still asked for every 14
days, so it is not forgotten.

**Two-factor codes:** RFC 6238 (SHA-1, SHA-256, SHA-512), tested against
the RFC's own vectors; codes are worked out when used and never stored.

**Have I Been Pwned:** only the first five characters of a password's
SHA-1 hash leave the machine (k-anonymity), and only when the user runs
the check.

Libraries: RustCrypto's `argon2`, `chacha20poly1305`, `hmac`, `sha1`,
`sha2`; no hand-written cryptography.

## Design choices that matter for security

- **Secrets never enter the webview** unless shown on purpose (SPEC 11.2).
  The window gets summaries; copying, auto-type, SSH staging and RDP are
  done in Rust. A password reaches the webview only when the user reveals
  it or opens the editor.
- **No keyboard hook.** Shortcuts use `RegisterHotKey`, which delivers only
  the combinations asked for. Latchkey never sees other keystrokes.
- **No administrator rights, no service, no autostart entry, no code
  injected anywhere.** Typing uses the ordinary `SendInput`; windows
  running as administrator are refused, not worked around.
- **One `unsafe` crate.** `latchkey-core`, which holds every secret and
  all the cryptography, is `#![forbid(unsafe_code)]`; Windows calls live in
  `latchkey-platform`, each `unsafe` block justified in a comment.
- **Strict Content Security Policy** in the webview: no remote scripts, no
  inline scripts, no connections but Latchkey's own.
- **Dependencies are audited** in CI with `cargo deny` (advisories,
  licences, sources); one accepted advisory is documented in `deny.toml`
  with its reason.
- **Every build is scanned by Microsoft Defender** in CI before release.

## Sync

- The shared copy in the sync folder (`latchkey-sync.lks`) is sealed like
  the vault, under a key of its own that is wrapped under the master
  password of the machine that started syncing. Whoever can read the
  folder — OneDrive, Microsoft, anyone with the account — holds what a
  stolen vault file gives: nothing without that password, and Argon2id
  slowing every guess. Choose the master password with that in mind.
- That key is also kept inside each syncing machine's vault, so syncing
  needs no password; it never appears in the shared copy, the settings or
  the log.
- A merge never loses a password silently: when two machines changed the
  same entry, the other password is kept in its history. A shared file
  made under another key is reported, not overwritten.

## Updates and releases

- **An update installs only if it is signed** with the updater's key
  (minisign, Ed25519). The public key is built into Latchkey; the private
  key is a repository secret, kept offline by the owner as well. A file
  that does not verify is never run, whatever the feed says, and an older
  version is never offered (SPEC 11.10).
- **Nothing installs unasked.** The check sends nothing about the user or
  the vault, can be turned off, and the vault is locked before the
  installer runs.
- **The key is used apart from the build.** A release is built first; the
  installer is then signed in a step that runs only Tauri's CLI, so no
  dependency's build script or macro ever has the key in its environment.
- **The build is pinned.** Every GitHub Action is pinned to a commit, not
  a tag that could be moved; workflows get a read-only token unless a job
  publishes; Dependabot proposes dependency updates weekly, and
  `cargo deny` refuses known-vulnerable crates on every change.
- Every release is built, started, scanned by Microsoft Defender and
  driven end to end in CI from the tagged commit; `SHA256SUMS.txt` lists
  its checksums.

## Known limitations of the beta

- **Not code-signed.** SmartScreen warns about the first download; a
  signature is the user's assurance the file came from here. Until then,
  compare the SHA-256 in the release's `SHA256SUMS.txt`. Updates are
  signature-checked by Latchkey itself (above).
- **The Remote Desktop password is also put on the clipboard**, because
  Windows can ask for it despite the stored sign-in (Credential Guard). It
  is excluded from clipboard history and cleared as any copy is.
- **Host keys are checked on first use only.** From 0.1.0-beta.11 the key
  is checked before every SSH connection (on by default); the first one
  for a server is shown to be confirmed, and only a change after that is
  caught. A server that cannot be reached for the check is connected to
  anyway, with a note.
- **Smart App Control** blocks the unsigned GitHub build; the Microsoft
  Store build is signed by Microsoft (SPEC 11.11).
