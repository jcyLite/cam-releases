# CAM Releases

[English](README.md) | [中文](README.zh-CN.md)

<p align="center">
  <img src="assets/cam-release-hero.svg" alt="CAM 将终端中的 AI 编码代理暂停转换为可远程处理的审批与回复" width="100%">
</p>

<p align="center">
  <a href="https://github.com/jcyLite/cam-releases/releases/latest"><img alt="最新版本" src="https://img.shields.io/github/v/release/jcyLite/cam-releases?label=latest%20release"></a>
  <a href="https://github.com/jcyLite/cam-releases/releases/latest/download/install-remote.sh"><img alt="远程安装脚本" src="https://img.shields.io/badge/install-one%20command-2f81f7"></a>
  <a href="https://github.com/jcyLite/cam-releases/releases"><img alt="平台支持" src="https://img.shields.io/badge/packages-macOS%20arm64%20%7C%20Linux%20x86__64%20%7C%20Android-1f6feb"></a>
</p>

CAM 是 AI 编码代理的远程控制室。Claude Code、Codex、Qoder 这类终端代理很强，
但它们仍会在关键节点停下来等待人类决策：批准命令、选择方案、补充需求、处理错误。
CAM 会监控这些暂停，把它们转换成结构化决策，让你在 Dashboard 里远程处理，而不是一直盯着终端窗口。

这个仓库是 CAM 的公开发布仓库，用来托管二进制包、安装脚本和 Android Dashboard APK。
源码仓库可以保持私有，安装入口保持简单稳定。

## 一行命令安装

```bash
curl -fsSL https://github.com/jcyLite/cam-releases/releases/latest/download/install-remote.sh | bash
```

安装脚本会自动识别平台，下载匹配的安装包，把 `cam` 安装到 `~/.local/bin`，
并将 Dashboard / OpenClaw 插件等运行时资源放到稳定的用户目录。

安装固定版本：

```bash
curl -fsSL https://github.com/jcyLite/cam-releases/releases/download/v0.1.11/install-remote.sh | \
  CAM_INSTALL_RELEASE=v0.1.11 bash
```

macOS 可选自动安装后台服务：

```bash
curl -fsSL https://github.com/jcyLite/cam-releases/releases/latest/download/install-remote.sh | \
  CAM_INSTALL_SERVICES=1 bash
```

## CAM 为什么不一样

<p align="center">
  <img src="assets/cam-decision-loop.svg" alt="CAM 决策闭环：代理暂停、CAM 检测、用户远程回复、代理继续执行" width="100%">
</p>

大多数 agent 工具负责启动任务，CAM 负责让任务持续推进。

| Agent 暂停时 | CAM 做什么 | 你做什么 |
| --- | --- | --- |
| 权限确认 | 检测待处理决策和风险上下文 | 批准、拒绝或发送自定义回复 |
| 方案或实现选择 | 捕获相关终端状态 | 从 Dashboard 或 CLI 回复 |
| 长任务状态变化 | 跟踪运行中、空闲、待输入和已完成状态 | 不打开每个终端也能掌握全局 |
| 多 Agent 协作 | 汇总团队活动和待处理消息 | 把后续指令路由给正确的 Agent |

CAM 的核心闭环很直接：

```text
Agent 需要输入 -> CAM 检测到暂停 -> Dashboard 展示决策 -> 你远程回复 -> Agent 继续执行
```

## 系统截图

公开安装包内置的就是 CAM 本地使用的同一套 Web Dashboard：Agent 状态卡片、
项目过滤、待确认决策、终端详情和右侧消息流。

<p align="center">
  <img src="assets/cam-dashboard-agents.png" alt="CAM Dashboard 展示 Agent 状态卡片和项目过滤" width="100%">
</p>

<p align="center">
  <img src="assets/cam-dashboard-terminal.png" alt="CAM 终端详情页展示消息流和 Agent 操作入口" width="100%">
</p>

## Release 包含什么

一次完整发布会包含：

- `install-remote.sh`
- `cam-local-<version>-darwin-arm64.tar.gz`
- `cam-local-<version>-darwin-arm64.tar.gz.sha256`
- `cam-local-<version>-linux-x86_64.tar.gz`
- `cam-local-<version>-linux-x86_64.tar.gz.sha256`
- `cam-dashboard-<version>.apk`

本地安装包包含 CAM CLI、OpenClaw 插件运行时、Bridge 服务和导出的 Web Dashboard 静态资源，
可以直接用于远程控制部署。

## Android Dashboard

从最新 Release 下载 Android Dashboard APK：

```bash
gh release download --repo jcyLite/cam-releases --pattern 'cam-dashboard-*.apk'
```

安装后填写 CAM Bridge 地址，例如：

```text
http://192.168.1.20:3456
```

登录时使用 CAM Bridge 显示的 Dashboard token。

## Release 检查

打开最新公开 Release：

```bash
gh release view --repo jcyLite/cam-releases --web
```

列出 Release 资源：

```bash
gh release view --repo jcyLite/cam-releases --json tagName,url,assets \
  --jq '{tagName,url,assets:[.assets[].name]}'
```

验证本机安装：

```bash
cam --version
cam service status
```

## 发布

Release 从源码仓库触发发布：

```bash
scripts/github-cam-release.sh --version 0.1.11 --android-version-code 11 --push
```

该流程会构建平台包、上传资源到本仓库，并保持公开安装地址稳定：

```text
https://github.com/jcyLite/cam-releases/releases/latest/download/install-remote.sh
```
