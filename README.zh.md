# dsh-git

一个 DSH 插件：给右侧栏加一个 **Git** 页面——左边是工作区改动与最近提交，右边是点开的 diff——只读、带高亮、双栏对齐。列表留在原地，所以两条 diff 可以并排比较。

[English](README.md) | 中文

![Git 页面：左边是改动与提交，点开的 diff 就在它们旁边。点提交展开它改动的文件。](docs/screenshots/zh/01-log.png)

| 双栏 · 折行 | 双栏 · 不折行 | 单栏 |
| --- | --- | --- |
| ![默认：长行在各自半栏里折行](docs/screenshots/zh/02-diff-split.png) | ![每一栏各自横向滚动长行，行号钉住不动](docs/screenshots/zh/03-diff-unwrapped.png) | ![单栏：每条改动先删后增](docs/screenshots/zh/04-diff-inline.png) |

## 安装

需要 Node 22.19+（或 24+）与 DSH CLI。本插件跟随 `0.2.0` 这条 DSH 线（`^0.2.0-rc.2`）。

Web 端：

```sh
dsh plugin --profile web add @lengmoxxl/dsh-git
dsh --profile web
```

DSH Desktop：桌面版 profile 由 Electron 应用自己管理，不会按需跑包管理器，所以按「手动 link 一层」的方式装：在 `~/.dsh/profiles/desktop/package.json` 里加一条 `link:` 依赖和一条 `dsh.profile.bundles` 条目，再把该包软链到 profile 的 `node_modules/@lengmoxxl/` 下，然后重启应用。

```jsonc
// ~/.dsh/profiles/desktop/package.json
"dependencies": { "@lengmoxxl/dsh-git": "link:/path/to/dsh-git" },
"dsh": { "profile": { "bundles": [ /* …, */ "@lengmoxxl/dsh-git" ] } }
```

```sh
ln -s /path/to/dsh-git ~/.dsh/profiles/desktop/node_modules/@lengmoxxl/dsh-git
```

装好后重启应用即可：Git 页面从右侧栏的 `+` 添加控件里进入。

## 依赖、权限与限制

- `PATH` 上有 `git`，并且会话工作目录是一个 Git 工作区。
- 通过 Harness 文件系统读取该仓库，通过 Harness 子进程运行 `git`：不访问网络，不读凭据，不碰其他路径。
- 不写入任何东西——工作区和索引都不会被改动。命令以 DSH 进程自身的权限运行，仓库或 diff 很大时要花时间采集和绘制。

## 发布

在干净的 `main` 上：

```sh
npm version patch -m "Cut %s"   # 或 minor / major
npm publish
```

## License

MIT
