# HelmFox

本地数据库管理工具, 当前支持 PostgreSQL。连接配置与查询历史都留在自己机器上,
不经过任何服务器。

这个仓库只存放发布包与更新清单, 源码不公开。使用中遇到问题请提
[Issue](https://github.com/gokele/helmfox-releases/issues)。

## 下载

到 [Releases](https://github.com/gokele/helmfox-releases/releases/latest) 页取最新版本。

| 平台 | 下载 | 说明 |
|---|---|---|
| macOS 11+ | `HelmFox-<版本>.dmg` | Apple 芯片与 Intel 通用 |
| Windows 10 1809+ | `HelmFox-<版本>-windows-amd64.exe` | 免安装单文件 |
| Windows on ARM | `HelmFox-<版本>-windows-arm64.exe` | 免安装单文件 |

页面上还有几个 `.zip` 与一份 `updates.json`, 那是软件自动更新时用的, 手动下载请忽略。

## 安装

**macOS** — 双击 dmg 挂载, 把 HelmFox 拖进「应用程序」, 弹出即可。
安装包已签名并通过 Apple 公证, 不会出现"无法验证开发者"的拦截。

**Windows** — 下载 exe 直接双击运行, 不需要安装。

需要 WebView2 运行时: Win11 与打过更新的 Win10 自带, 缺少时装一个
[Microsoft Edge WebView2 Runtime](https://developer.microsoft.com/microsoft-edge/webview2/)。

首次运行会被 SmartScreen 拦一次 (未做 Windows 代码签名), 点「更多信息 → 仍要运行」。

**把 exe 放在有写权限的目录**, 用户目录下的任意位置都行。自动更新是就地替换这个 exe,
放进 `C:\Program Files` 会因为需要管理员权限而更新失败。数据不跟 exe 放在一起,
换掉 exe 不影响任何配置。

## 自动更新

启动后与之后每隔几小时静默检查一次, 只有确实存在新版本时才弹窗, 由你决定
立即更新、跳过此版本还是稍后提醒。没联网就跳过, 不打扰也不报错。
也可以随时从菜单或托盘手动检查。

更新包由 Ed25519 私钥签名, 公钥编译期钉死在程序里, 下载后逐字节校验摘要与签名 ——
即使这个仓库被拿下, 没有私钥也发不出能装上的更新。

## 数据放在哪

| 系统 | 位置 |
|---|---|
| macOS | `~/.helmfox` |
| Windows | `%USERPROFILE%\.helmfox` |

里面是元数据库 `helmfox.db` 与备份目录 `backups/`。连接密码存系统钥匙串,
从不写进这个数据库; 数据库里的连接信息、SQL 历史与结构快照经 AES-GCM 加密存储,
主密钥同样在钥匙串里, 单独拿到 `helmfox.db` 文件解不开。

卸载时删掉程序本体即可, 数据目录里还放着备份文件, 需要的话先取走。

## 系统要求

- macOS 11 或更高
- Windows 10 1809 或更高, 需要 WebView2 运行时
- 目标数据库: PostgreSQL 12 及以上
- 备份与恢复功能需要本机装有 `pg_dump` / `pg_restore` / `psql`, 版本不低于目标服务端

除更新检查外软件不联网, 离线完全可用。
