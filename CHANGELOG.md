# Changelog

All notable changes to Latchkey. Dates are UTC; versions follow
[Semantic Versioning](https://semver.org).

## [Unreleased]

## [0.1.0-beta.13] — 2026-10-07

### Added
- **Sync between your computers** through OneDrive or a network folder
  (Settings → Sync). Each computer keeps its own vault; an encrypted shared
  copy is merged entry by entry every 20 seconds while unlocked. The later
  change wins and the other password is kept in history; deletions travel;
  OneDrive's conflict copies are merged and removed. Joining takes the
  master password of the computer that started, once. Settings, shortcuts
  and Windows Hello stay per computer.

### Changed
- README, the feature list and the user guide brought up to date, with
  install from the Microsoft Store.

## [0.1.0-beta.12] — 2026-10-07

### Added
- The Microsoft Store package carries the Store's identity
  (`EslamAtwa.Latchkey`, Store ID 9PGSSFT92PGP) and is attached to each
  release for uploading to Partner Center.

## [0.1.0-beta.11] — 2026-10-06

### Added
- **A Microsoft Store package**, built with every version: signed by the
  Store, so Smart App Control — which blocks each new unsigned build —
  lets it run, and the Store updates it. Same program, same vault. A copy
  from the Store does not check for updates itself; About says where a
  copy came from. Waiting for the Store identity to be filled in.
- `PRIVACY.md`, the privacy policy the Store links to.

### Changed
- **SSH host keys are checked by default.** The setting has a new name, so
  a settings file that saved the old default (off) turns it on once.

## [0.1.0-beta.10] — 2026-10-06

### Added
- **Settings → About**: version, who made it, the licence, the source,
  and links to what's new, every version, reporting a problem and the
  security policy; what Latchkey sends over the network, in one line.
- **Show what you typed** on the lock screen: an eye beside the master
  password. Hidden again when the window loses the focus.

### Fixed
- The right-click menu in the Mini Vault ran past the window's edge; it
  now stays inside, and long items end in "…" with the full text on hover.

### Security
- From a review after the repository went public: the updater key is now
  used in a step of its own after the build, so no dependency's build
  script or macro has it in its environment; every GitHub Action is
  pinned to a commit; workflows get a read-only token unless a job
  publishes; Dependabot proposes updates weekly; a build tool's
  advisory (source-map-js, development only) is fixed.

## [0.1.0-beta.9] — 2026-10-06

### Added
- A hint on the progress strip, over the session's window, after Remote
  Desktop or SSH starts with the password on the clipboard: Ctrl+V
  pastes it. A toast in the hidden main window was not seen.

## [0.1.0-beta.8] — 2026-10-06

### Fixed
- Check now in Settings found an update with nothing to press: the
  result pointed at a banner behind the dialog, which a check from
  Settings did not raise. Install and restart is now beside the result,
  and the banner appears too.

## [0.1.0-beta.7] — 2026-10-06

### Added
- **Drag entries into your own order.** The list keeps it; Settings →
  Auto-type → Order of the list switches between most recently used, by
  name and your own.
- **What auto-type remembers is visible.** The details panel lists the
  windows and sites an entry is typed into without asking, each with a
  button to forget it; Settings → Auto-type lists them all. The strip
  names quick search for typing another account instead.
- **SSH and Remote Desktop from the Mini Vault and quick search** open
  the connection in a small window of its own, not the main window. The
  SSH dialog has **Several servers…** for Bulk SSH with the same account.
- The editor's Save stays at the bottom while scrolling; Ctrl+S saves.

### Fixed
- Remote Desktop asking for the password although Latchkey stored the
  sign-in (Credential Guard, a server that always asks): the password is
  now on the clipboard as well, and the message says Ctrl+V.
- The right-click menu in the Mini Vault covered the window with no way
  out: it fits now, names its entry, has a close button and closes on a
  click outside it.

### Added
- **Updates from inside Latchkey.** A check a minute after start and
  daily (Settings → General → Updates, with **Check now**); a banner when
  a newer version is out; **Install and restart** downloads it, checks
  its signature against the key built in, locks the vault and runs the
  installer, which reopens Latchkey. Downloaded by Latchkey, not a
  browser, so SmartScreen does not warn about updates.

### Changed
- Releases carry the installer only: an installed copy updates itself,
  and the portable `.exe` could not.
- The version carries the pre-release part (`0.1.0-beta.6`), which the
  updater compares.

## [0.1.0-beta.5] — 2026-10-06

### Fixed
- The right-click menu on an entry did nothing, in the main window and
  the Mini Vault: it closed itself first, and with it the entry its
  action needed. The end-to-end test now picks from the menu.

### Changed
- The SSH and Remote Desktop dialogs show the servers used recently as
  buttons under the server field, for an account with no host of its own.

## [0.1.0-beta.4] — 2026-10-05

### Fixed
- Bulk SSH to a running MobaXterm lost sessions: each was handed over
  400 ms after the last, and one still being opened dropped the next,
  while the panel said every one had opened. Each handoff now waits
  until MobaXterm has taken it, and one it did not take says so.

### Added
- **Retry** beside every session in the Bulk SSH panel: one that did not
  open, or one closed since, opens again with the same account.
- SSH and Remote Desktop from any entry, also one with no host — a
  domain account that works on every server: the dialog asks for the
  server, and offers the ones used recently.

## [0.1.0-beta.3] — 2026-10-05

The third test release: shortcuts that do not work say so.

### Fixed
- Shortcuts another program holds are said in a banner in the main
  window until fixed, not only in a notice at start-up that was easy to
  miss — after which the shortcuts simply did nothing. When the old
  Password Vault is running, which registers the same defaults, the
  message names it. The shortcuts that did register are logged.

## [0.1.0-beta.2] — 2026-10-05

The second test release: the fixes from the 4 October code review.

### Added
- Releases are built and published by CI (`release.yml`) from a pushed
  `v*` tag, after the same build, start-up check, Defender scan and
  end-to-end run as every test build.
- Documentation: README, user guide (Arabic), SECURITY.md, architecture,
  development and this changelog.
- Tests: the auto-type flow, commands and settings, Windows Hello and
  recovery, backups; the interface's translations; hostile input to every
  parser (fuzzing); a 5,000-entry scale test; an end-to-end run of the real
  app on every build; a Microsoft Defender scan of every build.
- Five sentences from the Rust side that had no Arabic translation, found
  by the new translation test.

### Changed
- CI formats `main` itself instead of failing on formatting.
- The password-field check asks the browser ten times a second, not a
  hundred, while it waits for the focus to move after Tab.

### Fixed
- Auto-type in a browser whose address bar could not be read typed on
  the page title alone, so a phishing page titled "Sign in · GitHub"
  got the GitHub password. A title match there is now only offered in
  the picker.
- In a browser, a window pattern types only when it names the site in
  the address bar (`outlook.office.com`, or a glob like `*.corp.local`),
  never on words in the page title, which the page writes. A word
  pattern such as `Outlook` now offers the entry instead, and no longer
  matches a host that merely contains it (`outlook.evil.io`).
  "Remember this window" in a browser saves the site's host.
- SSH host keys are checked against, and recorded on, the entry only for
  the entry's own host and port. A host edited in the SSH dialog is
  checked against `known_hosts` instead: a different key is refused, an
  unknown one is asked about before connecting, and its key is never
  recorded on the entry. Bulk SSH now compares the port as well as the host.
- A host or username containing `%` is refused: `cmd.exe` expanded
  `%USERNAME%` and the like in place, so the session went somewhere
  nobody typed.
- A stored SSH key written to a temporary file for a session is created
  readable by its owner only, instead of being written first and
  restricted after, which left the private key briefly readable with the
  folder's inherited permissions.
- A password or passphrase staged for an SSH or Remote Desktop session
  that then failed to start is taken off the clipboard at once, instead
  of waiting for the clear timer.

### Documentation
- SECURITY.md names the three-minute window in which a terminal can
  receive the last session's password.

## [0.1.0-beta.1] — 2026-10-03

The first test release: a rebuild of Password Vault in Rust and Tauri,
feature-complete for its first release.

### Vault and security
- Vault format v2: Argon2id key slots for the master password and a
  recovery key, XChaCha20-Poly1305, every byte authenticated.
- A recovery key confirmed before the vault is written; an emergency kit
  to print; a check every 90 days that it is still at hand.
- Unlock with Windows Hello; the master password asked for every 14 days;
  a forgotten master password reset through Hello.
- Auto-lock, lockout after failed attempts, clipboard excluded from
  history and cleared, files restricted to the owner.
- Reading and importing the predecessor's vault, read only.

### Auto-type
- Three shortcuts; the kind of window decides what is typed (the password
  alone in a terminal, scan codes in remote consoles).
- The browser's real address from its address bar, so a page cannot claim
  another site by its title; country domains such as `.com.eg` understood.
- A password is never typed into a plain text field; typing waits for
  Ctrl and Alt to be released and checks the window before every key.
- Two-page sign-ins in one press; `{NEXTPAGE}` and `{TOTP}` in sequences.
- A progress strip, a picker that remembers by program, and a diagnosis.

### Everyday use
- Quick search over any window (`Ctrl+Alt+K`).
- Two-factor codes (TOTP), imported from other managers.
- A details panel with a live code; the list ordered by use; a right-click
  menu in every window; password history; templates; an easy-to-type
  generator for remote consoles; settings search.
- The floating bubble and Mini Vault; Arabic and English; light and dark.

### Sessions
- SSH through MobaXterm, PuTTY and OpenSSH; Bulk SSH; SSH keys; a
  Pageant-compatible agent; host-key verification.
- Remote Desktop signing itself in through Credential Manager.

### Data
- Import from Chrome, Edge, Firefox, Bitwarden, 1Password, LastPass and
  KeePass; export to CSV and Excel.
- Encrypted backups and an automatic daily backup.
- A security dashboard with a k-anonymous Have I Been Pwned check.

### Fixed during the beta
- Closing the main window now turns it into the bubble instead of
  quitting, and the bubble stays on screen.
- Auto-type no longer types while modifiers are held, which opened new
  browser windows in the predecessor.
- From review: a launched SSH session's password could reach a browser;
  `.com.eg`-style domains matched each other; a second press could carry
  on into another tab's site; Windows Hello prompted after an idle lock.

[Unreleased]: https://github.com/eslamatwa/latchkey-releases/compare/v0.1.0-beta.13...HEAD
[0.1.0-beta.13]: https://github.com/eslamatwa/latchkey-releases/compare/v0.1.0-beta.12...v0.1.0-beta.13
[0.1.0-beta.12]: https://github.com/eslamatwa/latchkey-releases/compare/v0.1.0-beta.11...v0.1.0-beta.12
[0.1.0-beta.11]: https://github.com/eslamatwa/latchkey-releases/compare/v0.1.0-beta.10...v0.1.0-beta.11
[0.1.0-beta.10]: https://github.com/eslamatwa/latchkey-releases/compare/v0.1.0-beta.9...v0.1.0-beta.10
[0.1.0-beta.9]: https://github.com/eslamatwa/latchkey-releases/compare/v0.1.0-beta.8...v0.1.0-beta.9
[0.1.0-beta.8]: https://github.com/eslamatwa/latchkey-releases/compare/v0.1.0-beta.7...v0.1.0-beta.8
[0.1.0-beta.7]: https://github.com/eslamatwa/latchkey-releases/compare/v0.1.0-beta.6...v0.1.0-beta.7
[0.1.0-beta.6]: https://github.com/eslamatwa/latchkey-releases/compare/v0.1.0-beta.5...v0.1.0-beta.6
[0.1.0-beta.5]: https://github.com/eslamatwa/latchkey-releases/compare/v0.1.0-beta.4...v0.1.0-beta.5
[0.1.0-beta.4]: https://github.com/eslamatwa/latchkey-releases/compare/v0.1.0-beta.3...v0.1.0-beta.4
[0.1.0-beta.3]: https://github.com/eslamatwa/latchkey-releases/compare/v0.1.0-beta.2...v0.1.0-beta.3
[0.1.0-beta.2]: https://github.com/eslamatwa/latchkey-releases/compare/v0.1.0-beta.1...v0.1.0-beta.2
[0.1.0-beta.1]: https://github.com/eslamatwa/latchkey-releases/releases/tag/v0.1.0-beta.1
