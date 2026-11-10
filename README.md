# geoiplite-syncipinfo
geoip-data sync from ipinfo_lite


# GeoIP Lite Weekly Release

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)

每周自动从 IPinfo 同步 GeoIP Lite CSV 数据库，并通过 GitHub Releases 提供固定下载地址。适用于 OPNsense 等需要定期更新 GeoIP 数据的场景。

## 📥 永久下载地址

```text
https://github.com/halp3ng/geoiplite-syncipinfo/releases/latest/download/geoip-lite.csv.gz
```

> 该链接始终指向最新版本，无需随版本号变更而修改。

## ⚙️ 工作机制

- **触发频率**：每周一 UTC 02:00 自动执行（支持手动触发）
- **版本标签**：采用简化 CalVer 格式 `vYY.MM.WW`（如 `v26.11.45`）
- **数据来源**：[IPinfo Lite CSV](https://ipinfo.io/lite)，每日更新
- **自动清理**：Workflow 内置清理步骤，仅保留最近 **12 周**的版本，自动删除更早的 Release 及其附件

## 🔧 部署前提

在仓库 **Settings → Secrets and variables → Actions** 中添加以下 Secret：

| Secret 名称 | 说明 |
| :--- | :--- |
| `IPINFO_TOKEN` | IPinfo API Token，从 [Dashboard](https://ipinfo.io/dashboard/downloads) 获取 |

> `GITHUB_TOKEN` 由 GitHub 自动生成，无需手动配置。

## 📄 License

本项目基于 [Apache License 2.0](LICENSE) 开源。

GeoIP 数据版权归 [IPinfo](https://ipinfo.io) 所有，使用需遵守其服务条款。

---
## 参考与致谢
- [IPinfo 官方文档](https://ipinfo.io/developers)
- [IPinfo Lite 数据库说明](https://ipinfo.io/data)
- [OPNsense GeoIP 配置指南]((https://docs.opnsense.org/manual/how-tos/ipinfo_geo_ip.html))
