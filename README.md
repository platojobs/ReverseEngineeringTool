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
