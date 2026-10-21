# openwrt_helloworld

ZeroWrt 使用的 PassWall 与 OpenClash 相关软件包，全部内置在本仓库里自己维护。

固件构建不再同时依赖四个上游仓库：上游发版出错时可以在这里单独修，上游更新了又可以通过
workflow 自动跟进。

## 目录

- [仓库里有什么](#仓库里有什么)
- [软件包](#软件包)
- [自动更新](#自动更新)
- [在固件构建中使用](#在固件构建中使用)
- [许可证](#许可证)

## 仓库里有什么

| 类别 | 目录 | 上游 |
| --- | --- | --- |
| 软件包 | 见下表，直接放在仓库根目录 | [Openwrt-Passwall/openwrt-passwall-packages](https://github.com/Openwrt-Passwall/openwrt-passwall-packages) |
| LuCI 插件 | `luci-app-passwall` | [Openwrt-Passwall/openwrt-passwall](https://github.com/Openwrt-Passwall/openwrt-passwall) |
| LuCI 插件 | `luci-app-passwall2` | [Openwrt-Passwall/openwrt-passwall2](https://github.com/Openwrt-Passwall/openwrt-passwall2) |
| LuCI 插件 | `luci-app-openclash` | [vernesong/OpenClash](https://github.com/vernesong/OpenClash) |

软件包放在根目录是有意的：更新脚本按上游的目录布局直接改 `Makefile`，路径不用另做映射。

## 软件包

| 目录 | 用途 |
| --- | --- |
| `xray-core` / `xray-plugin` | Xray 内核，以及把它接入 v2ray-plugin 的插件 |
| `sing-box` | sing-box 内核，PassWall 的 Sing-Box 模式 |
| `hysteria` | Hysteria 协议 |
| `naiveproxy` | NaiveProxy |
| `shadow-tls` | ShadowTLS |
| `shadowsocks-rust` | Shadowsocks Rust 实现 |
| `shadowsocksr-libev` | SSR 老协议支持 |
| `v2ray-plugin` | v2ray-plugin |
| `v2ray-geodata` | `geoip.dat` / `geosite.dat` |
| `chinadns-ng` | DNS 分流 |
| `dns2socks` | DNS over SOCKS |
| `microsocks` | SOCKS5 服务端 |
| `ipt2socks` | iptables 转 SOCKS |
| `tcping` | TCP 探测 |
| `geoview` | IP 地理位置查询 |

## 自动更新

只有一条 workflow：**Auto update packages v3**（`.github/workflows/auto-update-packages.yml`），
它有三个 job。

**`job_auto_update_packages`** — 每 4 小时（`11 */4 * * *`）跑一次。对每个软件包：

1. 读上游 release，和 `Makefile` 里的 `PKG_VERSION` 比版本；
2. 有新版本就下载对应文件算 `sha256`，写回 `PKG_VERSION` / `PKG_HASH`；
3. 推一个 `patches-<包名>-<版本>` 分支，开一个 PR。

所以版本更新以 PR 的形式进来，合并与否由人决定，不会直接改动 `main`。

**`job_sync_luci_apps`** — 每天随同一次运行。三个 `luci-app-*` 是整目录替换，内容就是上游
原样，没有本地改动，所以直接提交到 `main`；没有变化就什么都不做。

OpenClash 仓库带了各种内核二进制，历史有 40GB 以上，所以同步时用
`--filter=blob:none --sparse` 只取 `luci-app-openclash` 一个目录。

**`job_init`** — 清理旧运行记录，并保留上游提供的 SSH 调试入口（`ssh=true` 时启动 tmate）。

### 手动触发

`workflow_dispatch` 有几个开关：

| 输入 | 作用 |
| --- | --- |
| `sync_packages` | 除插件外，把软件包目录也按上游整目录重刷一遍 |
| `is_update_geodata` | 这次运行是否处理 `geoip` / `geosite` 数据包 |
| `is_update_from_prerelease` | 用包含预发布版的 release 做版本对比 |
| `ssh` | 启动 tmate，登录 runner 手动排查 |

> `xray-core` 单独改过一处：Xray 把**每个 release 都标成 prerelease**，上游原本的
> “只取非预发布”过滤永远只能看到 26.3.27，会被判定成“本地版本更高”。所以这一项改成取最新 tag。

## 在固件构建中使用

**方式一：当成 feed**

```sh
echo 'src-git helloworld https://github.com/MinimaxFlora/openwrt_helloworld.git' \
    >> feeds.conf.default
./scripts/feeds update helloworld
./scripts/feeds install -a -p helloworld
```

**方式二：复制进 `package/new/`**

```sh
git clone --depth 1 https://github.com/MinimaxFlora/openwrt_helloworld.git /tmp/hw
mkdir -p package/new
cp -r /tmp/hw/luci-app-passwall /tmp/hw/luci-app-passwall2 /tmp/hw/luci-app-openclash \
      /tmp/hw/xray-core /tmp/hw/sing-box … package/new/
```

ZeroWrt 目前用的是方式二（`scripts/zerowrt_core/packages.py`）。

## 许可证

各目录保留自己的许可证：

- PassWall、passwall-packages、luci-app-passwall(2)：GPL-2.0
- OpenClash：GPL-3.0

具体条款见各目录下的 `LICENSE` 文件。`.github/workflows/auto-update-packages.yml`
来自 [smallprogram](https://github.com/smallprogram)，保留原始版权声明。
