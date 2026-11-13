# 逆向工程工具索引

[English](README.md) | 中文版

一份经过核实、面向 **Android**、**iOS** 与 **Flutter（Dart 快照 / AOT）** 应用逆向的开源工具索引。每条都指向公开的 GitHub 仓库，描述只限于项目自身文档写明或可直接验证的内容。

- **数据快照：** 2026-11-13 13:45 CST。星标数与最近推送时间均为该时刻通过 GitHub REST API 读取。星标数为约数且每日变化，仅作热度参考，不代表质量高低。
- **收录标准：** 仓库公开、源码可用，并在逆向流程的至少一个环节中有明确作用（脱壳、静态分析、动态插桩、抓包、导出解密、重打包、快照分析）。
- **已归档或停止维护的项目会标注。** 收录它们是因为在旧版本目标上依然可用，而非仍在维护。

## 流程映射

| 阶段 | 产物 | 本索引中的工具 |
|---|---|---|
| 获取 | APK、IPA、已解密的 IPA | `adb`、[libimobiledevice](https://github.com/libimobiledevice/libimobiledevice)、[Clutch](https://github.com/KJCracks/Clutch)、[frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump) |
| 识别 | 判断使用了哪种加固/保护 | [APKiD](https://github.com/rednaga/APKiD) |
| 脱壳 | 从壳中还原原始 DEX | [BlackDex](https://github.com/CodingGay/BlackDex)、[FART](https://github.com/hanbinglengyue/FART) |
| 静态分析 | 可读代码、资源、头文件 | [jadx](https://github.com/skylot/jadx)、[Apktool](https://github.com/iBotPeaches/Apktool)、[Ghidra](https://github.com/NationalSecurityAgency/ghidra)、[radare2](https://github.com/radareorg/radare2) |
| 动态插桩 | Hook 方法、绕过检测 | [Frida](https://github.com/frida/frida)、[objection](https://github.com/sensepost/objection)、[LSPosed](https://github.com/LSPosed/LSPosed) |
| 抓包 | 明文的 HTTP(S) 流量 | [mitmproxy](https://github.com/mitmproxy/mitmproxy)、[reFlutter](https://github.com/Impact-I/reFlutter)（Flutter） |
| 导出 / 解密 | 解密后的 Mach-O、IPA | [frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump)、[Clutch](https://github.com/KJCracks/Clutch)、[bfdecrypt](https://github.com/BishopFox/bfdecrypt) |
| 重打包 | 已修改并签名的产物 | [Apktool](https://github.com/iBotPeaches/Apktool)、[uber-apk-signer](https://github.com/patrickfav/uber-apk-signer)、[ios-app-signer](https://github.com/DanTheMan827/ios-app-signer) |

## Android

APK 是包含 DEX 字节码与资源的 ZIP。常规流程是先反编译资源与 DEX，遇到加固、完整性校验或原生代码阻碍静态阅读时再转入运行时分析。

| 仓库 | 工具 | 阶段 | 平台 | ★ | 说明 |
|---|---|---|---|---|---|
| [iBotPeaches/Apktool](https://github.com/iBotPeaches/Apktool) | Apktool | 解码 / 重建 | APK（JVM 主机） | 27.2k | 将清单与资源解码到接近原始形态，并可在修改 smali 后重新打包；资源级修改的事实起点。 |
| [skylot/jadx](https://github.com/skylot/jadx) | jadx | 静态分析 | DEX / APK（JVM 主机） | 51.8k | DEX→Java 反编译器，提供 GUI 与 CLI、smali 调试器、多 dex 与反混淆辅助。 |
| [pxb1988/dex2jar](https://github.com/pxb1988/dex2jar) | dex2jar | 静态分析 | DEX ↔ class | 13.2k | 将 DEX 转为 JVM class，并提供 dex→smali 包装；当下游工具只接受字节码时使用。 |
| [JesusFreke/smali](https://github.com/JesusFreke/smali) | smali / baksmali | 反汇编 / 汇编 | DEX | 6.6k | Apktool 使用的参考汇编/反汇编器。上游已于 2024 年归档，修复由分支承接。 |
| [androguard/androguard](https://github.com/androguard/androguard) | Androguard | 静态分析 / 自动化 | Python | 6.4k | 解析清单、权限、证书、DEX 与调用图的库和 CLI，适合脚本化流水线。 |
| [google/android-classyshark](https://github.com/google/android-classyshark) | ClassyShark | 静态分析 | APK / DEX / JAR | 7.5k | 浏览类与方法的字节码工具。2023 年归档，仍适合快速初筛。 |
| [rednaga/APKiD](https://github.com/rednaga/APKiD) | APKiD | 识别 | APK / DEX | 2.6k | 指纹识别加固壳、保护器、混淆器与编译器（“Android 版 PEiD”），直接决定是否需要脱壳。 |
| [ev-flow/quark-engine](https://github.com/ev-flow/quark-engine) | Quark-Engine | 行为分析 | Python | 1.7k | 基于规则与权重的行为打分引擎，面向恶意样本初筛与自动化报告。 |
| [APKLab/APKLab](https://github.com/APKLab/APKLab) | APKLab | 工作台 | VS Code | 4.1k | 把 Apktool、jadx、smali 与签名串成 VS Code 内的编辑—构建—测试闭环。 |
| [frida/frida](https://github.com/frida/frida) | Frida | 动态插桩 | Android / iOS / 桌面 | 22.8k | 可脚本化（JS）的运行时插桩：Hook Java 与原生代码、读写内存、替换返回值，是多数运行时绕过的底层。 |
| [sensepost/objection](https://github.com/sensepost/objection) | objection | 动态插桩 | Android / iOS | 9.5k | 基于 Frida 的运行时探查，内置 SSL Pinning 与 root/越狱检测绕过、文件系统访问与方法调用。 |
| [LSPosed/LSPosed](https://github.com/LSPosed/LSPosed) | LSPosed | Hook 框架 | Android（Zygisk） | 25.5k | 现行 Android 上的 Xposed 兼容模块加载器，运行旧模块时通常离不开它。 |
| [ac-pm/Inspeckage](https://github.com/ac-pm/Inspeckage) | Inspeckage | 动态分析 | Android（Xposed） | 3.0k | 运行时 API Hook、加解密/文件/网络日志、启动未导出 Activity。上游自 2020 年起无推送，兼容性偏旧。 |
| [Fuzion24/JustTrustMe](https://github.com/Fuzion24/JustTrustMe) | JustTrustMe | TLS 绕过 | Android（Xposed） | 5.4k | 关闭常见 HTTP 栈的证书校验。属于旧方案，目前多由 Frida/objection 脚本替代。 |
| [MobSF/Mobile-Security-Framework-MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF) | MobSF | 自动化分析 | Android / iOS / Windows | 22.2k | 静态与动态分析一体的框架并输出报告，是陌生应用最快的基线初筛。 |
| [CodingGay/BlackDex](https://github.com/CodingGay/BlackDex) | BlackDex | 脱壳 | Android 5.0–12 | 6.5k | 设备端 DEX dump，针对加固应用；项目自述支持 Android 5.0–12 且不依赖额外环境。 |
| [hanbinglengyue/FART](https://github.com/hanbinglengyue/FART) | FART | 脱壳 | Android 6.0 ART | 2.7k | 基于主动调用的脱壳方案与参考实现，适合研究 ART 层的 dump 点。 |
| [mitmproxy/mitmproxy](https://github.com/mitmproxy/mitmproxy) | mitmproxy | 抓包 | 跨平台主机 | 46.9k | 支持 TLS 的拦截代理，含 CLI、TUI、Web UI 与 Python 插件；需先绕过 Pinning 才有效。 |
| [patrickfav/uber-apk-signer](https://github.com/patrickfav/uber-apk-signer) | uber-apk-signer | 重打包 / 签名 | JVM 主机 | 2.8k | 用调试或发布证书对 APK 签名并 zip 对齐（v1/v2/v3），签名后自动校验。 |

## iOS

App Store 分发的二进制是经过 FairPlay 加密的 Mach-O；以下工具假定设备已越狱，或二进制已解密。Swift 与 Objective-C 的静态分析主要在 Ghidra 或 radare2/Rizin 中完成，运行时配合 LLDB。

| 仓库 | 工具 | 阶段 | 平台 | ★ | 说明 |
|---|---|---|---|---|---|
| [NationalSecurityAgency/ghidra](https://github.com/NationalSecurityAgency/ghidra) | Ghidra | 静态分析 | Mach-O，跨平台主机 | 99.8k | 集成反汇编、反编译与脚本的逆向框架，支持 Mach-O 与 AArch64，是 Swift/Objective-C 及 Flutter 引擎内部逻辑的常用选择。 |
| [radareorg/radare2](https://github.com/radareorg/radare2) | radare2 | 静态 / 动态分析 | Mach-O、ELF | 25.4k | CLI 优先的逆向框架：反汇编、打补丁、调试、二进制比对与脚本，架构与格式覆盖广。 |
| [rizinorg/rizin](https://github.com/rizinorg/rizin) | Rizin | 静态分析 | Mach-O、ELF | 4.2k | 由 radare2 派生、重做 API 与默认行为的框架；Cutter 图形界面基于它构建。 |
| [nygard/class-dump](https://github.com/nygard/class-dump) | class-dump | 静态分析 | Mach-O | 3.6k | 从 Mach-O 的 Objective-C 元数据生成头文件。上游自 2022 年起未更新；混合二进制需配合 Swift 符号还原。 |
| [lief-project/LIEF](https://github.com/lief-project/LIEF) | LIEF | 二进制解析 | Mach-O / ELF / DEX / PE | 5.7k | 通过 Python、C++、Rust API 解析与修改头、加载命令、段和符号，适合搭建自有工具链。 |
| [tpoechtrager/cctools-port](https://github.com/tpoechtrager/cctools-port) | cctools-port | 静态分析 | Linux / *BSD 主机 | 0.8k | 面向非 Apple 主机构建的 Apple cctools（otool、lipo、nm），无需 macOS 即可检查 Mach-O。 |
| [DerekSelander/LLDB](https://github.com/DerekSelander/LLDB) | LLDB 脚本集 | 调试 | iOS / macOS LLDB | 1.9k | LLDB 别名、正则命令与 Python 脚本，加快 Objective-C 与 Swift 对象的运行时检查。 |
| [AloneMonkey/frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump) | frida-ios-dump | 解密 / 导出 | 越狱 iOS | 4.0k | 通过 Frida 从越狱设备导出已解密 IPA，是从已安装应用到可分析 Mach-O 的最快路径。 |
| [KJCracks/Clutch](https://github.com/KJCracks/Clutch) | Clutch | 解密 / 导出 | 越狱 iOS 8+ | 3.8k | 高速可执行文件 dump 工具并打包为 IPA；项目声明仅用于教育与安全研究。 |
| [stefanesser/dumpdecrypted](https://github.com/stefanesser/dumpdecrypted) | dumpdecrypted | 解密 / 导出 | 越狱 iOS | 3.0k | 经典的内存 dump 工具，用于导出解密后的 Mach-O。上游自 2017 年起未变，适用于较旧目标。 |
| [BishopFox/bfdecrypt](https://github.com/BishopFox/bfdecrypt) | bfdecrypt | 解密 | 越狱 iOS 11.x | 0.5k | 在 iOS 11.x 越狱设备上解密 App Store 应用；系统版本覆盖窄，自 2020 年起未更新。 |
| [libimobiledevice/libimobiledevice](https://github.com/libimobiledevice/libimobiledevice) | libimobiledevice | 设备访问 | 主机 ↔ iOS 设备 | 8.3k | 跨平台协议库，支持配对、备份、应用安装/列表、syslog 与崩溃日志，是设备侧自动化的基础。 |
| [DanTheMan827/ios-app-signer](https://github.com/DanTheMan827/ios-app-signer) | iOS App Signer | 重打包 / 签名 | macOS | 6.3k | 重新签名并打包可安装到设备的 IPA，用于修改已解密二进制之后。 |
| [axi0mX/ipwndfu](https://github.com/axi0mX/ipwndfu) | ipwndfu | Bootrom 研究 | 较旧 iPhone 硬件 | 7.5k | 开源 bootrom 利用工具集；支持范围受芯片型号限制，使用前需核对支持列表。 |
| [palera1n/palera1n](https://github.com/palera1n/palera1n) | palera1n | 越狱 | A8–A11 与 T2，iOS 15+ | 6.7k | 半 tethered 越狱，是受支持硬件上进行 dump 与解密流程的前提。 |
| [mvt-project/mvt](https://github.com/mvt-project/mvt) | MVT | 取证 | iOS / Android | 16.0k | Mobile Verification Toolkit：分析备份与文件系统镜像，查找失陷指标。 |

## Flutter / Dart

Release 构建会把 Dart AOT 快照放进 `libapp.so`（Android）或 `App.framework/App`（iOS）。对象布局与快照哈希在不同 Dart 版本之间会变化，因此快照解析工具绑定版本区间，引擎补丁绑定构建哈希。先确定 Dart/Flutter 版本，再选工具。

| 仓库 | 工具 | 阶段 | 平台 | ★ | 说明 |
|---|---|---|---|---|---|
| [worawit/blutter](https://github.com/worawit/blutter) | blutter | AOT 快照分析 | Android arm64 `libapp.so` | 2.8k | 编译与目标版本匹配的 Dart AOT 运行时来解析 `libapp.so`，输出 Dart 层伪代码及类/对象清单。项目注明目前仅支持 Android arm64 与较新 Dart 版本。 |
| [Impact-I/reFlutter](https://github.com/Impact-I/reFlutter) | reFlutter | 引擎替换 / 抓包 | Android arm64/arm32/x64，iOS arm64 | 2.8k | 替换或插桩修改后的 `libflutter.so`：关闭 BoringSSL 证书校验以便代理抓包，并可导出含函数/类名与代码偏移的 `dump.dart`，供 IDA、Ghidra、Frida 使用。支持范围按快照哈希划分。 |
| [mildsunrise/darter](https://github.com/mildsunrise/darter) | darter | 快照解析 | Dart 二进制快照 | 0.3k | Python 解析 old/AppJIT/AppAOT 快照：对象图、字符串表、radare2 元数据导出。项目明确提示新版本 Dart 大概率解析失败。 |
| [rscloura/Doldrums](https://github.com/rscloura/Doldrums) | Doldrums | 快照解析 | Android `libapp.so`，Dart 2.10 | 0.9k | Beta 阶段解析器，从 isolate 快照导出类与方法代码偏移；仅覆盖 Dart 2.10 版本。 |
| [dart-lang/sdk](https://github.com/dart-lang/sdk) | Dart SDK | 格式参考 | Dart VM / 编译器 | 11.3k | 上游 VM 与编译器源码。修复解析器时，快照序列化与 AOT 代码布局的最终依据。 |
| [flutter/flutter](https://github.com/flutter/flutter) | Flutter | 引擎版本映射 | Flutter SDK | 181k | 引擎修订与产物映射；打补丁前先用它把观察到的 `libflutter.so` 对应到已知引擎构建。 |
| [Guardsquare/flutter-re-demo](https://github.com/Guardsquare/flutter-re-demo) | flutter-re-demo | 参考实验 | Flutter 应用 | 0.2k | Guardsquare 公开的 Flutter 应用逆向可行性实验，可用于判断混淆究竟隐藏了什么。 |

### Flutter 流程提示

1. 从 APK 或 IPA 中取出 `libapp.so` / `App.framework`，记录 `libflutter.so` 的构建指纹。
2. 将指纹对应到 Flutter 引擎版本，再选择支持该 Dart 版本的工具（`blutter`、`darter`、`Doldrums`）或支持该引擎哈希的工具（`reFlutter`）。
3. 还原符号名与代码偏移，导入 Ghidra/IDA，或用 Frida 按偏移 Hook。
4. 需要 TLS 抓包时，优先走替换引擎的路线（`reFlutter`），因为 Flutter 不使用系统证书库，安装证书通常无效。

## 2020 年历史快照

> **历史数据，截止 2020-12-31 23:59:59 UTC。** 本节只记录仍可核实的 2020 年状态，不代表这些项目的现状。当前星标与活跃度见上文 [Android](#android)、[iOS](#ios)、[Flutter / Dart](#flutter--dart) 三张表，其数据采集于 2026-11-13。

### 采集口径

| 字段 | 来源 |
|---|---|
| 2020 年是否存在 | `GET /repos/{owner}/{repo}` 的 `created_at` |
| 2020-12-31 前最后一次提交 | `GET /repos/{owner}/{repo}/commits?until=2020-12-31T23:59:59Z&per_page=1` |
| 2020-12-31 前最新发布 | `GET /repos/{owner}/{repo}/releases` 中 `created_at` 早于截止时间的最新一条 |
| 星标 | Wayback Machine 存档的仓库页面，存档日期标注在同一单元格内 |

局限：星标为存档页面当时公布的精确计数，不是估算值，但存档日期不一定正好是 2020-12-31。标为 `未还原` 表示没有找到 2020 年存档，或存档站限流导致无法读取，未做任何推算。2020 年后改名的仓库按当前路径列出，改名前的历史通过 GitHub 重定向读取。`—` 表示截止时间前没有 GitHub Release 记录。

### 2020 年已存在

| 仓库 | 工具 | 最后提交（≤ 2020-12-31） | 最新 Release（≤ 2020） | ★（存档日期） | 2020 年状态 |
|---|---|---|---|---|---|
| [iBotPeaches/Apktool](https://github.com/iBotPeaches/Apktool) | Apktool | 2020-12-11 | v2.5.0 / 2020-12-02 | 11,121（2020-12-28） | 当月发布 v2.5.0，已是资源解码与重建的默认工具。 |
| [skylot/jadx](https://github.com/skylot/jadx) | jadx | 2020-12-31 | v1.2.0 / 2020-11-14 | 24,638（2020-12-28） | 全年活跃，11 月发布 v1.2.0。 |
| [pxb1988/dex2jar](https://github.com/pxb1988/dex2jar) | dex2jar | 2020-06-24 | 2.1-nightly-28 / 2018-04-02 | 8,217（2020-12-17） | 2020 年仍有维护，但最新产物仍是 2018 年的 nightly。 |
| [JesusFreke/smali](https://github.com/JesusFreke/smali) | smali / baksmali | 2020-03-25 | — | 4,719（2020-12-17） | 实际上已冻结，主要通过 Apktool 使用。 |
| [androguard/androguard](https://github.com/androguard/androguard) | Androguard | 2020-11-24 | v3.4.0a1 / 2020-04-16 | 3,302（2020-12-08） | Python 分析库，当时只有 3.4 的预发布版。 |
| [google/android-classyshark](https://github.com/google/android-classyshark) | ClassyShark | 2020-07-28 | 8.2 / 2018-07-25 | 6,615（2020-12-05） | 开发已放缓；仓库于 2023 年归档。 |
| [rednaga/APKiD](https://github.com/rednaga/APKiD) | APKiD | 2020-11-20 | v2.1.1 / 2020-11-12 | 未还原 | 11 月发布 v2.1.1；未找到该仓库 2020 年的存档页，星标无法还原。 |
| [ev-flow/quark-engine](https://github.com/ev-flow/quark-engine) | Quark-Engine | 2020-12-09 | v20.12 / 2020-12-07 | 432（2020-12-19） | 当时位于 `quark-engine/quark-engine`，按月份命名版本（v20.12）。 |
| [APKLab/APKLab](https://github.com/APKLab/APKLab) | APKLab | 2020-12-27 | — | 未还原 | 仓库创建于 2020-05，当时仍是早期插件，无 GitHub Release。 |
| [frida/frida](https://github.com/frida/frida) | Frida | 2020-12-30 | 14.2.2 / 2020-12-20 | 6,397（2020-11-08） | 处于 14.x 系列，已覆盖 Android 与 iOS 动态插桩。 |
| [sensepost/objection](https://github.com/sensepost/objection) | objection | 2020-10-01 | 1.9.6 / 2020-08-13 | 3,160（2020-12-04） | 1.9.6，基于 Frida 的运行时探查。 |
| [LSPosed/LSPosed](https://github.com/LSPosed/LSPosed) | LSPosed | 2020-12-31 | — | 未还原 | 2020-12-12 才创建，截止前无发布，实际可用从 2021 年开始。 |
| [ac-pm/Inspeckage](https://github.com/ac-pm/Inspeckage) | Inspeckage | 2018-08-24 | v2.4 / 2018-08-24 | 1,981（2020-12-07） | 2020 年已停滞，最后提交停在 2018 年。 |
| [Fuzion24/JustTrustMe](https://github.com/Fuzion24/JustTrustMe) | JustTrustMe | 2020-12-08 | v.2 / 2016-01-26 | 2,605（2020-11-12） | Release 停留在 2016 年，2020 年仅有零星提交。 |
| [MobSF/Mobile-Security-Framework-MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF) | MobSF | 2020-12-31 | v3.1.1 / 2020-08-05 | 7,872（2020-12-24） | v3.1.1，当时已同时覆盖 Android 与 iOS。 |
| [hanbinglengyue/FART](https://github.com/hanbinglengyue/FART) | FART | 2020-07-31 | — | 902（2020-09-21） | ART 脱壳研究方案，无 GitHub Release。 |
| [mitmproxy/mitmproxy](https://github.com/mitmproxy/mitmproxy) | mitmproxy | 2020-12-30 | v6.0.2 / 2020-12-15 | 20,846（2020-12-06） | v6.0.2。 |
| [patrickfav/uber-apk-signer](https://github.com/patrickfav/uber-apk-signer) | uber-apk-signer | 2020-11-29 | v1.2.1 / 2020-11-29 | 617（2020-12-28） | v1.2.1。 |
| [NationalSecurityAgency/ghidra](https://github.com/NationalSecurityAgency/ghidra) | Ghidra | 2020-12-31 | Ghidra_9.2.2_build / 2020-12-29 | 24,092（2020-12-30） | 年末为 9.2.2；2019 年开源。 |
| [radareorg/radare2](https://github.com/radareorg/radare2) | radare2 | 2020-12-31 | 5.0.0 / 2020-12-21 | 13,677（2020-12-28） | 当年的大版本为 5.0.0。 |
| [rizinorg/rizin](https://github.com/rizinorg/rizin) | Rizin | 2020-12-30 | — | 453（2020-12-27） | 2020-09 从 radare2 分叉后创建，截止前无 Release。 |
| [nygard/class-dump](https://github.com/nygard/class-dump) | class-dump | 2019-03-15 | 3.4 / 2012-11-19 | 2,811（2020-11-11） | 2020 年已是遗留工具：自 2019 年无提交，最后发布停在 2012 年。 |
| [lief-project/LIEF](https://github.com/lief-project/LIEF) | LIEF | 2020-12-31 | 0.10.1 / 2019-11-29 | 未还原 | 2020 年持续提交；存档站限流导致星标无法读取。 |
| [tpoechtrager/cctools-port](https://github.com/tpoechtrager/cctools-port) | cctools-port | 2020-12-19 | cctools-877.8-ld64-253.9-1 / 2016-06-24 | 438（2020-12-19） | 2020 年仍在更新，Release 标签停留在 2016 年。 |
| [DerekSelander/LLDB](https://github.com/DerekSelander/LLDB) | LLDB 脚本集 | 2020-07-11 | — | 1,107（2020-11-25） | 脚本集合，无 Release。 |
| [AloneMonkey/frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump) | frida-ios-dump | 2020-06-01 | — | 1,751（2020-12-02） | 基于 Frida 导出已解密 IPA。 |
| [KJCracks/Clutch](https://github.com/KJCracks/Clutch) | Clutch | 2019-11-04 | 2.0.4 / 2016-08-23 | 3,032（2020-12-10） | 自 2019 年起停更。 |
| [stefanesser/dumpdecrypted](https://github.com/stefanesser/dumpdecrypted) | dumpdecrypted | 2014-02-13 | — | 2,494（2020-09-30） | 历史工具，自 2014 年起未变更。 |
| [BishopFox/bfdecrypt](https://github.com/BishopFox/bfdecrypt) | bfdecrypt | 2018-02-07 | — | 321（2020-11-12） | 仅支持 iOS 11.x，自 2018 年起停更。 |
| [libimobiledevice/libimobiledevice](https://github.com/libimobiledevice/libimobiledevice) | libimobiledevice | 2020-12-30 | 1.3.0 / 2020-06-15 | 3,673（2020-12-15） | 活跃维护，2020-06 发布 1.3.0。 |
| [DanTheMan827/ios-app-signer](https://github.com/DanTheMan827/ios-app-signer) | iOS App Signer | 2020-06-22 | 1.13.1 / 2019-12-05 | 3,552（2020-12-04） | 1.13.1。 |
| [axi0mX/ipwndfu](https://github.com/axi0mX/ipwndfu) | ipwndfu | 2019-10-01 | — | 6,065（2020-12-15） | Bootrom 工具集，自 2019 年起停更。 |
| [mildsunrise/darter](https://github.com/mildsunrise/darter) | darter | 2020-11-27 | — | 21（2020-11-17） | 2019-11 已创建，此时仍是早期快照解析器。 |
| [rscloura/Doldrums](https://github.com/rscloura/Doldrums) | Doldrums | 2020-12-16 | — | 未还原 | 2020-11 创建；Beta 解析器，仅支持 Dart 2.10。 |
| [dart-lang/sdk](https://github.com/dart-lang/sdk) | Dart SDK | 2020-12-31 | — | 6,185（2020-11-30） | 当时快照与 AOT 格式的参考实现。 |
| [flutter/flutter](https://github.com/flutter/flutter) | Flutter | 2020-12-31 | — | 109,895（2020-12-31） | 用于把 2020 年的 `libflutter.so` 对应回当时的引擎构建。 |

### 2020 年尚不存在

| 仓库 | GitHub 创建时间 | 说明 |
|---|---|---|
| [CodingGay/BlackDex](https://github.com/CodingGay/BlackDex) | 2021-05-21 | 2020 年不存在。 |
| [mvt-project/mvt](https://github.com/mvt-project/mvt) | 2021-07-16 | 2020 年不存在。 |
| [Impact-I/reFlutter](https://github.com/Impact-I/reFlutter) | 2022-04-13 | 当前仓库创建于 2022 年，不属于 2020 年的工具版图。 |
| [Guardsquare/flutter-re-demo](https://github.com/Guardsquare/flutter-re-demo) | 2022-06-02 | 2020 年不存在。 |
| [palera1n/palera1n](https://github.com/palera1n/palera1n) | 2022-09-17 | 2020 年不存在。 |
| [worawit/blutter](https://github.com/worawit/blutter) | 2023-08-22 | 2020 年不存在；当时可用的 Flutter AOT 解析工具是 `darter` 与 `Doldrums`。 |

## 数据核验

星标与最近推送时间可用 GitHub CLI 复现：

```bash
gh api repos/iBotPeaches/Apktool --jq '{stars: .stargazers_count, pushed: .pushed_at}'
```

本索引中的仓库均于 2026-11-13 通过 GitHub REST API 可访问。上游已归档的项目在“说明”列中标注。

## 合法与合规

仅在自有或获得明确授权评估的应用上使用这些工具。解密、重打包与流量拦截可能受法律、平台条款及目标软件许可证的限制。

## 许可证

本仓库内容采用 GNU General Public License v3.0 发布，见 [LICENSE](LICENSE)。所列项目各自保留其许可证。
