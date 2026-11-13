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
