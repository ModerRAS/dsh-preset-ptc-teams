# REPORT — dsh-preset-ptc-teams

- 日期：2026-11-11（Asia/Shanghai）
- 执行人：preset-smith（subagent）
- 本地仓库：`C:/WorkSpace/Claw/projects/dsh-preset-ptc-teams/`（git 已init，commit `b13931f` "PTC Teams preset plugin 1.0.0"，8 files / 238 insertions）

## 1. GitHub 仓地址

已推送：https://github.com/ModerRAS/dsh-preset-ptc-teams （Lead 接力，`gh repo create --public --source . --push`，branch master 跟踪 origin/master）。

原子弟 `gh auth status` 未登录是其会话钥匙串不同所致，Lead 会话内 `gh` 为 ModerRAS 已登录，无需担心。"

Lead 接力（2 条命令）：

```powershell
cd C:/WorkSpace/Claw/projects/dsh-preset-ptc-teams
gh auth login
gh repo create dsh-preset-ptc-teams --public --source . --push
```

## 2. 装机验证

**未安装 —— 本 subagent 工具域无 `plugin_manager`，无法执行 `install_bundle`。**

Lead 接力：在 desktop profile 的会话里调 `plugin_manager` `action: install_bundle`，
`target` = `C:/WorkSpace/Claw/projects/dsh-preset-ptc-teams`（仓库根，`package.json` 所在目录），
然后去 设置 → Agent 预设 确认出现 **PTC Teams**。成功后把截图/文字补在这里。

## 3. 与 ptc 的 diff 点（已程序化验证）

- 方法：把双方 `cordis.patch.yml` 去掉注释、预设身份行（id/order/name/description）和新增的 3 行 `disabled: true` 后逐行比对 → **128 行完全一致**。
- 全部差异只有 4 处：
  1. 行 id `preset-ptc` → `preset-ptc-teams`；config `id: ptc / order: 2` → `id: ptc-teams / order: 3 / name: PTC Teams / description: PTC composition without subagent tools; use teammates instead.`
  2. `delegation → tool-subagent-control` 加 `disabled: true`
  3. `delegation → tool-subagent`（spawn）加 `disabled: true`（config 原样保留）
  4. `delegation → tool-subagent-fork`（fork）加 `disabled: true`（config 原样保留）
- persona 前缀等其余一字不动；ptc 真身目录零写入（只读）。

## 4. 打包说明（与 dsh-llm-wiki 的有意差异）

- 本包是**配置型 bundle**，照 `cordis-plugin-development → templates/mcp` 模板：
  `package.json`（`dsh.bundle.patch`）+ `cordis.patch.yml` + `locale/{en,zh}.json`（meta.title/description）+ `icon.svg`（原创三节点图，451B），全部位于仓库根（2026-11-11 已把 `dsh/` 提至根目录，否则 `install_bundle` 认不出 bundle）。
- 无 Host/Client 代码、无依赖、无构建、无 dist——`install_bundle` 直装。
  dsh-llm-wiki 的重型 packaging（`dsh/host-index.js` re-export `../dist` + `pnpm build:dsh`）是给有 Host 逻辑的插件用的，纯预设不需要，故意不用。
- JSON 全合法，icon 远小于 256KiB 上限。

## 5. 合规

- 新项目落在 `projects/dsh-preset-ptc-teams/`（AGENTS.md §1），目标无同名、系新建。
- 共享 workspace 零网盘改动、零工单改动；ptc 真身目录只读未碰。
