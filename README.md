# geoiplite-syncipinfo
geoip-data sync from ipinfo_lite


## 简介
这是一个用于自动同步 IPinfo Lite GeoIP 数据库的 GitHub Actions 模板仓库。旨在为 OPNsense 防火墙提供稳定、免费、每日更新的国家级 IP 地址数据源，用于配置 GeoIP 别名和防火墙规则，实现基于国家/地区的流量控制。

## 特性
- **自动化同步**：通过 GitHub Actions 定时从 IPinfo 拉取最新的 `ipinfo_lite.csv.gz` 数据。
- **免费无限制**：基于 IPinfo Lite 免费层级，无 API 请求限制，支持 IPv4 和 IPv6。
- **无缝对接 OPNsense**：生成的数据链接可直接填入 OPNsense 的 GeoIP 设置中，替代已停止免费更新的 MaxMind GeoLite2。
- **合规许可**：严格遵守 IPinfo 的署名要求及 CC BY-SA 4.0 数据许可。

---

## 部署与使用指南

### 1. 准备工作
- 注册 [IPinfo 账号](https://ipinfo.io/signup) 并获取你的免费 API Token（无需绑定信用卡）。
- Fork 本仓库到你的 GitHub 账号。

### 2. 配置 GitHub Actions
1. 进入 Fork 后的仓库，点击 **Settings** -> **Secrets and variables** -> **Actions**。
2. 添加一个新的 Repository secret：
   - **Name**: `IPINFO_TOKEN`
   - **Value**: 你的 IPinfo API Token
3. 启用 GitHub Actions（在 **Actions** 标签页中允许 workflows 运行）。
4. 手动触发一次 workflow（或等待定时任务执行），确保数据文件成功生成并发布。

### 3. 获取数据 URL
根据你 workflow 的输出方式，获取 `ipinfo_lite.csv.gz` 的直接下载链接：
- **如果使用 GitHub Releases**：前往仓库的 Releases 页面，右键点击 `ipinfo_lite.csv.gz` 并复制链接地址。
- **如果使用 Raw 文件**：获取类似 `https://raw.githubusercontent.com/<你的用户名>/<仓库名>/<分支>/ipinfo_lite.csv.gz` 的链接。

### 4. 配置 OPNsense
1. 登录 OPNsense Web 管理界面。
2. 导航至 **Firewall** -> **Aliases**，选择 **GeoIP settings** 选项卡。
3. 在 **Url** 文本框中，粘贴你在上一步获取的数据下载链接。
4. 点击 **Apply** 保存并应用设置。OPNsense 将在后台下载并解析数据库（首次加载可能需要几分钟）。
5. 导航至 **Firewall** -> **Aliases** -> **IP**，点击 **+** 号创建新别名：
   - **Name**: 自定义（如 `Blocked_Countries`）
   - **Type**: 选择 `GeoIP`
   - **Countries**: 选择你需要封锁或放行的国家代码（如 CN, RU 等）
   - 点击 **Save** 并 **Apply changes**。
6. 最后，前往 **Firewall** -> **Rules**，在 WAN 接口创建规则，将 Source 设置为刚创建的 GeoIP 别名，Action 设置为 Block。

---

## 许可协议

本项目采用混合许可模式，以区分工具代码与第三方数据：

### 1. 仓库代码 (MIT License)
本仓库中的 GitHub Actions 配置文件、脚本及文档等代码部分，采用 Apache-2.0 license 开源。你可以自由使用、修改和分发这些代码。

### 2. GeoIP 数据 (CC BY-SA 4.0)
本仓库同步和托管的 GeoIP 数据文件（`ipinfo_lite.csv.gz`）来源于 [IPinfo Lite](https://ipinfo.io/)，该数据采用 [Creative Commons Attribution-ShareAlike 4.0 International License (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/) 许可。

**署名要求 (Attribution)**：
根据 CC BY-SA 4.0 协议，使用此数据必须保留以下署名：
> GeoIP data provided by [IPinfo](https://ipinfo.io/) under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

*合规提示：如果你将此数据文件打包进其他产品或对外提供公开下载服务，你的整体衍生作品必须同样以 CC BY-SA 4.0 或兼容协议发布。仅作为个人/企业内部防火墙内部同步使用，不触发 SA（相同方式共享）条款对防火墙本身配置的约束。*

---

## 参考与致谢
- [IPinfo 官方文档](https://ipinfo.io/developers)
- [IPinfo Lite 数据库说明](https://ipinfo.io/data)
- [OPNsense GeoIP 配置指南](https://docs.opnsense.org/manual/how-tos/geoip.html)
