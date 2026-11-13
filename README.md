# Reverse Engineering Tool Index

[中文版](README.zh-CN.md) | English

A verified index of open-source tooling for reverse engineering **Android**, **iOS**, and **Flutter (Dart snapshot / AOT)** applications. Every entry points to a public GitHub repository, and every description is limited to what the project documents or demonstrably does.

- **Data snapshot:** 2026-11-13 13:45 CST. Stars and last-push dates were read from the GitHub REST API at that time. Star counts are rounded and change daily — treat them as a popularity signal, not a quality score.
- **Inclusion rule:** public repository, available source, and a concrete role in at least one pipeline stage (unpacking, static analysis, dynamic instrumentation, traffic interception, dumping/decryption, repackaging, snapshot analysis).
- **Archived or frozen projects are marked.** They are kept because they still work on legacy targets, not because they are maintained.

## Pipeline map

| Stage | Output | Tooling in this index |
|---|---|---|
| Acquisition | APK, IPA, decrypted IPA | `adb`, [libimobiledevice](https://github.com/libimobiledevice/libimobiledevice), [Clutch](https://github.com/KJCracks/Clutch), [frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump) |
| Identification | Which packer/protector is in use | [APKiD](https://github.com/rednaga/APKiD) |
| Unpacking | Original DEX out of a shell | [BlackDex](https://github.com/CodingGay/BlackDex), [FART](https://github.com/hanbinglengyue/FART) |
| Static analysis | Readable code, resources, headers | [jadx](https://github.com/skylot/jadx), [Apktool](https://github.com/iBotPeaches/Apktool), [Ghidra](https://github.com/NationalSecurityAgency/ghidra), [radare2](https://github.com/radareorg/radare2) |
| Dynamic instrumentation | Hooked methods, bypassed checks | [Frida](https://github.com/frida/frida), [objection](https://github.com/sensepost/objection), [LSPosed](https://github.com/LSPosed/LSPosed) |
| Traffic interception | Plaintext HTTP(S) | [mitmproxy](https://github.com/mitmproxy/mitmproxy), [reFlutter](https://github.com/Impact-I/reFlutter) (Flutter) |
| Decryption / dumping | Decrypted Mach-O, IPA | [frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump), [Clutch](https://github.com/KJCracks/Clutch), [bfdecrypt](https://github.com/BishopFox/bfdecrypt) |
| Repackaging | Patched, signed artifact | [Apktool](https://github.com/iBotPeaches/Apktool), [uber-apk-signer](https://github.com/patrickfav/uber-apk-signer), [ios-app-signer](https://github.com/DanTheMan827/ios-app-signer) |

## Android

An APK is a ZIP containing DEX bytecode and resources. Work usually starts with resource decoding and DEX decompilation, then moves to runtime when packers, integrity checks, or native code block static reading.

| Repository | Tool | Stage | Platform | ★ | Notes |
|---|---|---|---|---|---|
| [iBotPeaches/Apktool](https://github.com/iBotPeaches/Apktool) | Apktool | Decode / rebuild | APK (JVM host) | 27.2k | Decodes manifest and resources to near-original form and rebuilds after smali edits; the de facto starting point for resource-level patching. |
| [skylot/jadx](https://github.com/skylot/jadx) | jadx | Static analysis | DEX / APK (JVM host) | 51.8k | DEX→Java decompiler with GUI and CLI, smali debugger, multi-dex and deobfuscation helpers. |
| [pxb1988/dex2jar](https://github.com/pxb1988/dex2jar) | dex2jar | Static analysis | DEX ↔ class | 13.2k | Converts DEX to JVM class files and ships dex→smali wrappers; use when the downstream tool only accepts bytecode. |
| [JesusFreke/smali](https://github.com/JesusFreke/smali) | smali / baksmali | Disassembly / reassembly | DEX | 6.6k | Reference assembler/disassembler used by Apktool. Upstream archived in 2024, so fixes now come from forks. |
| [androguard/androguard](https://github.com/androguard/androguard) | Androguard | Static analysis / automation | Python | 6.4k | Library and CLI for manifest, permission, certificate, DEX and call-graph analysis; built for scripted pipelines. |
| [google/android-classyshark](https://github.com/google/android-classyshark) | ClassyShark | Static analysis | APK / DEX / JAR | 7.5k | Bytecode browser for classes and methods. Archived in 2023; still fine for quick triage. |
| [rednaga/APKiD](https://github.com/rednaga/APKiD) | APKiD | Identification | APK / DEX | 2.6k | Fingerprints packers, protectors, obfuscators and compilers ("PEiD for Android"), which decides whether unpacking is needed. |
| [ev-flow/quark-engine](https://github.com/ev-flow/quark-engine) | Quark-Engine | Behaviour analysis | Python | 1.7k | Rule- and weight-based scoring of app behaviour, aimed at malware triage and automated reporting. |
| [APKLab/APKLab](https://github.com/APKLab/APKLab) | APKLab | Workbench | VS Code | 4.1k | Wires Apktool, jadx, smali and signing into one VS Code edit-build-test loop. |
| [frida/frida](https://github.com/frida/frida) | Frida | Dynamic instrumentation | Android / iOS / desktop | 22.8k | Scriptable JS instrumentation: hook Java and native code, read memory, replace return values. Base layer for most runtime bypasses. |
| [sensepost/objection](https://github.com/sensepost/objection) | objection | Dynamic instrumentation | Android / iOS | 9.5k | Frida-powered runtime exploration with ready-made SSL-pinning and root/jailbreak-detection bypasses, filesystem access and method invocation. |
| [LSPosed/LSPosed](https://github.com/LSPosed/LSPosed) | LSPosed | Hooking framework | Android (Zygisk) | 25.5k | Xposed-compatible module loader for current Android; needed to run legacy hook modules. |
| [ac-pm/Inspeckage](https://github.com/ac-pm/Inspeckage) | Inspeckage | Dynamic analysis | Android (Xposed) | 3.0k | Runtime API hooks, crypto/file/network logging, launching of unexported activities. No upstream pushes since 2020; expect legacy compatibility. |
| [Fuzion24/JustTrustMe](https://github.com/Fuzion24/JustTrustMe) | JustTrustMe | TLS bypass | Android (Xposed) | 5.4k | Disables certificate validation in common HTTP stacks. Legacy module; usually replaced by Frida/objection scripts today. |
| [MobSF/Mobile-Security-Framework-MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF) | MobSF | Automated analysis | Android / iOS / Windows | 22.2k | All-in-one static and dynamic analysis with reporting; the fastest baseline triage for an unknown app. |
| [CodingGay/BlackDex](https://github.com/CodingGay/BlackDex) | BlackDex | Unpacking | Android 5.0–12 | 6.5k | On-device DEX dumping for packed apps; the project lists Android 5.0–12 support and no external environment requirement. |
| [hanbinglengyue/FART](https://github.com/hanbinglengyue/FART) | FART | Unpacking | Android 6.0 ART | 2.7k | Active-invocation unpacking scheme with a reference implementation; useful for studying ART-level dump points. |
| [mitmproxy/mitmproxy](https://github.com/mitmproxy/mitmproxy) | mitmproxy | Traffic interception | Cross-platform host | 46.9k | TLS-capable intercepting proxy with CLI, TUI, web UI and Python addons; effective only after pinning is bypassed. |
| [patrickfav/uber-apk-signer](https://github.com/patrickfav/uber-apk-signer) | uber-apk-signer | Repackaging / signing | JVM host | 2.8k | Signs and zip-aligns APKs with debug or release keys (v1/v2/v3) and verifies the result. |

## iOS

App Store binaries are FairPlay-encrypted Mach-O; the tools below assume a jailbroken device or an already-decrypted binary. Swift and Objective-C analysis is mostly done in Ghidra or radare2/Rizin, with LLDB for runtime work.

| Repository | Tool | Stage | Platform | ★ | Notes |
|---|---|---|---|---|---|
| [NationalSecurityAgency/ghidra](https://github.com/NationalSecurityAgency/ghidra) | Ghidra | Static analysis | Mach-O, cross-platform host | 99.8k | SRE suite with disassembly, decompiler and scripting; handles Mach-O and AArch64 and is the usual choice for Swift/Objective-C and Flutter engine internals. |
| [radareorg/radare2](https://github.com/radareorg/radare2) | radare2 | Static / dynamic analysis | Mach-O, ELF | 25.4k | CLI-first framework: disassembly, patching, debugging, binary diffing, scripting; broad architecture and format coverage. |
| [rizinorg/rizin](https://github.com/rizinorg/rizin) | Rizin | Static analysis | Mach-O, ELF | 4.2k | radare2-derived framework with reworked API and defaults; the Cutter GUI is built on it. |
| [nygard/class-dump](https://github.com/nygard/class-dump) | class-dump | Static analysis | Mach-O | 3.6k | Generates Objective-C headers from Mach-O metadata. Unchanged upstream since 2022; pair with Swift demangling for mixed binaries. |
| [lief-project/LIEF](https://github.com/lief-project/LIEF) | LIEF | Binary parsing | Mach-O / ELF / DEX / PE | 5.7k | Parses and modifies headers, load commands, segments and symbols through Python, C++ and Rust APIs; foundation for custom tooling. |
| [tpoechtrager/cctools-port](https://github.com/tpoechtrager/cctools-port) | cctools-port | Static analysis | Linux / *BSD host | 0.8k | Apple cctools (otool, lipo, nm) built for non-Apple hosts, so Mach-O inspection does not require macOS. |
| [DerekSelander/LLDB](https://github.com/DerekSelander/LLDB) | LLDB scripts | Debugging | iOS / macOS LLDB | 1.9k | LLDB aliases, regex commands and Python scripts that speed up runtime inspection of Objective-C and Swift objects. |
| [AloneMonkey/frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump) | frida-ios-dump | Decryption / dumping | Jailbroken iOS | 4.0k | Pulls a decrypted IPA from a jailbroken device over Frida — the fastest route from an installed app to an analysable Mach-O. |
| [KJCracks/Clutch](https://github.com/KJCracks/Clutch) | Clutch | Decryption / dumping | Jailbroken iOS 8+ | 3.8k | High-speed executable dumper that produces IPAs; the project states educational and security-research use. |
| [stefanesser/dumpdecrypted](https://github.com/stefanesser/dumpdecrypted) | dumpdecrypted | Decryption / dumping | Jailbroken iOS | 3.0k | Classic memory dumper for decrypted Mach-O. No upstream changes since 2017; use for older targets. |
| [BishopFox/bfdecrypt](https://github.com/BishopFox/bfdecrypt) | bfdecrypt | Decryption | Jailbroken iOS 11.x | 0.5k | Decrypts App Store apps on iOS 11.x; narrow version coverage, unchanged since 2020. |
| [libimobiledevice/libimobiledevice](https://github.com/libimobiledevice/libimobiledevice) | libimobiledevice | Device access | Host ↔ iOS device | 8.3k | Cross-platform protocol library for pairing, backups, app install/list, syslog and crash logs; the base for device-side automation. |
| [DanTheMan827/ios-app-signer](https://github.com/DanTheMan827/ios-app-signer) | iOS App Signer | Repackaging / signing | macOS | 6.3k | Re-signs and bundles an IPA that installs on a device; used after patching a decrypted binary. |
| [axi0mX/ipwndfu](https://github.com/axi0mX/ipwndfu) | ipwndfu | Bootrom research | Older iPhone hardware | 7.5k | Open-source bootrom exploitation toolkit; device coverage is chipset-bound, so check the support list before relying on it. |
| [palera1n/palera1n](https://github.com/palera1n/palera1n) | palera1n | Jailbreak | A8–A11 and T2, iOS 15+ | 6.7k | Semi-tethered jailbreak; the prerequisite for dump and decrypt workflows on supported hardware. |
| [mvt-project/mvt](https://github.com/mvt-project/mvt) | MVT | Forensics | iOS / Android | 16.0k | Mobile Verification Toolkit: analyses backups and filesystem dumps for compromise indicators. |

## Flutter / Dart

Release builds ship a Dart AOT snapshot inside `libapp.so` (Android) or `App.framework/App` (iOS). Object layout and snapshot hashes change between Dart releases, so snapshot parsers are pinned to version ranges and engine patches are pinned to build hashes. Identify the Dart/Flutter version first, then pick the tool.

| Repository | Tool | Stage | Platform | ★ | Notes |
|---|---|---|---|---|---|
| [worawit/blutter](https://github.com/worawit/blutter) | blutter | AOT snapshot analysis | Android arm64 `libapp.so` | 2.8k | Compiles a matching Dart AOT runtime and uses it to parse `libapp.so` into Dart-level pseudo-code with class and object listings. Project notes Android arm64 and recent Dart versions only. |
| [Impact-I/reFlutter](https://github.com/Impact-I/reFlutter) | reFlutter | Engine patching / traffic interception | Android arm64/arm32/x64, iOS arm64 | 2.8k | Swaps in or instruments a patched `libflutter.so`: disables BoringSSL verification for proxy interception and can emit `dump.dart` with function/class names and code offsets for IDA, Ghidra and Frida. Coverage is keyed by snapshot hash. |
| [mildsunrise/darter](https://github.com/mildsunrise/darter) | darter | Snapshot parsing | Dart binary snapshots | 0.3k | Python parser for old/AppJIT/AppAOT snapshots: object graph, string tables, radare2 metadata export. The project warns that newer Dart versions likely fail to parse. |
| [rscloura/Doldrums](https://github.com/rscloura/Doldrums) | Doldrums | Snapshot parsing | Android `libapp.so`, Dart 2.10 | 0.9k | Beta parser that dumps classes and method code offsets from the isolate snapshot; limited to Dart 2.10 releases. |
| [dart-lang/sdk](https://github.com/dart-lang/sdk) | Dart SDK | Format reference | Dart VM / compiler | 11.3k | Upstream VM and compiler source — the ground truth for snapshot serialization and AOT code layout when a parser needs fixing. |
| [flutter/flutter](https://github.com/flutter/flutter) | Flutter | Engine mapping | Flutter SDK | 181k | Engine revision and artifact mapping; use it to tie an observed `libflutter.so` to a known engine build before patching. |
| [Guardsquare/flutter-re-demo](https://github.com/Guardsquare/flutter-re-demo) | flutter-re-demo | Reference experiments | Flutter apps | 0.2k | Guardsquare's published experiments on the feasibility of Flutter app reverse engineering; a useful baseline for what obfuscation does and does not hide. |

### Flutter workflow notes

1. Extract `libapp.so` / `App.framework` from the APK or IPA and record the `libflutter.so` build fingerprint.
2. Match the fingerprint to a Flutter engine release, then choose a tool that supports that Dart version (`blutter`, `darter`, `Doldrums`) or that engine hash (`reFlutter`).
3. Recover symbol names and code offsets, then import them into Ghidra/IDA or hook the offsets with Frida.
4. For TLS inspection, prefer the engine-patching route (`reFlutter`) before attempting certificate installation, because Flutter does not use the platform trust store.

## 2020 historical snapshot

> **Historical data, cut-off 2020-12-31 23:59:59 UTC.** This section records what can still be verified about 2020 and does not describe the projects as they are today. For current stars and activity see [Android](#android), [iOS](#ios) and [Flutter / Dart](#flutter--dart), whose snapshot is 2026-11-13.

### Method

| Field | Source |
|---|---|
| Existence in 2020 | `created_at` from `GET /repos/{owner}/{repo}` |
| Last commit on or before 2020-12-31 | `GET /repos/{owner}/{repo}/commits?until=2020-12-31T23:59:59Z&per_page=1` |
| Latest release on or before 2020-12-31 | Newest entry in `GET /repos/{owner}/{repo}/releases` with `created_at` before the cut-off |
| Stars | Archived repository page from the Wayback Machine; the archive date is shown in the same cell |

Limits: star figures are exact counts published on the archived page, not estimates, but the archive date is not always 31 December 2020. `unknown` means no 2020 archive page could be recovered, or the archive rate-limited the request, and no value was inferred. Repositories renamed since 2020 are listed under their current path; history before the rename is read through GitHub redirects. `—` means the project had no GitHub Release before the cut-off.

### Present in 2020

| Repository | Tool | Last commit ≤ 2020-12-31 | Latest release ≤ 2020 | ★ (archive date) | State in 2020 |
|---|---|---|---|---|---|
| [iBotPeaches/Apktool](https://github.com/iBotPeaches/Apktool) | Apktool | 2020-12-11 | v2.5.0 / 2020-12-02 | 11,121 (2020-12-28) | v2.5.0 shipped that month; already the default resource decode/rebuild tool. |
| [skylot/jadx](https://github.com/skylot/jadx) | jadx | 2020-12-31 | v1.2.0 / 2020-11-14 | 24,638 (2020-12-28) | Active throughout the year; v1.2.0 in November. |
| [pxb1988/dex2jar](https://github.com/pxb1988/dex2jar) | dex2jar | 2020-06-24 | 2.1-nightly-28 / 2018-04-02 | 8,217 (2020-12-17) | Maintained in 2020, but the newest artifact was still a 2018 nightly. |
| [JesusFreke/smali](https://github.com/JesusFreke/smali) | smali / baksmali | 2020-03-25 | — | 4,719 (2020-12-17) | Effectively frozen; consumed through Apktool. |
| [androguard/androguard](https://github.com/androguard/androguard) | Androguard | 2020-11-24 | v3.4.0a1 / 2020-04-16 | 3,302 (2020-12-08) | Python analysis library; only a 3.4 pre-release existed. |
| [google/android-classyshark](https://github.com/google/android-classyshark) | ClassyShark | 2020-07-28 | 8.2 / 2018-07-25 | 6,615 (2020-12-05) | Development had slowed; the repository was archived later, in 2023. |
| [rednaga/APKiD](https://github.com/rednaga/APKiD) | APKiD | 2020-11-20 | v2.1.1 / 2020-11-12 | unknown | v2.1.1 in November; no 2020 archive page of the repository was recoverable for stars. |
| [ev-flow/quark-engine](https://github.com/ev-flow/quark-engine) | Quark-Engine | 2020-12-09 | v20.12 / 2020-12-07 | 432 (2020-12-19) | Hosted at `quark-engine/quark-engine` at the time and versioned by month (v20.12). |
| [APKLab/APKLab](https://github.com/APKLab/APKLab) | APKLab | 2020-12-27 | — | unknown | Repository created 2020-05; still an early-stage extension with no GitHub Release. |
| [frida/frida](https://github.com/frida/frida) | Frida | 2020-12-30 | 14.2.2 / 2020-12-20 | 6,397 (2020-11-08) | 14.x line with Android and iOS instrumentation. |
| [sensepost/objection](https://github.com/sensepost/objection) | objection | 2020-10-01 | 1.9.6 / 2020-08-13 | 3,160 (2020-12-04) | 1.9.6; Frida-based runtime exploration. |
| [LSPosed/LSPosed](https://github.com/LSPosed/LSPosed) | LSPosed | 2020-12-31 | — | unknown | Created 2020-12-12 only; no release before the cut-off, so practical use begins in 2021. |
| [ac-pm/Inspeckage](https://github.com/ac-pm/Inspeckage) | Inspeckage | 2018-08-24 | v2.4 / 2018-08-24 | 1,981 (2020-12-07) | Already dormant in 2020; no commits since 2018. |
| [Fuzion24/JustTrustMe](https://github.com/Fuzion24/JustTrustMe) | JustTrustMe | 2020-12-08 | v.2 / 2016-01-26 | 2,605 (2020-11-12) | Release frozen at 2016; only minor commits in 2020. |
| [MobSF/Mobile-Security-Framework-MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF) | MobSF | 2020-12-31 | v3.1.1 / 2020-08-05 | 7,872 (2020-12-24) | v3.1.1; already covered both Android and iOS. |
| [hanbinglengyue/FART](https://github.com/hanbinglengyue/FART) | FART | 2020-07-31 | — | 902 (2020-09-21) | ART unpacking research; no GitHub Release. |
| [mitmproxy/mitmproxy](https://github.com/mitmproxy/mitmproxy) | mitmproxy | 2020-12-30 | v6.0.2 / 2020-12-15 | 20,846 (2020-12-06) | v6.0.2. |
| [patrickfav/uber-apk-signer](https://github.com/patrickfav/uber-apk-signer) | uber-apk-signer | 2020-11-29 | v1.2.1 / 2020-11-29 | 617 (2020-12-28) | v1.2.1. |
| [NationalSecurityAgency/ghidra](https://github.com/NationalSecurityAgency/ghidra) | Ghidra | 2020-12-31 | Ghidra_9.2.2_build / 2020-12-29 | 24,092 (2020-12-30) | 9.2.2 at year end; open source since 2019. |
| [radareorg/radare2](https://github.com/radareorg/radare2) | radare2 | 2020-12-31 | 5.0.0 / 2020-12-21 | 13,677 (2020-12-28) | 5.0.0 was the major release of the year. |
| [rizinorg/rizin](https://github.com/rizinorg/rizin) | Rizin | 2020-12-30 | — | 453 (2020-12-27) | Created 2020-09 after the radare2 fork; no release before the cut-off. |
| [nygard/class-dump](https://github.com/nygard/class-dump) | class-dump | 2019-03-15 | 3.4 / 2012-11-19 | 2,811 (2020-11-11) | Legacy already: no commits since 2019, last release from 2012. |
| [lief-project/LIEF](https://github.com/lief-project/LIEF) | LIEF | 2020-12-31 | 0.10.1 / 2019-11-29 | unknown | Actively committed in 2020; star count not recovered because the archive rate-limited the page request. |
| [tpoechtrager/cctools-port](https://github.com/tpoechtrager/cctools-port) | cctools-port | 2020-12-19 | cctools-877.8-ld64-253.9-1 / 2016-06-24 | 438 (2020-12-19) | Still updated in 2020; release tag dates from 2016. |
| [DerekSelander/LLDB](https://github.com/DerekSelander/LLDB) | LLDB scripts | 2020-07-11 | — | 1,107 (2020-11-25) | Script collection; no releases. |
| [AloneMonkey/frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump) | frida-ios-dump | 2020-06-01 | — | 1,751 (2020-12-02) | Frida-based decrypted IPA extraction. |
| [KJCracks/Clutch](https://github.com/KJCracks/Clutch) | Clutch | 2019-11-04 | 2.0.4 / 2016-08-23 | 3,032 (2020-12-10) | Dormant since 2019. |
| [stefanesser/dumpdecrypted](https://github.com/stefanesser/dumpdecrypted) | dumpdecrypted | 2014-02-13 | — | 2,494 (2020-09-30) | Historical dumper, unchanged since 2014. |
| [BishopFox/bfdecrypt](https://github.com/BishopFox/bfdecrypt) | bfdecrypt | 2018-02-07 | — | 321 (2020-11-12) | iOS 11.x only; dormant since 2018. |
| [libimobiledevice/libimobiledevice](https://github.com/libimobiledevice/libimobiledevice) | libimobiledevice | 2020-12-30 | 1.3.0 / 2020-06-15 | 3,673 (2020-12-15) | Active; 1.3.0 shipped in June 2020. |
| [DanTheMan827/ios-app-signer](https://github.com/DanTheMan827/ios-app-signer) | iOS App Signer | 2020-06-22 | 1.13.1 / 2019-12-05 | 3,552 (2020-12-04) | 1.13.1. |
| [axi0mX/ipwndfu](https://github.com/axi0mX/ipwndfu) | ipwndfu | 2019-10-01 | — | 6,065 (2020-12-15) | Bootrom toolkit; dormant since 2019. |
| [mildsunrise/darter](https://github.com/mildsunrise/darter) | darter | 2020-11-27 | — | 21 (2020-11-17) | Existed since 2019-11; an early snapshot parser at this point. |
| [rscloura/Doldrums](https://github.com/rscloura/Doldrums) | Doldrums | 2020-12-16 | — | unknown | Created 2020-11; beta parser limited to Dart 2.10. |
| [dart-lang/sdk](https://github.com/dart-lang/sdk) | Dart SDK | 2020-12-31 | — | 6,185 (2020-11-30) | Reference for the snapshot and AOT formats of that period. |
| [flutter/flutter](https://github.com/flutter/flutter) | Flutter | 2020-12-31 | — | 109,895 (2020-12-31) | Used to map a 2020 `libflutter.so` back to the engine build of the time. |

### Not present in 2020

| Repository | Created on GitHub | Note |
|---|---|---|
| [CodingGay/BlackDex](https://github.com/CodingGay/BlackDex) | 2021-05-21 | Did not exist in 2020. |
| [mvt-project/mvt](https://github.com/mvt-project/mvt) | 2021-07-16 | Did not exist in 2020. |
| [Impact-I/reFlutter](https://github.com/Impact-I/reFlutter) | 2022-04-13 | Current repository created in 2022; the project is not part of the 2020 landscape. |
| [Guardsquare/flutter-re-demo](https://github.com/Guardsquare/flutter-re-demo) | 2022-06-02 | Did not exist in 2020. |
| [palera1n/palera1n](https://github.com/palera1n/palera1n) | 2022-09-17 | Did not exist in 2020. |
| [worawit/blutter](https://github.com/worawit/blutter) | 2023-08-22 | Did not exist in 2020; Flutter AOT parsing tools of that era were `darter` and `Doldrums`. |

## Verification

The star and last-push figures can be reproduced with the GitHub CLI:

```bash
gh api repos/iBotPeaches/Apktool --jq '{stars: .stargazers_count, pushed: .pushed_at}'
```

All repositories in this index were reachable through the GitHub REST API on 2026-11-13. Projects whose upstream is archived are marked in the Notes column.

## Legal and ethical use

Use these tools only on applications you own or are explicitly authorised to assess. Decryption, repackaging and traffic interception may be restricted by law, by platform terms, and by the licence of the target software.

## License

Content of this repository is released under the GNU General Public License v3.0; see [LICENSE](LICENSE). Each listed project keeps its own licence.
