# Windows C 盘可清理目录/文件参考大全

本文档列出了 Windows C 盘上常见的可清理目录和文件，按类别分组。
扫描时应参考此文档识别各类缓存和残留文件。

## 目录

1. [Windows 系统临时文件](#1-windows-系统临时文件)
2. [浏览器缓存](#2-浏览器缓存)
3. [Windows Update 缓存](#3-windows-update-缓存)
4. [回收站](#4-回收站)
5. [缩略图与图标缓存](#5-缩略图与图标缓存)
6. [日志文件](#6-日志文件)
7. [已卸载软件残留](#7-已卸载软件残留)
8. [开发工具缓存](#8-开发工具缓存)
9. [常见应用缓存](#9-常见应用缓存)
10. [系统还原与备份](#10-系统还原与备份)
11. [字体缓存](#11-字体缓存)
12. [Windows 传递优化](#12-windows-传递优化)
13. [休眠文件](#13-休眠文件)
14. [虚拟内存页面文件](#14-虚拟内存页面文件)
15. [Windows 旧安装文件](#15-windows-旧安装文件)
16. [DRM 与媒体缓存](#16-drm-与媒体缓存)
17. [游戏平台缓存](#17-游戏平台缓存)
18. [大文件/长期未用文件扫描策略](#18-大文件长期未用文件扫描策略)

---

## 1. Windows 系统临时文件

| 路径 | 说明 | 风险等级 |
|------|------|---------|
| `C:\Windows\Temp` | 系统临时文件夹 | 低 |
| `%TEMP%` / `%TMP%` (通常为 `C:\Users\<用户>\AppData\Local\Temp`) | 用户临时文件夹 | 低 |
| `C:\Windows\Prefetch` | 预取缓存，加速程序启动 | 低-中 |
| `C:\Windows\SoftwareDistribution\Download` | Windows Update 下载缓存 | 低 |

**删除依据**：临时文件是程序运行时产生的中间数据，程序结束后通常不再需要。预取缓存会在系统使用过程中自动重建。

**删除影响**：
- 临时文件：无影响，当前正在使用的文件会被系统锁定无法删除
- 预取缓存：删除后系统重新启动时需重新构建预取数据，首次启动各程序可能略慢

---

## 2. 浏览器缓存

| 路径 | 说明 | 风险等级 |
|------|------|---------|
| `C:\Users\<用户>\AppData\Local\Google\Chrome\User Data\Default\Cache` | Chrome 缓存 | 低 |
| `C:\Users\<用户>\AppData\Local\Google\Chrome\User Data\Default\Code Cache` | Chrome 代码缓存 | 低 |
| `C:\Users\<用户>\AppData\Local\Microsoft\Edge\User Data\Default\Cache` | Edge 缓存 | 低 |
| `C:\Users\<用户>\AppData\Local\Microsoft\Edge\User Data\Default\Code Cache` | Edge 代码缓存 | 低 |
| `C:\Users\<用户>\AppData\Local\Mozilla\Firefox\Profiles\<profile>\cache2` | Firefox 缓存 | 低 |

**删除依据**：浏览器缓存存储网页资源以加速重复访问，删除后浏览器会自动重新下载。

**删除影响**：首次访问近期网页时加载稍慢，已登录的网站登录状态不受影响。

---

## 3. Windows Update 缓存

| 路径 | 说明 | 风险等级 |
|------|------|---------|
| `C:\Windows\SoftwareDistribution\Download` | 已下载的更新包 | 低 |
| `C:\Windows\SoftwareDistribution\DataStore` | 更新数据存储 | 中 |

**删除依据**：已安装的更新包文件不再需要，保留只占用空间。

**删除影响**：Download 文件夹删除后如需回滚更新需重新下载；DataStore 删除后更新历史记录丢失。

---

## 4. 回收站

| 路径 | 说明 | 风险等级 |
|------|------|---------|
| `C:\$Recycle.Bin` | 回收站实际存储位置 | 低-中 |

**删除依据**：回收站中的文件是用户已"删除"的文件，占用空间实质上是浪费。

**删除影响**：清空后无法通过回收站恢复。

---

## 5. 缩略图与图标缓存

| 路径 | 说明 | 风险等级 |
|------|------|---------|
| `C:\Users\<用户>\AppData\Local\Microsoft\Windows\Explorer\thumbcache_*.db` | 缩略图缓存 | 低 |
| `C:\Users\<用户>\AppData\Local\IconCache.db` | 图标缓存 | 低 |

**删除依据**：缩略图和图标缓存是系统为加速文件浏览生成的预览图像，删除后自动重建。

**删除影响**：首次打开含大量图片/视频的文件夹时缩略图生成稍慢。

---

## 6. 日志文件

| 路径 | 说明 | 风险等级 |
|------|------|---------|
| `C:\Windows\Logs` | Windows 系统日志 | 低-中 |
| `C:\Windows\System32\winevt\Logs` | 事件查看器日志 | 中 |
| `C:\Users\<用户>\AppData\Local\CrashDumps` | 崩溃转储文件 | 低 |
| `C:\Windows\Minidump` | 内核崩溃转储 | 低 |
| `C:\Users\<用户>\AppData\Local\Microsoft\Windows\WER` | Windows 错误报告 | 低 |

**删除依据**：日志文件用于故障排查，旧日志通常不再需要。

**删除影响**：删除后如需排查近期问题将缺少历史日志。

---

## 7. 已卸载软件残留

| 路径 | 说明 | 风险等级 |
|------|------|---------|
| `C:\Program Files\<已卸载软件名>` | Program Files 中的残留目录 | 中 |
| `C:\Program Files (x86)\<已卸载软件名>` | Program Files (x86) 中的残留目录 | 中 |
| `C:\ProgramData\<已卸载软件名>` | ProgramData 中的残留数据 | 中 |
| `C:\Users\<用户>\AppData\Local\<已卸载软件名>` | Local AppData 残留 | 中 |
| `C:\Users\<用户>\AppData\Roaming\<已卸载软件名>` | Roaming AppData 残留 | 中 |

**检测策略**：对比注册表中已安装程序列表与磁盘目录，找出不匹配的残留。

**删除依据**：软件卸载后配置文件和缓存数据往往残留在 AppData 等位置。

**删除影响**：重新安装同一软件时可能需要重新配置。

---

## 8. 开发工具缓存

| 路径 | 说明 | 风险等级 |
|------|------|---------|
| `C:\Users\<用户>\.npm` | npm 缓存 | 低 |
| `C:\Users\<用户>\AppData\Local\npm-cache` | npm 缓存（新版本） | 低 |
| `C:\Users\<用户>\pip\cache` | pip 缓存 | 低 |
| `C:\Users\<用户>\AppData\Local\pip\cache` | pip 缓存（新版本） | 低 |
| `C:\Users\<用户>\.m2\repository` | Maven 本地仓库 | 中 |
| `C:\Users\<用户>\.gradle\caches` | Gradle 缓存 | 中 |
| `C:\Users\<用户>\.nuget\packages` | NuGet 包缓存 | 中 |
| `C:\Users\<用户>\.cargo\registry` | Rust Cargo 缓存 | 中 |
| `C:\Users\<用户>\AppData\Local\Yarn\Cache` | Yarn 缓存 | 低 |
| `C:\Users\<用户>\AppData\Local\pnpm-cache` | pnpm 缓存 | 低 |
| `C:\Users\<用户>\.conda\pkgs` | Conda 包缓存 | 中 |
| `C:\Users\<用户>\.vscode\extensions` | VS Code 扩展 | 中 |
| `C:\Users\<用户>\AppData\Local\JetBrains` | JetBrains IDE 缓存 | 中 |
| `C:\Users\<用户>\.android\build-cache` | Android 构建缓存 | 低 |

**删除依据**：开发工具缓存存储已下载的依赖包和构建产物，删除后重新构建时自动下载。

**删除影响**：低风险项仅影响下次安装速度；中风险项可能影响离线构建能力。

---

## 9. 常见应用缓存

| 路径 | 说明 | 风险等级 |
|------|------|---------|
| `C:\Users\<用户>\AppData\Local\Spotify\Storage` | Spotify 缓存 | 低 |
| `C:\Users\<用户>\AppData\Local\Discord\Cache` | Discord 缓存 | 低 |
| `C:\Users\<用户>\AppData\Local\Slack\Cache` | Slack 缓存 | 低 |
| `C:\Users\<用户>\AppData\Local\Microsoft\Teams\Cache` | Teams 缓存 | 低 |
| `C:\Users\<用户>\AppData\Local\WeChat\Files` | 微信文件缓存 | 中 |
| `C:\Users\<用户>\Documents\WeChat Files` | 微信文件（含聊天记录附件） | 高 |
| `C:\Users\<用户>\AppData\Roaming\Tencent` | 腾讯应用数据 | 中 |
| `C:\Users\<用户>\AppData\Local\DingTalk` | 钉钉缓存 | 中 |

**删除影响**：低风险仅影响加载速度；中风险可能丢失部分聊天图片缓存；高风险可能丢失聊天附件。

---

## 10. 系统还原与备份

| 路径 | 说明 | 风险等级 |
|------|------|---------|
| `C:\System Volume Information` | 系统还原点 | 高 |
| `C:\Windows\backup` | Windows 备份文件 | 高 |
| `C:\Windows\WinSxS\Backup` | 组件存储备份 | 高 |
| `C:\Recovery` | 恢复环境 | 高 |

**删除影响**：删除后无法回滚到之前的系统状态。不建议手动删除，应通过系统工具操作。

---

## 11. 字体缓存

| 路径 | 说明 | 风险等级 |
|------|------|---------|
| `C:\Windows\ServiceProfiles\LocalService\AppData\Local\FontCache` | 系统字体缓存 | 低 |

**删除依据**：字体缓存是系统为加速字体加载生成的索引数据，删除后自动重建。

**删除影响**：重启后首次打开使用大量字体的应用时加载稍慢。

---

## 12. Windows 传递优化

| 路径 | 说明 | 风险等级 |
|------|------|---------|
| `C:\Windows\SoftwareDistribution\DeliveryOptimization` | 传递优化缓存 | 低 |
| `C:\Windows\ImmersivePrefetch` | UWP 应用预取 | 低 |

**删除依据**：传递优化用于局域网内共享 Windows 更新，缓存文件是已分发的更新块。

**删除影响**：删除后不影响已安装的更新，仅影响局域网内其他设备从此机获取更新的能力（暂时的）。

---

## 13. 休眠文件

| 路径 | 说明 | 风险等级 |
|------|------|---------|
| `C:\hiberfil.sys` | 休眠文件（通常等于内存大小） | 高 |

**删除依据**：hiberfil.sys 占用与物理内存等量的磁盘空间。如果不使用休眠功能，可以禁用休眠释放此空间。

**删除影响**：禁用休眠后将无法使用休眠功能，但睡眠功能不受影响。需通过 `powercfg /hibernate off` 禁用，不可直接删除。

---

## 14. 虚拟内存页面文件

| 路径 | 说明 | 风险等级 |
|------|------|---------|
| `C:\pagefile.sys` | 页面文件 | 高 |
| `C:\swapfile.sys` | UWP 交换文件 | 高 |

**说明**：系统正常运行必需，不应删除。可调整大小或移到其他分区。

---

## 15. Windows 旧安装文件

| 路径 | 说明 | 风险等级 |
|------|------|---------|
| `C:\Windows.old` | 系统升级后的旧系统文件 | 中 |
| `C:\$WINDOWS.~BT` | Windows 升级临时文件 | 低-中 |
| `C:\$WINDOWS.~Q` | Windows 升级回滚文件 | 中 |

**删除影响**：删除后无法回滚到之前的 Windows 版本。应通过磁盘清理工具删除。

---

## 16. DRM 与媒体缓存

| 路径 | 说明 | 风险等级 |
|------|------|---------|
| `C:\Users\<用户>\AppData\Local\Microsoft\Windows Media Player` | WMP 缓存 | 低 |
| `C:\ProgramData\Microsoft\PlayReady` | DRM 许可缓存 | 中 |

**删除影响**：PlayReady 缓存删除后可能需要重新获取 DRM 许可才能播放受保护内容。

---

## 17. 游戏平台缓存

| 路径 | 说明 | 风险等级 |
|------|------|---------|
| `C:\Program Files (x86)\Steam\steamapps\downloading` | Steam 下载中的游戏文件 | 中 |
| `C:\Program Files (x86)\Steam\steamapps\shadercache` | Steam 着色器缓存 | 低 |
| `C:\Program Files (x86)\Steam\steamapps\temp` | Steam 临时文件 | 低 |
| `C:\Program Files (x86)\Epic Games\Launcher\VaultCache` | Epic 游戏缓存 | 中 |

**删除影响**：downloading 文件夹删除需重新下载游戏；着色器缓存删除后首次启动游戏需重新编译。

---

## 18. 大文件/长期未用文件扫描策略

### 大文件扫描
- 扫描 C 盘中超过 100MB 的文件
- 排除系统关键文件（pagefile.sys, hiberfil.sys, swapfile.sys 等）
- 按文件大小排序展示
- 标注文件类型和最后访问时间

### 长期未用文件
- 扫描超过 90 天未被访问的文件
- 重点关注用户目录下的文件
- 标注文件大小和最后访问时间

### 已卸载软件残留检测
- 读取注册表获取已安装软件列表
- 对比 Program Files / AppData 中的目录
- 识别不属于任何已安装软件的目录
- 注意：某些软件使用不同的注册名和目录名，需谨慎判断

### 空目录
- 扫描 C 盘中的空目录（特别是在 AppData 下）
- 排除系统必需的空目录
