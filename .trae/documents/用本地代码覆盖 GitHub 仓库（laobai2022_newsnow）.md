## 行动目标
将 `https://github.com/laobai2022/newsnow.git` 的远程内容全部替换为当前本地目录 `f:\LAOBAI\newsnow-main` 的代码，并确保推送成功、可回滚、无泄密。

## 前提与影响
- 远程仓库目前是 MCP NewsNow Server（聚合 14+ 平台热点，支持 `NEWS_API_URL` 等），本次覆盖将删除其现有实现与历史（如 `get_newsnow`/`get_multi_news`/`get_all_news`/`list_sources` 相关代码）。
- 若开启 branch protection（保护分支），需暂时允许 force push。
- 覆盖远程会重写历史，建议保留“旧版本备份分支/标签”。

## 步骤一：为旧仓库做备份（推荐）
1. 在任意临时目录：
   - `git clone https://github.com/laobai2022/newsnow.git`
   - `cd newsnow`
   - 确认默认分支：`git rev-parse --abbrev-ref HEAD`（通常为 `main`）
   - 创建备份分支：`git branch archive/legacy-$(Get-Date -Format yyyyMMdd)`
   - 推送备份分支：`git push origin archive/legacy-$(Get-Date -Format yyyyMMdd)`
   - 可选：打标签：`git tag legacy-$(Get-Date -Format yyyyMMdd)`，`git push origin --tags`

## 步骤二：准备本地代码（在 `f:\LAOBAI\newsnow-main`）
- 若本地已是 Git 仓库：
  1. `git status` 确认干净工作区（如有未提交文件，先 `git add . && git commit -m "local code ready"`）
  2. 规范分支名：`git branch -M main`
  3. 添加/修正远程：`git remote set-url origin https://github.com/laobai2022/newsnow.git`（若无则 `git remote add origin ...`）
- 若本地不是 Git 仓库：
  1. `git init`
  2. `git add .`
  3. `git commit -m "Replace repo with local code"`
  4. `git branch -M main`
  5. `git remote add origin https://github.com/laobai2022/newsnow.git`

## 步骤三：必要的安全与体积检查
- `.gitignore`：确保忽略构建产物、缓存、敏感文件（如 `.env`）。
- 秘钥扫描：本地快速检查
  - `git grep -I -n -E "(API_KEY|SECRET|PASSWORD|TOKEN)"`
- 大文件：如含媒体/模型，使用 Git LFS
  - `git lfs install`
  - `git lfs track "*.mp4"`（按需）
  - `git add .gitattributes && git commit -m "Add LFS tracking"`
- 许可证：保留或新增 `LICENSE`（MIT），并在 `README` 简述覆盖说明（如需）。

## 步骤四：强制覆盖推送
- 确认可登录 GitHub（使用凭据管理器或 PAT，不要把令牌写进 URL）。
- 进行推送（推荐更安全的 lease）：
  - `git fetch origin`
  - `git push --force-with-lease -u origin main`
- 如遇保护分支限制：暂时在 GitHub 关闭保护或允许 force push，然后重试。

## 步骤五：验证
- 打开仓库主页，确认文件树已是本地版本。
- `git ls-remote https://github.com/laobai2022/newsnow.git` 查看远程 HEAD 指向本地最新提交。
- 新克隆测试：
  - 在新目录 `git clone https://github.com/laobai2022/newsnow.git`
  - `cd newsnow && git log -n 1` 验证提交信息与内容一致。

## 回滚/保留方案
- 若已创建备份分支/标签，可在 GitHub 发起 PR 将 `archive/legacy-YYYYMMDD` 合并回 `main`，或 `git reset --hard origin/archive/legacy-YYYYMMDD` 后再推送覆盖回滚。

## 常见问题
- 认证失败：在 PowerShell 运行一次 `git credential-manager-core configure` 或通过 GitHub Desktop 登录。
- branch 名不一致：用 `git branch -M main` 统一到 `main`；或改远程默认分支。
- 大文件拒绝：启用 LFS 后重新提交并推送。
- 保护分支：在仓库设置里短暂允许 force push。

## 我将执行
1) 远程旧代码备份（创建 `archive/legacy-20251114` 分支并推送）
2) 规范/初始化本地 Git 并设置远程为 `newsnow`
3) 安全与体积检查（`.gitignore`/秘钥扫描/LFS）
4) `--force-with-lease` 推送覆盖到 `main`
5) 验证结果与提供回滚指引

请确认是否按此方案执行；若不需要备份，我将直接进行覆盖推送。