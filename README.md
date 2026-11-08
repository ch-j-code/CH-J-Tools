# CH-J Tools

CH-J Tools is a desktop SSH and SFTP client for Linux server administration, with Windows server support in selected tools, and a local file hash calculator and checksum verification tool for Windows, macOS, and Linux. It combines server profiles, an interactive terminal, a multi-panel file manager, monitoring, log inspection, user administration, NGINX management, disk management, and 49 hashing algorithms in one application.

**Beta 0.1.2** is the current public release, built with Rust and Tauri 2. CH-J Tools was renamed from CH-J Server Manager. On first start, the existing data folder is moved automatically; the Windows installer removes the old installation.

## Download

### Beta 0.1.2 — current public release

| Platform | Architecture | Package |
| --- | --- | --- |
| macOS | Apple Silicon (`arm64`) | [DMG signed with Developer ID and notarized by Apple](https://tools.ch-j.eu/download.php?channel=beta&platform=mac&arch=arm64&lang=en) |
| Windows 11 | `x64` | [NSIS installer](https://tools.ch-j.eu/download.php?channel=beta&platform=win&arch=x64&lang=en) |
| Ubuntu 24.04 / Zorin OS | `amd64` | [DEB package](https://tools.ch-j.eu/download.php?channel=beta&platform=ubuntu&arch=x64&lang=en), also available through the [APT repository](#ubuntu-apt-repository), beta channel |

Every release file has a detached OpenPGP signature (`.asc`) and a SHA-512 checksum. See [download verification](#security-encryption-backups-and-license). Back up important data before installation or updates.

[Browse source code](https://tools.ch-j.cz/cs/source/) · [Releases](https://tools.ch-j.eu/en/releases/) · [Documentation](https://tools.ch-j.eu/en/documentation/)

**Looking for the mobile app?** SM Mobile: [website](https://tools.ch-j.cz/cs/?edition=mobile) · [sources](https://tools.ch-j.cz/cs/source/?edition=mobile) · [README on GitHub](https://github.com/ch-j-code/SM-Mobile). See also [CH-J Ecosystem](#ch-j-ecosystem--desktop--mobile).

## Supported operating systems

| Operating system | Current target | Package |
| --- | --- | --- |
| Windows | Windows 11, `x64` | NSIS `.exe` installer |
| macOS | Apple Silicon, `arm64` | `.dmg` |
| Linux | Ubuntu 24.04 LTS / Zorin OS, `amd64` | Debian-format `.deb`; signed APT repository, beta channel |

The Linux client is distributed as the `ch-j-tools` DEB package, which replaces `ch-j-server-manager`. Ubuntu 24.04 LTS is the primary test platform; the same DEB installation procedure applies to Zorin OS. Linux requires WebKitGTK 4.1, declared as a package dependency. Windows requires WebView2, included with Windows 11.

No Intel Mac installer is available. Compatibility with Linux distributions other than Ubuntu and Zorin OS, and with older Windows versions, is not verified; a Debian package format alone does not establish support for every Debian-based system.

## Available features

- encrypted local vault using scrypt and AES-256-GCM;
- server profiles with password and private-key SSH authentication;
- profile import/export in desktop JSON/CSV, with shared version 1 JSON for CH-J Tools Mobile; transferred fields exclude credentials and trusted host keys;
- mandatory SHA-256 SSH host-key verification and explicit handling of changed host keys;
- multiple SSH sessions and an interactive terminal;
- file browsing, editing, upload, download, deletion, and ZIP/TAR/TAR.GZ export through a restricted SFTP interface;
- user interface in 42 languages; license, safety guidance and installer texts in Czech, German and English;
- Key Generator: SSH keys (Ed25519, RSA, ECDSA and explicitly confirmed legacy DSA/SSH1 RSA) and a password generator (4–70 characters, A–Z, a–z, 1–9 and `-@)¨§'=+`, optional dash after every four characters), copied through the main process clipboard;
- bundled first-party tools for System Monitor, Key Generator, Log Viewer, Users, File Manager, NGINX Manager, Apache Manager, Rsync Manager, Network Manager, and Disk Manager, with separately distributed plugin updates;
- NGINX Manager with an overview of sites and reverse proxies, a wizard for new sites, graphical edits that keep unknown directives, domain routes in NGINX maps with preview and undo, domain path diagnostics, certificates, logs, configuration history, own site templates and the full configuration editor;
- Apache Manager: sites as Apache really loads them, a wizard for new sites, graphical edits with preview and undo, modules, PHP, TLS, reverse proxy, access rules, `.htaccess`, logs, MPM settings and findings with a reason and, where it is safe, a fix shown as a diff;
- Rsync Manager (`chj.rsync-manager`, Linux servers): copy, mirror, versioned backups, hard-linked snapshots and comparison between this computer, servers and folders of one server, with a preview first. Deleting needs the exact number from the preview, a system folder needs its path typed, and rsync stops if more would be deleted. Filters, bandwidth limit, resume, ACL, extended attributes and an allow-listed advanced field adapt to the rsync that runs. Profiles, schedules on the server's systemd or cron, history, retention, restore and diagnostics. Copying from or to a Windows client's disk is not supported yet; transfers between servers work from every client. Also available from the File Manager panel menu;
- Network Manager (`chj.network-manager`, Linux servers): all interfaces including disconnected ones and the one carrying the current SSH connection, IPv4 and IPv6 configuration, DNS, MTU, routes, listening ports, firewall rules and default policy, profiles that propose rules, and a "why does this port not work" check that says what can and cannot be seen from the server. Changes show a diff and each risk, and undo themselves unless confirmed after a new SSH login. VLAN, bridge and bond creation, NAT editing and Windows servers are not available yet; OpenWrt is read-only;
- Disk Manager (`chj.disks`, Linux servers only): disk map, partitions, file systems, LVM and LUKS layers, automatic expansion after a disk was enlarged, and a reviewed queue of changes that runs as a server-side job;
- System Monitor and Users for remote Linux servers and Windows servers reached through Windows OpenSSH (see the platform notes below);
- terminal command suggestions matched to the connected system (Ubuntu, Debian, Arch Linux, Rocky Linux, Alpine Linux, OpenWrt, Windows). Suggestions cover whole commands, work after `sudo`, correct typing mistakes and Windows-style paths against the real files on the server, can be turned off in Settings and have a personal dictionary. They are only inserted as text and run only when you press Enter;
- terminal appearance profiles: prepared looks (CH-J default, Retro green and amber, Solarized Dark, Dracula, Nord, Gruvbox Dark, Light, High contrast), your own profiles, font, size, cursor, all 16 ANSI colors, and profile import and export as JSON text;
- settings arranged into sections: General, Updates, Server availability, Suggestions, Terminal appearance, Security and Legal;
- integrated Web Server Diagnostics: ICMP, traceroute, DNS, HTTP/1.1/2/3 capability tests, server timings, compression, TLS, TCP ports, local history and JSON/CSV/TXT exports;
- a bundled local Hash & Checksum tool in the sidebar for calculation, verification, comparison, and checksum manifests;
- modules open inside the main window and can move to their own windows and back, with capability-based access to Core services;
- updates in Settings, with a check after startup and a dialog offering a newer version;
- faster Windows startup through language catalogs loaded on demand;
- alpha, beta, and stable update-channel selection;
- application updates protected by size checks, SHA-512, and mandatory detached OpenPGP signatures.

### Terminal, availability and File Manager

Use two terminal panes side by side or stacked, on one server or two different servers, with a draggable divider. Hiding a terminal does not end its session. Connect through a saved or manually entered jump host, with chains of up to three saved hosts and host-key verification on every hop. A manually entered host can be used without saving a profile.

With one SSH connection, a module selects it automatically; with several, it asks which session to use. Modules do not switch servers by themselves.

The availability monitor checks saved servers with a TCP connection to their SSH port without logging in. Results appear in the server list and terminal menu. An unreachable SSH port does not prove that the server is stopped. A server behind a jump host is checked through an open jump-host session and otherwise shown as not verified.

File Manager offers one, two or three panels, each showing the local disk or an open SSH session. Copy or move with keyboard shortcuts, dragging between panels or the context menu; create files with any name and edit them in Monaco. Server-to-server SFTP transfers stream through this computer without being stored on its disk, with a queue, pause, resume, cancel and conflict handling. A moved source file is deleted only after its SHA-256 matches and it has not changed.

Protected files and folders offer sudo for listing, creation, renaming, deletion, copying and moving. A protected source is deleted only after hash verification. Server-to-server copies requiring sudo are not supported. The panel menu can synchronize, compare or back up with rsync, save a profile or open Rsync Manager.

Rsync uses the application's verified SSH session with password or private-key authentication, host-key checks and jump hosts. Servers can exchange data through the app or connect directly without exposing passwords in process arguments. The scheduler also lists existing cron jobs, systemd services, daemon configuration, processes and scripts in a read-only view that hides passwords.

Network Manager supports netplan, NetworkManager, systemd-networkd and ifupdown, with ufw, firewalld, nftables and iptables. It shows listening ports and their programs; inspecting other users' programs requires sudo. Changes use a server-side backup, an undo timer and confirmation after a successful new SSH login; unconfirmed changes roll back even after a server restart. Proxmox VE bridges can be edited where ifupdown2 manages them; OpenWrt network information is read-only. Server commands come from a shared command matrix; OpenWrt otherwise offers the terminal and autocomplete, and unavailable modules report their limits.

NGINX Manager and Apache Manager offer to install certbot when missing; the certificate email address is optional.

### Standby Helper and file signatures

Standby Helper is a small background process without a WebView, enabled after installation and can be turned off in **Settings → Standby Helper**. It supports start at login, KeySync and file manager context-menu actions: **Verify signature** and **Sign (OpenPGP)** in Finder Quick Actions, Explorer, Nautilus, Nemo and Dolphin.

Quick verification opens a small window. It can use Keychain identities, saved public keys, a public key file you choose, or automatically retrieve a public key from **keys.ch-j.de** using the fingerprint in the signature. An optional expected fingerprint lets you compare the signer. The window shows the key's revocation state on keys.ch-j.de for any key. A changed file is reported as **“signature does not match the file”**.

Context-menu signing lets you choose a primary key or signing subkey, enter its passphrase in the window and write `<file>.asc`.

### Current limitations

Windows production code signing is not confirmed. In test mode, standard CA verification is temporarily relaxed for the explicitly allowlisted update host; OpenPGP verification remains mandatory for update artifacts.

Remote archive extraction, SSH port forwarding, dragging files from Finder or Explorer into a panel, server-to-server copies requiring sudo, and Safe Mode are unavailable. Use the Local disk panel to copy local files. Normal File Manager server-to-server transfers pass through this computer; Rsync Manager also supports direct transfers. Behavior and data formats may change during beta testing.

### System Monitor and virtual memory

System Monitor detects the remote operating system from system data (`uname`, or the Windows registry / `Win32_OperatingSystem`) and uses a separate Linux or Windows collector. On Windows, it works with `cmd.exe` and PowerShell as the default SSH shell. It does not show a load average on Windows, because Windows has none, and shows the processor queue length instead. When a network driver reports no byte counters, System Monitor shows packet counts.

System Monitor lists every CPU socket and GPU or accelerator. NVIDIA metrics use `nvidia-smi`; AMD metrics use sysfs. Emulated GPUs show their name and PCI ID, and unreported values appear as unavailable.

The **Memory and virtual memory** section shows a RAM breakdown in which no value is counted twice. It also shows RAM and swap history and lists every swap area or page file with its type, path, priority, persistence, and swap-in/swap-out activity. Reading metrics never requires sudo.

- **Linux swap files (root or sudo):** the form offers a free name for each new swap file. You can create, activate, deactivate, resize, reprioritize, persist, and delete swap files in `/swapfile*`, `/swap/<name>`, or `/var/lib/swap/<name>`, sized from 64 MiB to 64 GiB.
  - Supported file systems are ext2/3/4, XFS, Btrfs, and F2FS.
  - A swap area is not deactivated when its contents would not fit into the available memory, which keeps a reserve, plus the free space in other swap areas.
  - `/etc/fstab` is backed up before every change.
  - Swap partitions can only be deactivated or reprioritized. zram and zswap are read-only.
- **Windows page files (elevated administrator SSH session, UAC is never bypassed):** you can switch automatic management, set custom or system-managed sizes, and add or remove page files.
  - Changes take effect only after a restart. The application shows "restart pending" and **never restarts Windows**.

The terminal status bar measures **ICMP ping** (local system `ping`, with median, packet loss, and a reason such as timeout or blocked) and **SSH response time** (round trips over the existing SSH transport) separately. A blocked ICMP does not mark a server as offline.

### Users: root SSH access

Users protects the root account: you cannot delete it or change its UID/GID, groups, shell, or home directory. You can manage its SSH keys and SSH access through a root session or sudo. This includes `PermitRootLogin` (`no`, `prohibit-password`, which is recommended, `forced-commands-only`, `yes`, or inherited server settings). Enabling password login for root requires an extra typed confirmation.

Deleting a user stops the account's running processes first. A home not owned by the account can also be removed unless it belongs to another account or lies outside `/home`.

On Linux, Users can disable SSH password login for all existing accounts with a verified change and automatic rollback on failure. It warns about accounts without SSH keys and requires a session authenticated with a key. Accounts created later are not covered; sudo and local passwords stay available.

### Remote server systems

System Monitor and Users support remote Linux servers and Windows servers through Windows OpenSSH. Disk Manager, Rsync Manager and Network Manager target Linux servers, with the limits described above. Client installation targets do not imply support for every remote management tool on that system.

On Proxmox VE, root's `authorized_keys` is a link into the cluster file system, so Users shows it but does not change it. On Alpine Linux, a locked root account cannot log in with SSH, even with a key.

### Web Server Diagnostics

Open **Web Server Diagnostics** between **Plugins** and **Hash & Checksum**. Enter a hostname, IPv4, IPv6 or HTTP(S)/WS(S) URL, choose Auto / IPv4 / IPv6 / Both, and run the overview or an individual tool. No SSH connection or external plugin is required. Results update while probes run, and individual tools or the entire run can be stopped.

The module includes per-packet ICMP graphs/statistics (interval 0.05–10 s, total packet count, payload size, TTL and don't-fragment settings, no root required), progressive traceroute, twelve DNS record types with TTL and consistency observations, independently negotiated HTTP versions, request phase timings and independent connections for each HTTP sample, verified gzip/deflate/Brotli (and runtime-supported zstd), TLS 1.2/1.3 certificate inspection, bounded TCP port tests, an SSH host-key handshake without authentication, security-header/redirect checks and an optional WebSocket handshake. It supports Czech, German and English and local light/dark/system themes.

HTTP/1.1, HTTP/2 and HTTP/3 use the pinned bundled curl client, with strict certificate verification and no system curl dependency. HTTP/3 uses `--http3-only` and requires a completed QUIC transfer on the target HTTPS port; otherwise the UI reports unavailable, unsupported or error with a reason. `Alt-Svc` and HTTP/2 fallback never establish HTTP/3 support. DNSSEC is explicitly **not validated**; DS/DNSKEY records are observations. ICMP failures do not label a server offline.

History stays in the application's local data directory. JSON/CSV/TXT reports omit bodies, cookies, authentication metadata and URL queries. Import JSON, reopen or compare saved runs from History. OS ping/traceroute tools provide ICMP/route capabilities. The HTTP client and its CA store are bundled; no system curl or first-run download is required. Each HTTP sample starts an independent connection; connection reuse is not measured.

See [implementation, IPC, limits and verification](https://tools.ch-j.eu/en/documentation/server-diagnostics/).

### Hash & Checksum

Calculate file hashes, verify checksums, and compare local files with 49 algorithms, including SHA-256, SHA-512, SHA-3, BLAKE3, and xxHash. The built-in hashing tool supports HEX and Base64 output and GNU, BSD, and SFV checksum manifests.

Open **Hash & Checksum** from the left sidebar, directly below **Web Server Diagnostics**. The tool is bundled with the application and does not appear in the installed-plugin list. Like other modules, it can move between the main window and its own window.

The interface follows the application's saved language setting. Saving a language change updates an already open Hash window, including controls, progress, result statuses, errors, and native file-dialog labels, while preserving selected files, algorithms, parameters, and results.

Available functions:

- **Favorite algorithms:** save frequently used algorithms for quick selection.
- **OpenPGP verification:** verify detached signatures with a public key and an optional expected fingerprint.
- **Calculate:** hash a single file, multiple files, or a directory, optionally including subdirectories. Use the recommended selection (SHA-256, SHA-512, and BLAKE3), select individual algorithms, or select all 49 at once. Directory symlinks are skipped.
- **Output and parameters:** display digests as lowercase HEX, uppercase HEX, or Base64. Configure output length for SHAKE and KangarooTwelve, seeds for xxHash and MurmurHash3, and hexadecimal keys for SipHash and HighwayHash. An empty key field uses an all-zero key.
- **Verify:** compare a file with an expected HEX or Base64 digest using a selected fixed-length algorithm. HEX comparison ignores letter case and surrounding whitespace. Suggested algorithms based on HEX length are hints, not definitive identification.
- **Compare Files:** compare the calculated digests of two files using one or more fixed-length algorithms.
- **Manifests:** generate and verify GNU, BSD, and SFV checksum lists with relative paths. Generation requires a fixed-length algorithm without a key or seed; SFV requires CRC32. Verification supports automatic algorithm detection or a manual override when the digest length is ambiguous.
- **Progress and results:** view processed bytes, percentage, throughput, elapsed time, estimated remaining time, and per-file statuses; cancel a running job; copy an individual digest or all results; export results to a text file.

#### Supported algorithms

The complete list of **49 supported algorithms** is shown below. Algorithm IDs match the result table and exported results.

| Algorithm | ID | Output size (bits) |
| --- | --- | --- |
| SHA-224 | `sha224` | 224 |
| SHA-256 | `sha256` | 256 |
| SHA-384 | `sha384` | 384 |
| SHA-512 | `sha512` | 512 |
| SHA-512/224 | `sha512-224` | 224 |
| SHA-512/256 | `sha512-256` | 256 |
| SHA3-224 | `sha3-224` | 224 |
| SHA3-256 | `sha3-256` | 256 |
| SHA3-384 | `sha3-384` | 384 |
| SHA3-512 | `sha3-512` | 512 |
| SHAKE128 | `shake128` | Variable (default 256) |
| SHAKE256 | `shake256` | Variable (default 512) |
| BLAKE2b-512 | `blake2b-512` | 512 |
| BLAKE2s-256 | `blake2s-256` | 256 |
| BLAKE3 | `blake3` | 256 |
| KangarooTwelve | `kangaroo-twelve` | Variable (default 256) |
| RIPEMD-160 | `ripemd160` | 160 |
| Whirlpool | `whirlpool` | 512 |
| Tiger | `tiger` | 192 |
| Tiger2 | `tiger2` | 192 |
| MD5 | `md5` | 128 |
| SHA-1 | `sha1` | 160 |
| XXH32 | `xxh32` | 32 |
| XXH64 | `xxh64` | 64 |
| XXH3-64 | `xxh3-64` | 64 |
| XXH3-128 | `xxh3-128` | 128 |
| MurmurHash3 x86 32 | `murmur3-x86-32` | 32 |
| MurmurHash3 x86 128 | `murmur3-x86-128` | 128 |
| MurmurHash3 x64 128 | `murmur3-x64-128` | 128 |
| CityHash32 | `cityhash32` | 32 |
| CityHash64 | `cityhash64` | 64 |
| CityHash128 | `cityhash128` | 128 |
| FarmHash32 | `farmhash32` | 32 |
| FarmHash64 | `farmhash64` | 64 |
| FarmHash128 | `farmhash128` | 128 |
| HighwayHash64 | `highwayhash64` | 64 |
| HighwayHash128 | `highwayhash128` | 128 |
| HighwayHash256 | `highwayhash256` | 256 |
| SipHash-2-4 | `siphash-2-4` | 64 |
| FNV-1 32 | `fnv1-32` | 32 |
| FNV-1 64 | `fnv1-64` | 64 |
| FNV-1a 32 | `fnv1a-32` | 32 |
| FNV-1a 64 | `fnv1a-64` | 64 |
| CRC-16/CCITT-FALSE | `crc16-ccitt-false` | 16 |
| CRC-32/ISO-HDLC | `crc32` | 32 |
| CRC-32C/Castagnoli | `crc32c` | 32 |
| CRC-64/ECMA-182 | `crc64-ecma` | 64 |
| CRC-64/XZ | `crc64-xz` | 64 |
| Adler-32 | `adler32` | 32 |

SHAKE128, SHAKE256, and KangarooTwelve allow an output length of **16–1024 bytes**. Their defaults are 32, 64, and 32 bytes respectively. BLAKE3 currently produces a fixed 256-bit digest.

MD5 and SHA-1 are included only for legacy compatibility. xxHash, MurmurHash3, CityHash, FarmHash, HighwayHash, SipHash, FNV, CRC, and Adler-32 are not offered as cryptographic integrity proofs.

#### Module API

The Module API is `1.15.0`. It provides shared post-quantum/hybrid algorithm metadata, owner-scoped OpenPGP generation and text operations, and narrow permissions for Keychain storage, rsync, networking, disk management, virtual memory, file privileges and resumable transfers. Hash & Checksum uses the narrow `local.hash` permission.

Core owns every native file, directory, manifest, and export dialog. It gives the sandboxed plugin opaque, plugin-owned selection tokens rather than raw paths, rejects tokens belonging to another plugin, and keeps filesystem access out of the renderer. Hash jobs run in background tasks of the Rust Core, stream bounded chunks, support progress and cancellation, and verify stable file identity before returning results. Directory enumeration skips symlinks. Manifest verification rejects symlink escapes, traversal, absolute paths, drive paths, and UNC paths.

Hashing is local-only: the plugin has no network, SSH, or SFTP hashing capability and does not upload file contents or digests. It supports individual files, batches, recursive directories, file comparison, and GNU, BSD, and SFV manifests across 49 algorithms. MD5 and SHA-1 are retained only for legacy compatibility; fast hashes and checksums are explicitly non-cryptographic.

## Download and installation

Use the [download links at the top of this README](#download) or the official [release catalog](https://tools.ch-j.eu/en/releases/).

The [CH-J Tools release catalog](https://tools.ch-j.eu/en/releases/) lists the packages and channels that are actually available.

Distribution notes:

- macOS Apple Silicon DMGs are signed with Developer ID and notarized by Apple.
- Windows 11 x64 uses an NSIS installer.
- Ubuntu 24.04 / Zorin OS amd64 uses the `ch-j-tools` DEB package. For a downloaded package, use `sudo apt install ./<downloaded-file>.deb` if the graphical software center rejects it as coming from an unknown publisher.

### Ubuntu APT repository

Ubuntu 24.04 and Zorin OS amd64 users can install and update through the signed [CH-J APT repository](https://tools.ch-j.cz/apt/), using `Suites: beta` and package `ch-j-tools`. This package replaces `ch-j-server-manager`.

See [key verification, repository setup and installation](https://tools.ch-j.eu/en/documentation/updates-and-gpg/). APT installations display operational guidance and require license acceptance on first launch and after updates. Use APT for repository installations and avoid starting the application updater during an APT transaction.

### First launch

On first launch, choose the interface language, review the operational guidance, accept the license and acknowledge the data-loss and backup notice, then use the application without a vault password. **Servers** and **Keychain** ask to create or unlock the encrypted vault when opened. **Terminal** supports a manually entered host, username and password while the vault stays locked.

Application settings remain in the existing unencrypted configuration store; saved server profiles, passwords and Keychain identities remain in the encrypted vault. No existing encrypted profiles are moved to plaintext. Host keys accepted during locked manual connections are retained only in process memory.

### Keychain and Key Generator

Keychain stores SSH and OpenPGP keys and passwords. Its identity view lists stored SSH and OpenPGP identities, user IDs, primary and subkey fingerprints, key versions, and declared, derived and effective C/S/A/E capabilities. It provides public/private import and export, deletion, OpenPGP primary-key self-revocation, detached file signatures and verification. Signing can use a valid signing subkey, including with a certification-only primary. SSH signatures use namespace `file` (files up to 64 MiB); signature support depends on the imported private-key format. Private exports can contain unencrypted key material. Deleting a key does not revoke it.

Generation lives in **Key Generator**, opened from Keychain shortcuts for SSH, OpenPGP/GPG and passwords. Every generated SSH type, including explicitly confirmed legacy types, can be saved to Keychain. OpenPGP generation accepts a name, optional email/comment/passphrase, expiration and C/S/A/E choices compatible with the selected format and algorithm. Generation displays its actual phase and elapsed time. The five profiles generate their actual packet formats:

| Profile | Format and algorithms |
|---|---|
| **V6 — default** | [RFC 9580](https://www.rfc-editor.org/rfc/rfc9580.html); Ed25519/X25519 or RSA 3072/4096 |
| **V5 — alternative** | [LibrePGP draft-koch-librepgp-06](https://www.ietf.org/archive/id/draft-koch-librepgp-06.txt); distinct V5 keys, signatures and fingerprints, Ed25519Legacy/Curve25519Legacy, RSA 3072/4096 or DSA; requires compatible recipient software |
| **V4 — compatibility** | [RFC 4880](https://www.rfc-editor.org/rfc/rfc4880.html); Ed25519Legacy/Curve25519Legacy, RSA 3072/4096 or DSA |
| **V3 — legacy** | Historical [RFC 2440](https://www.rfc-editor.org/rfc/rfc2440.html) RSA 1024/2048; optional validity at creation rounded up to whole days |
| **V2 — legacy** | [RFC 1991](https://www.rfc-editor.org/rfc/rfc1991.html) PGP 2.x RSA 1024/2048; unlimited validity in this profile |

V2/V3 require explicit confirmation of obsolete MD5/IDEA, an ASCII identity/passphrase, and use derived C/S/E capabilities without A or subkeys. They are for historical compatibility; modern GnuPG may reject them. V5 is a separate LibrePGP alternative, not an alias for V6 or part of final RFC 9580. Its private exports use AES-256 CFB with iterated/salted SHA-256 S2K and the V5 secret-packet structure. V4/V6 protection is unchanged.

V4 and LibrePGP V5 also offer DSA with four exact parameter profiles: **1024/160 + SHA-1**, **2048/224 + SHA-224**, **2048/256 + SHA-256**, and **3072/256 + SHA-256** (p bits / q bits). DSA is restricted to certification/signing (C/S); use a separate encryption subkey for E. The 1024/160 + SHA-1 option is historical and may require explicit legacy allowances in other software. V6 does not offer DSA, and Core rejects V6 DSA generation, signing and verification. A shared bundled algorithm catalog controls the Generator options, stored-Keychain editor and backend validation. Actual p/q sizes and the profile hash are shown in Keychain metadata. This OpenPGP functionality is separate from legacy SSH DSA-1024.

V4/V5/V6 support up to eight subkeys during generation. Generated and stored private identities support adding and revoking subkeys and changing primary/subkey expiration without changing the primary fingerprint. Open a stored identity with **Subkeys and expiration…** in Keychain; these controls live in Key Generator. Historical V2/V3 validity cannot be edited after generation. All five profiles provide private/public import and export, detached binary file signatures, verification and primary self-revocation. Compatibility-profile file signatures are limited to 64 MiB. C/S/A/E labels appear alongside primary/subkey fingerprints; declared, derived and effective capabilities are shown separately.

All generated files are visible and exportable: private and public ASCII-armored certificates including their subkeys, and a separate `.rev` public revocation certificate. Storage in the encrypted Keychain is optional; generation works while the vault is locked. All generation and local operations run in Rust; GnuPG is used only for independent development tests.

Keychain can publish public OpenPGP V4/V6 certificates to **https://keys.ch-j.de**, check their availability, and publish a cryptographically signed revocation. The service is reachable over IPv4 and IPv6. V2/V3/V5 publishing and lookup are disabled in the application; current server integration supports V4/V6. Private keys stay in the local vault. Publishing is explicit and optional; local key generation and storage continue to work offline. If publishing fails after local revocation, the key remains revoked locally and the UI offers a retry. The client uses verified HTTPS, rejects redirects and confirms the returned fingerprint and revocation.

### Post-quantum and hybrid cryptography

Generator and Keychain share the same Rust backend and offer a **Post-Quantum / Hybrid** category. OpenPGP generation and import retain the actual packet version and algorithm ID; unsupported version/algorithm combinations are rejected before generating keys.

| Algorithm | ID | OpenPGP profile | Capabilities |
| --- | --- | --- | --- |
| ML-DSA-65 + Ed25519 | 30 | V6, RFC 9980 | C/S/A |
| ML-DSA-87 + Ed448 | 31 | V6, RFC 9980 | C/S/A |
| SLH-DSA-SHAKE-128s | 32 | V6, RFC 9980 | C/S/A |
| SLH-DSA-SHAKE-128f | 33 | V6, RFC 9980 | C/S/A |
| SLH-DSA-SHAKE-256s | 34 | V6, RFC 9980 | C/S/A |
| ML-KEM-768 + X25519 | 35 | V6; V4 encryption subkeys only | E |
| ML-KEM-1024 + X448 | 36 | V6 | E |

These implement [RFC 9980](https://www.rfc-editor.org/rfc/rfc9980.html) using rPGP and RustCrypto primitives. Hybrid signatures require both components. Operations include protected private/public export and import, detached and inline file signatures, cleartext signatures, message encryption/decryption, certification of the generated User ID, and primary/subkey/User ID revocation. Keychain provides native file operations; Generator provides text operations for generated and selected stored identities. Quick Actions use the same signing backend, including explicitly selected PQ signing subkeys. C/S/A/E flags describe key capabilities; hardware-agent authentication with OpenPGP keys is not implemented.

V6 certificates advertise SEIPDv2 and their actual cipher preferences. Cryptographic work runs in Rust workers, with generation phases and elapsed time in the UI. Cancel and close discards the operation's result; a library primitive already running finishes before its next cancellation checkpoint. Input and decompression sizes are bounded (64 MiB file/message operations; 1 MiB text input). Export uses atomic writes. Vault generation, selected-key revision and owner checks prevent a completed operation from writing through stale authorization. V2/V3 and LibrePGP V5 keep their separate implementations. LibrePGP draft-06 Kyber algorithm 8 is not implemented as an alias for RFC 9980 IDs 35/36, and no V5 PQ generation is offered.

SSH supports the final OpenSSH 10.6 **`ssh-mldsa44-ed25519`** authentication/signature key, protected OpenSSH private exports, public/private import and file signatures. The key is independent of the connection's key exchange. Server profiles and manually entered targets/jumps offer **prefer hybrid, allow classical fallback** or **hybrid only**. Actual KEX, authentication key, host-key algorithm and rekey count are shown separately for each hop; PQ key exchange alone does not mean PQ authentication.

The three [RFC 10042](https://www.rfc-editor.org/rfc/rfc10042.html) exchanges are `mlkem768x25519-sha256`, `mlkem768nistp256-sha256`, and `mlkem1024nistp384-sha384`. They use the shared SSH session for terminal and SFTP, including rekey. Saved-hop policy changes invalidate fresh logins using cached routes; vault locking/global disconnect cancels pending fresh logins and invalidates their snapshots across unlocks. SNTRUP KEX is not offered. SSH-agent use for the new key is not implemented.

Interoperability was verified using the published RFC 9980 vectors, Sequoia 2.4/OpenSSL 3.6 (all seven algorithms, protected exports, bidirectional signatures/encryption and revocations), OpenSSH 10.6 (composite keys, authentication, host verification, terminal/SFTP/rekey and fallback), and AsyncSSH/liboqs (all three hybrid KEX and rekey). These are independent development tools, not runtime dependencies. Tested GnuPG 2.5.24 with `--allow-9980` did **not** provide interoperable support for the new V6 signature profiles or the protected secret exports; compatibility with it is not claimed.

CH-J Keys PQ publication is prepared but production-server acceptance has not been verified. Uploads contain only verified public material. For new V6 PQ certificates, email User IDs are omitted from the upload; self-certification does not establish email ownership. A V4 PQ certificate with an email identity is stopped before upload and requires the server's explicit identity-verification workflow. Local certificates remain intact. Unsupported server formats are distinguished from local cryptographic verification failures.

Hardware tokens, arbitrary keyserver/WKD publishing, certification/Web of Trust, TSA, SSH CA/KRL are not implemented. Hagrid distributes OpenPGP certificates; SSH identities stay local. Email-based discovery requires the owner to verify their email address; CH-J Keys sends these verification messages through the existing mail service. Publishing a key or verifying an email does not certify a real-world identity. The generator and Keychain work locally without a CH-J server.

The vault password cannot be recovered. **Forgot password / reset vault** deletes encrypted server profiles, saved SSH passwords, trusted host fingerprints, and other encrypted data. Update settings and installed plugins are preserved.

Private-key passphrases are intentionally not stored and must be entered for each connection. An SSH account password can optionally be stored in the encrypted vault.

## Security, encryption, backups and license

Update verification is performed in the Rust Core and fails closed. The application downloads the selected artifact and its detached `.asc` signature, checks the expected size and SHA-512 hash, and verifies the signature with the bundled CH-J public key. Immediately before installation, it re-checks the same private-cache files to reduce time-of-check/time-of-use risk. The renderer cannot supply an artifact path, public key, fingerprint, or a forged verification result.

The long-term update trust anchor is the primary OpenPGP fingerprint:

```text
0D92 778A D8EC F85C 80E3  9248 48F2 433A D9CD F453
```

To verify a download manually, obtain the bundled [public key](https://tools.ch-j.eu/ch-j-signing-public.asc), check that its primary fingerprint matches the value above, and import it with `gpg --import ch-j-signing-public.asc`. Verify the downloaded manifest with `gpg --verify SHA512SUMS.asc SHA512SUMS`, then check the files with `shasum -a 512 -c SHA512SUMS` (macOS) or `sha512sum -c SHA512SUMS` (Linux). Windows PowerShell provides `Get-FileHash -Algorithm SHA512 <file>`. Each installer also has a detached `.asc` signature that can be checked with `gpg --verify <file>.asc <file>`.

Valid signing subkeys may be rotated as long as they remain cryptographically bound to this primary key and are valid for signing. Revoked, expired, unknown, malformed, or otherwise invalid keys and signatures block installation.

CH-J uses two separate groups of OpenPGP keys:

| Purpose | Primary key (SC) | Signing subkey (S) | Public key |
|---|---|---|---|
| Application updates, release signatures and the APT repository | `0D92 778A D8EC F85C 80E3  9248 48F2 433A D9CD F453` | `F3EA 1875 CBCF 63C8 9B2E  D897 4402 8841 DE4E C7E4` (held on a YubiKey) | [ch-j-signing-public.asc](https://tools.ch-j.eu/ch-j-signing-public.asc) |
| Signature embedded directly in the `.deb` package | `BC75 DC9E 5757 88C7 4189  5E82 D7F2 C5F4 7535 F7D7` | `F503 5C4D 5F8E 139C 507B  00CA DEB3 6BB0 DE62 F640` | [ch-j-deb-signing-public.asc](https://tools.ch-j.eu/ch-j-deb-signing-public.asc) |

The second group, `support@sm.ch-j.de`, signs only the package itself, as a `_gpgorigin` member in the debsigs format. It is not trusted for application updates or the APT repository, and it cannot replace a signature from the first group. `dpkg` and `apt` do not check this embedded signature by default; APT trust comes from the signed repository.

When connecting to a server for the first time, verify the displayed SSH host-key fingerprint through another trusted channel. If a known host key changes, verify both the old and new fingerprints before accepting the replacement.

### Vault and platform security

Optional Touch ID, Windows Hello (including PIN) and Ubuntu fingerprint unlock integrate with the existing encrypted Vault, with master-password fallback, localized settings, inactivity locking and optional focus locking. Windows and Linux provide convenience unlock; macOS seals the Vault key to a Touch ID-gated Secure Enclave key. **Locking disconnects SSH sessions and closes plugins.** Native hardware authentication still needs interactive verification. See [setup, dependencies, platform security differences and verification](https://tools.ch-j.eu/en/documentation/security-and-backups/).

On macOS, `EHOSTUNREACH` for local SSH servers may require allowing **CH-J Tools** under **System Settings → Privacy & Security → Local Network**, then restarting the app. This build declares the usage reason and explains the error. Ubuntu packages include standard icon sizes and a matching desktop/window identity.

### Backups and data loss

Create and test appropriate backups of remote data and connection information before risky operations or beta updates. Verify that restoration works. The application does not guarantee that backups exist or are recoverable. Profile export transfers connection metadata only; it is not a backup of the encrypted Vault, saved credentials or remote files.

### License and mandatory acceptance

This edition of CH-J Tools is expressly distributed under the **CH-J Proprietary Software License, version 1.14** (October 11, 2026). See the [complete license](https://tools.ch-j.eu/en/license/). Commercial and non-commercial use are free of license fees indefinitely under its terms. Third-party materials retain their separate licenses and notices; this application license does not replace them.

Website downloads start immediately with the Download button, without a confirmation page. License and operational guidance remain available on the website and in the app. Interactive Windows NSIS installation displays the guidance before the complete license and requires license acceptance. macOS DMG, Debian/Ubuntu and silent installation defer guidance and license acceptance to first launch; the application displays the guidance before license acceptance at first launch. Every platform requires per-user acceptance before using a new or updated build, including updates with the same version number. The required checkbox combines license consent with acknowledgment of data-loss risks, the need for appropriate backups before risky operations, and the fact that the application does not guarantee backup existence or recoverability. Declining exits the application. The acceptance record is local; no activation or reporting is required. See [license acceptance and packaging](https://tools.ch-j.eu/en/license/).

The application bundles [documentation](https://tools.ch-j.eu/en/documentation/), including [Provozní bezpečnost a zálohování (česky)](https://tools.ch-j.cz/cs/source/?path=docs%2Fprovozni-bezpecnost-a-zalohovani.md), [Operational Safety and Backup Guidance (English)](https://tools.ch-j.eu/en/source/?path=docs%2Foperational-safety-and-backup-guidance.md) and [Betriebssicherheit und Datensicherung (Deutsch)](https://tools.ch-j.de/de/source/?path=docs%2Fbetriebssicherheit-und-datensicherung.md). Displaying these recommendations is mandatory before proceeding; following them remains optional. They are informational and non-binding, are not part of the license and do not require separate acceptance. The application displays them again before license acceptance after every update, or if the bundled guidance changes. They are available in the application under **Settings → Licenses and documentation** and on the [website license page](https://tools.ch-j.eu/en/license/).

## Documentation and source catalog

[Browse the application source](https://tools.ch-j.cz/cs/source/) on the CH-J website, with directories, file views, line links and a complete source archive. Source availability does not grant rights beyond the [CH-J Proprietary Software License 1.14](https://tools.ch-j.eu/en/license/). The [GitHub repository](https://github.com/ch-j-code/CH-J-Servermanager) contains only this README; the source and release downloads are hosted on the CH-J website.

- [Getting started and documentation](https://tools.ch-j.eu/en/documentation/)
- [SSH, SFTP and remote editing](https://tools.ch-j.eu/en/documentation/ssh-and-sftp/)
- [Server Diagnostics](https://tools.ch-j.eu/en/documentation/server-diagnostics/)
- [Security and backups](https://tools.ch-j.eu/en/documentation/security-and-backups/)
- [Updates, signatures and APT setup](https://tools.ch-j.eu/en/documentation/updates-and-gpg/)
- [Desktop source catalog — Czech](https://tools.ch-j.cz/cs/source/)
- [Desktop source catalog — English](https://tools.ch-j.eu/en/source/)

The source browser provides directories, file views, line links and release archives. This GitHub repository contains only `README.md`; application paths mentioned here refer to the separately hosted sources.

Resource measurements for Beta 0.1.2 are published in the **Resource usage** article on the [official website](https://tools.ch-j.eu/en/).

## CH-J Ecosystem — Desktop & Mobile

Two independent applications share the CH-J server-management focus. Each connects directly to servers; neither requires the other application or a CH-J account. CH-J Tools Mobile is a separate Flutter application for phones and tablets.

| | CH-J Tools | CH-J Tools Mobile |
| --- | --- | --- |
| Environment | Rust + Tauri 2 desktop application | Flutter / Dart mobile application |
| Platforms | Windows, macOS and Linux; current package targets are listed above | Android 11+ and iOS/iPadOS 15+; phone and tablet layouts |
| Server access | Multiple SSH sessions, interactive terminals, SFTP and Monaco remote editing | One active SSH session, PTY terminal and SFTP browsing/transfers; no remote text editor |
| Administration | Diagnostics, local Hash & Checksum and first-party management tools | Foreground Linux monitoring and IPv4/IPv6 DNS/TCP diagnostics; no desktop plugin system or local hashing tool |
| Profile exchange | JSON version 1 and desktop CSV | Shared JSON version 1; no CSV |
| Status | Beta `0.1.2` | Development alpha `0.0.1`; Android packages and unsigned iOS development artifacts on the official website |
| Project README | [Desktop on GitHub](https://github.com/ch-j-code/CH-J-Servermanager) | [CH-J Tools Mobile on GitHub](https://github.com/ch-j-code/SM-Mobile) |
| Official website | [Desktop](https://tools.ch-j.cz/cs/) | [Mobile](https://tools.ch-j.cz/cs/?edition=mobile) |
| Source catalog | [Desktop sources](https://tools.ch-j.cz/cs/source/) | [Mobile sources](https://tools.ch-j.cz/cs/source/?edition=mobile) |

The shared `chj-server-profiles` JSON format, version 1, transfers only profile name, host, username and port. Passwords, private keys, passphrases and host-key fingerprints are excluded. Configure authentication and verify host keys after import. The two Vault formats are separate and cannot be exchanged.

CH-J Tools Mobile's tablet layout is implemented, while physical iPad validation remains outstanding. Its diagnostics do not provide ICMP or traceroute, and monitoring stops outside the foreground. The website lists a signed Android APK for installation and an AAB for distribution tooling. The iOS IPA requires your own Apple signing and provisioning; the simulator ZIP cannot be installed on a phone. See the [mobile README](https://github.com/ch-j-code/SM-Mobile#readme) for capabilities, distribution and validation limits.

## Official websites

- [CH-J Tools — Czech](https://tools.ch-j.cz/cs/)
- [CH-J Tools — German](https://tools.ch-j.de/de/)
- [CH-J Tools — English](https://tools.ch-j.eu/en/)
- [CH-J Tools Mobile — official website](https://tools.ch-j.cz/cs/?edition=mobile)
- [CH-J Tools Mobile — downloads and build details](https://tools.ch-j.cz/cs/releases/?edition=mobile)

## Project

CH-J Tools uses a Rust Core, a JavaScript user interface, a typed API layer and first-party modules. It supports independently distributed module packages through Module API `1.15.0`. Application source is available in the [official source catalog](https://tools.ch-j.cz/cs/source/).

### Topics

[#ssh](https://github.com/topics/ssh) · [#sftp](https://github.com/topics/sftp) · [#server-management](https://github.com/topics/server-management) · [#hash-calculator](https://github.com/topics/hash-calculator) · [#checksum](https://github.com/topics/checksum) · [#file-integrity](https://github.com/topics/file-integrity) · [#sha256](https://github.com/topics/sha256) · [#sha512](https://github.com/topics/sha512) · [#sha3](https://github.com/topics/sha3) · [#blake3](https://github.com/topics/blake3) · [#xxhash](https://github.com/topics/xxhash) · [#linux](https://github.com/topics/linux) · [#macos](https://github.com/topics/macos) · [#windows](https://github.com/topics/windows)
