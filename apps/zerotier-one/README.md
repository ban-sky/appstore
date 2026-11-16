# ZeroTier-One

## 应用简介
多点虚拟化网络解决方案。

英文说明：A Smart Ethernet Switch for Earth.

## 部署说明
- 本应用使用 Docker Compose 在 1Panel 中部署。
- 应用分类：工具。
- 支持架构：amd64。
- 可选版本：`1.16.2`、`latest`。
- 镜像由本仓库的 GitHub Actions 基于 ZeroTier 官方 Debian 软件源构建，发布在 `ghcr.io/ban-sky/zerotier`（官方 `zerotier/zerotier` 镜像停留在 1.14.2）。
- 容器使用 host 网络，并需要 `/dev/net/tun` 设备与 `NET_ADMIN`、`SYS_ADMIN` 权限。

## 端口
| 变量 | 说明 | 默认值 | 必填 |
| --- | --- | --- | --- |
| PANEL_APP_PORT_HTTP | 端口 | 9993 | 是 |

## 数据持久化
- `./data:/var/lib/zerotier-one`

升级或迁移前，请在 1Panel 中备份上述数据目录（其中包含节点身份 `identity.secret`）。

## 使用说明
例子：容器管理界面连接容器终端，执行命令加入网络
```
zerotier-cli join [网络ID]
```

也可以设置环境变量 `ZEROTIER_JOIN_NETWORKS`，在容器启动时自动加入网络（多个网络用空格分隔）。

## 参考资料
- 官网: <https://www.zerotier.com>
- 文档: <https://docs.zerotier.com>
- 源码: <https://github.com/zerotier/ZeroTierOne>

## 许可证
- ZeroTier Agent 采用 MPL-2.0 许可证，见上游 `LICENSE-MPL.txt`。
- 镜像内的网络控制器等组件位于上游 `nonfree/` 目录，采用 ZeroTier Source-Available License（仅限非商业用途），商业用途需向 ZeroTier 获取商业许可。
- 上游许可证说明: <https://github.com/zerotier/ZeroTierOne/blob/1.16.2/nonfree/LICENSE.md>
