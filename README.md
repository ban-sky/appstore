# 1Panel 第三方应用商店（zerotier-one）

本仓库沿用 [okxlin/appstore](https://github.com/okxlin/appstore) 的目录结构与 `data.yml` 规范，但**只保留 `zerotier-one` 一个应用**，不包含上游仓库中的其他应用。

## 应用列表

| 应用 | 当前版本 | 说明 |
| --- | --- | --- |
| [zerotier-one](apps/zerotier-one) | `1.16.2` | 多点虚拟化网络解决方案（A Smart Ethernet Switch for Earth） |

## 安装到 1Panel

在 1Panel 中新建类型为 `Shell 脚本` 的计划任务，或直接在终端执行（按实际 1Panel 安装目录修改 `PANEL_BASE_DIR`）：

```bash
set -euo pipefail

PANEL_BASE_DIR="/opt" # 按实际的 1Panel 安装根目录修改
LOCAL_APPS_DIR="$PANEL_BASE_DIR/1panel/resource/apps/local"
IMPORT_DIR="$LOCAL_APPS_DIR/appstore-localApps"

[ -d "$LOCAL_APPS_DIR" ] || { echo "本地应用目录不存在：$LOCAL_APPS_DIR" >&2; exit 1; }

git clone -b main https://github.com/ban-sky/appstore "$IMPORT_DIR"
cp -a "$IMPORT_DIR/apps/." "$LOCAL_APPS_DIR/"

find "$IMPORT_DIR" -xdev -mindepth 1 -delete
rmdir "$IMPORT_DIR"
```

执行完成后，在 1Panel 应用商店中刷新本地应用。

## 镜像

上游 `zerotier/zerotier` 官方镜像停留在 `1.14.2`，因此本仓库通过 GitHub Actions 使用 ZeroTier 官方 `Dockerfile.release` 与官方 Debian 软件源构建镜像，构建和发布都在 GitHub 完成：

- `ghcr.io/ban-sky/zerotier:1.16.2`
- `ghcr.io/ban-sky/zerotier:latest`

构建流程见 [build-zerotier.yml](.github/workflows/build-zerotier.yml)。

> 首次发布到 GHCR 的包默认是私有的。工作流会尝试自动改为公开；若失败，请到
> `https://github.com/users/ban-sky/packages/container/zerotier/settings` 手动将可见性改为 **Public**，
> 否则 1Panel 拉取镜像会失败。

## 更新应用版本

1. 新建版本目录，例如 `apps/zerotier-one/1.16.2/`，把 `docker-compose.yml` 中的镜像 tag 改为新版本号；
2. 同步 `apps/zerotier-one/latest/`；
3. 提交并推送到 `main`，GitHub Actions 会自动检测最新的版本目录并构建同 tag 的镜像。

## 目录结构

```text
.
├── .github/workflows/build-zerotier.yml   # 构建并推送 GHCR 镜像
└── apps
    └── zerotier-one
        ├── README.md                      # 应用商店中显示的说明
        ├── data.yml                       # 应用元数据
        ├── logo.png
        ├── 1.16.2/                        # 固定版本
        │   ├── .env.sample
        │   ├── data.yml
        │   └── docker-compose.yml
        └── latest/                        # latest 版本
            ├── .env.sample
            ├── data.yml
            └── docker-compose.yml
```

## 致谢

应用配置来自 [okxlin/appstore](https://github.com/okxlin/appstore)，镜像基于 [zerotier/ZeroTierOne](https://github.com/zerotier/ZeroTierOne) 官方发布文件构建。
