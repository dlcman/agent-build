# Agent Build

agent-project 的 CI 构建外壳，基于 GitHub Actions 免费容器。

## 架构

```
GitHub: dlcman/agent-source (私有)   ← 源码，nuitka-build 分支
GitHub: dlcman/agent-build (公开)    ← CI workflow (actions/checkout 拉源码)
  ├─ Nuitka --standalone 三平台编译 (Linux / macOS / Windows)
  ├─ smoke test 启动验证
  ├─ 产物带版本号 (日期-commit SHA)
  └─ 自动推送 → gitee 开源下载仓库

Gitee: dlcboy/deepbind-assistant (公开) ← 产物下载
```

## 触发方式

- **自动**: 每 30 分钟自动构建三平台产物
- **手动**: [Actions](https://github.com/dlcman/agent-build/actions) → "Build Agent Project" → Run workflow
- 多平台并行，fail-fast: false

## 产物

- 命名: `agent-project-{日期}-{commit SHA}-{平台}.zip`
- 下载: https://gitee.com/dlcboy/deepbind-assistant
- 内容: `agent-project.dist/` + `.env.example` + `run.sh`/`run.bat`

## 必需的 Secrets

| Secret | 说明 |
|--------|------|
| `GITEE_TOKEN` | gitee 私人令牌，用于推送产物到 deepbind-assistant |

配置: Settings → Secrets and variables → Actions → New repository secret

## 源码维护

源码在 `dlcman/agent-source` 私有仓库。本地推更新:

```bash
# agent-project 目录下
git push source nuitka-build
```

推送后 30 分钟内 CI 自动构建并发布新产物。