# Agent Build

agent-project 的 CI 构建外壳，基于 GitHub Actions 免费容器。

## 架构

```
GitHub: dlcman/agent-build (公开)   ← CI workflow (免费无限分钟)
  ├─ git clone gitee 私有源码 (GITEE_TOKEN 认证)
  ├─ Nuitka --standalone 三平台编译 (Linux / macOS / Windows)
  ├─ 产物带版本号 (日期-commit SHA)
  └─ 自动推送 → gitee 开源下载仓库

Gitee: dlcboy/agent-project (私有) ← 源码，nuitka-build 分支
Gitee: dlcboy/deepbind-assistant (公开) ← 产物下载
```

## 触发方式

- **自动**: 每 30 分钟检测 `gitee.com/dlcboy/agent-project` nuitka-build 分支，有变化则编译
- **手动**: [Actions](https://github.com/dlcman/agent-build/actions) → "Build Agent Project" → Run workflow
- 多平台并行 (fail-fast: false，一个平台失败不影响其他)

## 产物

- 命名: `agent-project-{日期}-{commit SHA}-{平台}.zip`
  - 例: `agent-project-20260625-a1b2c3d-ubuntu-latest.zip`
- 内容: `agent-project.dist/` + `.env.example` + `run.sh`/`run.bat` + `README_BUILD.md`
- 下载: https://gitee.com/dlcboy/deepbind-assistant

## 必需的 Secrets

| Secret | 说明 |
|--------|------|
| `GITEE_TOKEN` | gitee 私人令牌，需 `repo` 权限 |

配置路径: Settings → Secrets and variables → Actions → New repository secret

## 本地使用产物

1. 解压 zip，编辑 `.env.example` 改名为 `.env` 填入配置
2. Linux/macOS: `./run.sh` (首次需 `chmod +x run.sh`)
3. Windows: 双击 `run.bat`
4. 浏览器打开 http://localhost:8003
