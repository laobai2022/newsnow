## 前置准备
- 确认您对 `https://github.com/laobai2022/newsnow.git` 有推送权限（需管理员或协作者权限）。
- 选择认证方式：
  - HTTPS + 个人访问令牌（PAT，建议，作用域包含 `repo`）。
  - 或 SSH（需先在 GitHub 绑定公钥）。
- 当前项目为 Python 项目，已存在 `.gitignore`（忽略 `__pycache__/`、`*.py[oc]`、`build/`、`dist/`、`wheels/`、`*.egg-info`、`.venv`）。无需新增忽略规则。

## 仓库初始化（在 `f:\LAOBAI\mcp_newsnow-master`）
- 初始化 Git 并设置默认分支为 `main`：
  - `git init`
  - `git config init.defaultBranch main`
- 配置用户信息（若全局未配置）：
  - `git config user.name "您的名字"`
  - `git config user.email "您的邮箱"`
- 添加远程：
  - `git remote add origin https://github.com/laobai2022/newsnow.git`
  - 如已存在 `origin`，使用：`git remote set-url origin https://github.com/laobai2022/newsnow.git`

## 提交本地代码
- 生成初始提交：
  - `git add .`
  - `git commit -m "chore: import mcp_newsnow initial codebase"`

## 同步与推送策略
- 统一分支名并检查远端：
  - `git branch -M main`
  - `git fetch origin`
- 若远端为空或仅默认初始提交：
  - `git push -u origin main`
- 若远端已存在历史且不同步（常见：已有 README/License）：
  - 先对齐：`git pull --rebase origin main`
  - 解决冲突后再：`git push -u origin main`
  - 如需保留本地为新分支：`git push -u origin main:mcp_newsnow-import` 并在 GitHub 发起 PR

## 认证与安全
- HTTPS 方式：推送时密码处输入 GitHub PAT（作用域至少含 `repo`）。
  - 建议启用 Git Credential Manager（Windows 默认）：后续自动保存。
- SSH 方式：
  - 生成密钥：`ssh-keygen -t ed25519 -C "您的邮箱"`
  - 将公钥加入 GitHub，然后将远程改为：`git remote set-url origin git@github.com:laobai2022/newsnow.git`

## 验证与回滚
- 验证：在仓库页面检查最新提交是否可见；本地运行：
  - `git status`、`git log --oneline -n 5`
- 遇到非快进错误：
  - 备份本地分支：`git branch backup/local-main`
  - 选择 rebase 或新分支推送 + PR，避免强推覆盖远端历史。

## 我将执行
- 在上述目录按步骤执行初始化、提交与推送。
- 采用 HTTPS + PAT（默认），如您偏好 SSH 我也可改为 SSH。
- 完成后给出推送结果与 GitHub 链接以便您确认。请确认是否按此计划执行，并告知首选认证方式。