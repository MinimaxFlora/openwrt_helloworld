<div align="center">

  <!-- GitHub 个人统计卡片 (自动适配暗黑/明亮主题) -->
  <p>
    <img src="https://github-readme-stats.vercel.app/api?username=MinimaxFlora&show_icons=true&theme=tokyonight&hide_border=true&cache_seconds=86400#gh-dark-mode-only" alt="MinimaxFlora GitHub Stats" height="165" />
    <img src="https://github-readme-stats.vercel.app/api?username=MinimaxFlora&show_icons=true&theme=default&hide_border=true&cache_seconds=86400#gh-light-mode-only" alt="MinimaxFlora GitHub Stats" height="165" />
    <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=MinimaxFlora&layout=compact&theme=tokyonight&hide_border=true&cache_seconds=86400#gh-dark-mode-only" alt="Top Languages" height="165" />
    <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=MinimaxFlora&layout=compact&theme=default&hide_border=true&cache_seconds=86400#gh-light-mode-only" alt="Top Languages" height="165" />
  </p>

  <h1>🚀 OpenWrt HelloWorld</h1>
  <p><b>专为 OpenWrt / ImmortalWrt 打造的纯净、高频同步插件集合仓库</b></p>

  <!-- 仓库状态徽章 (Flat-Square 现代扁平风格) -->
  <p>
    <a href="https://github.com/MinimaxFlora/openwrt_helloworld/stargazers">
      <img src="https://img.shields.io/github/stars/MinimaxFlora/openwrt_helloworld?style=flat-square&color=f59e0b&logo=github" alt="Stars" />
    </a>
    <a href="https://github.com/MinimaxFlora/openwrt_helloworld/network/members">
      <img src="https://img.shields.io/github/forks/MinimaxFlora/openwrt_helloworld?style=flat-square&color=f97316&logo=git" alt="Forks" />
    </a>
    <a href="https://github.com/MinimaxFlora/openwrt_helloworld/issues">
      <img src="https://img.shields.io/github/issues/MinimaxFlora/openwrt_helloworld?style=flat-square&color=10b981" alt="Issues" />
    </a>
    <a href="https://github.com/MinimaxFlora/openwrt_helloworld/commits">
      <img src="https://img.shields.io/github/last-commit/MinimaxFlora/openwrt_helloworld?style=flat-square&color=3b82f6" alt="Last Commit" />
    </a>
    <img src="https://img.shields.io/github/languages/code-size/MinimaxFlora/openwrt_helloworld?style=flat-square&color=8b5cf6" alt="Code Size" />
    <a href="https://github.com/MinimaxFlora/openwrt_helloworld/blob/main/LICENSE">
      <img src="https://img.shields.io/github/license/MinimaxFlora/openwrt_helloworld?style=flat-square&color=ec4899" alt="License" />
    </a>
  </p>

</div>

---

### 📦 仓库简介

**`openwrt_helloworld`** 聚合了网络增强与科学上网相关的核心插件及依赖，定期紧跟上游源码更新，确保编译兼容性与稳定性。

| 插件名称 | 组件目录 | 核心功能说明 |
| :--- | :--- | :--- |
| **SSR Plus+** | `luci-app-ssr-plus` | 经典多协议代理客户端，支持主流节点订阅与分流 |
| **PassWall** | `luci-app-passwall` | 界面直观、规则灵活的代理与分流工具 |
| **PassWall 2** | `luci-app-passwall2` | 基于 Xray/Sing-box 架构重构的新一代代理插件 |
| **OpenClash** | `luci-app-openclash` | 适配 Clash / Mihomo 内核的全功能可视化代理客户端 |
| **HomeProxy** | `luci-app-homeproxy` | 专为 ImmortalWrt / OpenWrt 设计的现代 Sing-box 客户端 |
| **核心依赖库** | `xray-core` / `sing-box` / `mihomo` 等 | 完整配套的二进制内核与加密/分流依赖组件 |

> 💡 **注**：以上列表可根据你仓库实际包含的包名微调，未包含的直接删去对应行即可。

---

### 🛠️ 使用方法

在你的 OpenWrt 源码根目录下，将本仓库添加到 `feeds.conf.default` 中：

```bash
# 添加 helloworld 源
echo "src-git helloworld [https://github.com/MinimaxFlora/openwrt_helloworld.git](https://github.com/MinimaxFlora/openwrt_helloworld.git)" >> feeds.conf.default

# 更新并安装 feeds
./scripts/feeds update -a
./scripts/feeds install -a
