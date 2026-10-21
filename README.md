# openwrt_helloworld

Self-maintained copy of the packages ZeroWrt ships: the PassWall toolchain,
its two LuCI front ends and OpenClash. Everything here is vendored so a build
never depends on four different upstream checkouts, and so a broken upstream
release can be fixed in one place.

## Layout

```
chinadns-ng/  dns2socks/  geoview/  hysteria/  ipt2socks/  microsocks/
naiveproxy/  shadow-tls/  shadowsocks-rust/  shadowsocksr-libev/
simple-obfs/  sing-box/  tcping/  v2ray-geodata/  v2ray-plugin/
xray-core/  xray-plugin/                     <- package sources
luci-app-passwall/  luci-app-passwall2/      <- LuCI front ends
luci-app-openclash/
```

Package directories sit at the repository root on purpose: the update workflow
below edits `Makefile` in place and expects the same layout as
[openwrt-passwall-packages](https://github.com/Openwrt-Passwall/openwrt-passwall-packages).

## Where the content comes from

| Directory | Upstream |
| --- | --- |
| package sources | [Openwrt-Passwall/openwrt-passwall-packages](https://github.com/Openwrt-Passwall/openwrt-passwall-packages) |
| `luci-app-passwall` | [Openwrt-Passwall/openwrt-passwall](https://github.com/Openwrt-Passwall/openwrt-passwall) |
| `luci-app-passwall2` | [Openwrt-Passwall/openwrt-passwall2](https://github.com/Openwrt-Passwall/openwrt-passwall2) |
| `luci-app-openclash` | [vernesong/OpenClash](https://github.com/vernesong/OpenClash) |

## Keeping it current

Two workflows, on purpose:

* **Auto update packages v3** (`auto-update-packages.yml`) — runs every four
  hours, reads the latest release of every upstream project, bumps
  `PKG_VERSION` / `PKG_HASH` in the matching `Makefile` and opens a pull
  request per package. This is the workflow from openwrt-passwall-packages,
  with the token switched to the repository token.
* **Sync LuCI apps** (`sync-luci-apps.yml`) — runs daily, replaces the three
  LuCI directories with the upstream content and commits when something
  changed. It can also re-sync the package sources on demand
  (`workflow_dispatch` → `sync_packages=true`).

Reviews stay manual: version bumps arrive as pull requests, whole-directory
syncs are committed directly because they are upstream content and never
conflict with the version bumps (different files, and a later sync simply
restores the upstream Makefile).

## Using it from a build

Either add it as a feed:

```
src-git helloworld https://github.com/MinimaxFlora/openwrt_helloworld.git
```

or copy the directories into `package/new/` the way ZeroWrt does:

```sh
git clone --depth 1 https://github.com/MinimaxFlora/openwrt_helloworld.git /tmp/hw
cp -r /tmp/hw/luci-app-passwall /tmp/hw/luci-app-passwall2 /tmp/hw/luci-app-openclash \
      /tmp/hw/chinadns-ng /tmp/hw/xray-core ... "$LEDE_DIR/package/new/"
```

## License

The vendored content keeps its own licenses: PassWall and its packages are
GPL-2.0, OpenClash is GPL-3.0. See the `LICENSE` file inside each directory.
