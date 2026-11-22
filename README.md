# Minecraft 全量下载完整性校验报告

本仓库收录了一次「国内镜像源全量下载 Minecraft 生态」之后的**完整性校验产物**。
数据集本体约 116 GB、38,323 个文件，**不在本仓库内**（GitHub 仓库建议 ≤ 1 GB，无法容纳）。
本仓库只保存体量小、可长期留存的校验结论与指纹清单。

## 文件说明

| 文件 | 说明 |
|---|---|
| `mc_download_report.html` | 全量下载覆盖率报告（版本、加载器、插件、资产的计数与缺失项） |
| `mc_verify_report.html` | 完整性校验的可视化报告（方法、硬盘占用、修复记录） |
| `mc_verify_report.json` | 校验机器可读结论：38,323 文件，SHA1 14,525 + ZIP 21,895，最终残留 0 |
| `mc_final_check.json` | 修复轮结论：初始异常 364，修复 364，仍异常 0 |
| `mc_third_pass.json` | 独立第三轮复算：920/920 客户端匹配官方 SHA1，3/3 加载器 ZIP 有效 |
| `mc_md5_manifest.txt` | 全量 MD5 指纹清单，格式 `md5  字节数  相对路径`，共 38,323 行 |

## 校验方法

- **权威 SHA1 比对**（14,525 个）：客户端/服务端 jar 与 resources assets，用 Mojang/BMCLAPI 官方清单的 SHA1 + 文件大小逐文件比对。
  > Mojang/BMCLAPI 官方只发布 SHA1，不发布 MD5；故权威比对用 SHA1，MD5 清单仅作指纹留存。
- **ZIP 结构校验**（21,895 个）：Forge/NeoForge/Fabric/Quilt/LiteLoader/OptiFine 加载器、库、插件 jar/zip，解析 ZIP 中央目录，半截下载必被识别。
- **零字节检测**：全部文件 `size != 0`。

## 修复记录

初始校验发现 **364 个异常**：

- 361 个原版客户端 jar：合法 zip 但内容与官方 SHA1 不符（下错内容），按官方哈希从 BMCLAPI2/官方源重下；
- 3 个加载器 jar：Forge 1.12.2 installer、Quilt loader 0.17.5-beta.4 / beta.5 损坏，从对应 Maven 源重下。

修复后经两轮复核：**364/364 全部通过，残留 0**。第三轮独立复算再次确认 920/920 客户端 + 3/3 加载器全部有效。

_生成时间：2026-11_
