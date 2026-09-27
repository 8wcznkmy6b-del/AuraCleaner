# AuraCleaner

> **极简 · 纯净 · 高性能 原生 macOS 垃圾清理与专业软件卸载工具**  
> 纯 Swift 6 / SwiftUI 构建 | 0 第三方依赖 | 独立特权 Helper 守护进程 | 55 项端到端真机质检门禁

---

> 💡 **项目与分发说明**：  
> - **本仓库 (AuraCleaner)**：为 AuraCleaner 的**官方 DMG 正式发行与发布仓库**（托管安装镜像、Release 日志及 Issue 反馈）；  
> - **源码维护 (Monorepo)**：AuraCleaner 的完整源代码与伴侣截图应用统一维护于主工程仓库 👉 **[8wcznkmy6b-del/AuraSnap](https://github.com/8wcznkmy6b-del/AuraSnap)** 的 `CleanerApp/` 目录下。

---

## 📥 下载与安装 (Download & Installation)

前往本仓库的 **[Releases 页面](https://github.com/8wcznkmy6b-del/AuraCleaner/releases)** 下载最新版 DMG 安装包（如 `AuraCleaner-1.0.2.dmg`），双击挂载后直接将 `AuraCleaner.app` 拖入 `Applications` 文件夹即可。

---

## 📖 产品概述 (Product Overview)

`AuraCleaner` 是从 `AuraSnap` 体系中完全解耦剥离出的独立 macOS 原生系统伴侣应用，专注于**深度系统垃圾清理**与**应用彻底卸载（0 残留、0 僵死、0 误杀）**。

### 🌟 核心设计哲学
1. **纯净解耦**：完全独立的代码库与编译流水线，拥有专属的 App Bundle、独立 Mach-O、独立沙盒授权（`Cleaner.entitlements`）与特权辅助守护进程（`com.dochi.AuraCleaner.PrivilegedHelper`）；
2. **极简低调美学**：彻底消除浮夸花哨的“AI 味”与荧光色，与 AuraSnap 保持高度一致的工业级哑光深空黑微晶质感底座与纯白 45 度几何极简扫把图标；
3. **放权用户与零误杀红线**：
   - 绝不搞一刀切暴力删除；
   - 严格遵循《CleanSafetyGuard》安全准则与用户意图感知机制（Intent-Driven Safety Gate）；
   - 彻底区分只读 FDA 权限与 Root 权限，杜绝盲目弹窗索要管理员密码；
4. **底层流式 I/O 与常数级内存**：
   - 全局底层采用 POSIX `open/read/pread` 统一流式 I/O 引擎（`StreamingFileHasher`），扫描数十 GB 虚拟机镜像内存恒定 $\le 1\text{ MB}$（零堆泄漏）；
   - 重复文件识别采用「逻辑等长 -> APFS Inode 唯一排他 -> 4KB 头部快速指纹 -> 中尾渐进双采样 -> 流式 SHA-256」五级渐进漏斗；
   - 云端脱水占位符防风暴门禁（检测 `UF_DATALESS`），杜绝触发几十 GB 网络下载风暴；
5. **严苛的真机端到端全量质检**：
   - 内置 55 项全场景真实全盘扫描质检门禁（`CleanerEngineVerificationSuite`），直接驱动真实文件系统，100% 拒绝真空 Mock 放水。

---

## 🌟 最新版本特性 (v1.0.2)

### 1. 安全守卫按模式分治 (`CleanSafetyGuard`)
- **卸载上下文贯穿特权层**：此前扫描阶段带 Bundle ID 放行的路径到特权删除层丢失上下文被判为红线的问题彻底修复，全链路携带 `uninstalledBundleID`；
- **单条拒绝不再连坐**：特权批次中个别路径被守卫拒绝只跳过该路径并记审计 (`skipped`)，仅在授权本身失败时整单中止；
- **权限失败自动转特权兜底**：用户态移入废纸篓遇 root 属主、`/Users/Shared` 粘滞位等无权限时，合并为一次特权批次重试；
- **卸载模式**：已确权残留不再按文件名关键词二次否决；应用专属 dotdir 随应用删除；
- **清理模式**：缓存/日志/临时区内文件不按文件名关键词拦截；`/Library/Logs` 纳入特权可清范围。

### 2. 卸载归属：本机语料词特异性 + 独占性确权 (`TokenSpecificityIndex`)
- 从本机已安装应用的 Bundle ID / 显示名 / 包名自学习词频，被 $\ge 2$ 家不同开发者共用的词自动成为「通用词」，替代手写黑名单；
- 目录名的每个有效词都必须能被目标应用解释且至少一个词为其独有，才判为专属；只靠通用词判为「有歧义」仅展示。

### 3. 垃圾清理覆盖扩展 (零应用名，按结构识别)
- **应用内置浏览器内核缓存**：扫描 `Application Support` 与沙盒容器，按词边界识别 Chromium / Electron 缓存与代码缓存，默认勾选清理；
- **旧版本残留**：识别同级多版本目录，保留最新版本，历史旧版本列出供清理；
- **系统诊断快照**：`.logarchive` 统一日志归档包整体识别与列出。

---

## 🔬 内部测试架构与 55 项端到端全真机质检门禁

AuraCleaner 内置 55 项全场景真实全盘扫描质检门禁（`CleanerEngineVerificationSuite`，100% PASS），对真实文件系统与权限层进行实战校验。

---

## 📄 开源与许可证

源码统一托管于主工程仓库 [8wcznkmy6b-del/AuraSnap](https://github.com/8wcznkmy6b-del/AuraSnap)。
