# dsh-preset-ptc-teams

独立 DSH 插件：提供 Agent 预设 **PTC Teams**（= ptc 全量组合减掉 subagent 工具）。

## 用途

在 Agent Teams / 多人协作场景下，主会话不需要 `subagent` / `subagent_fork`（子代理各自带完整 ptc 上下文，开销大、易跑偏），改用 teammates 协作。本预设把 ptc 里三个子代理工具默认关闭，其余一字不动。

## 与 ptc 的差异（仅 4 处）

| # | 位置 | ptc | PTC Teams |
|---|------|-----|-----------|
| 1 | 预设声明 | `id: ptc / order: 2`（built-in，无 name/description） | `id: ptc-teams / order: 3 / name: PTC Teams / description: PTC composition without subagent tools; use teammates instead.` |
| 2 | `delegation → tool-subagent-control` | 启用 | `disabled: true` |
| 3 | `delegation → tool-subagent`（spawn） | 启用 | `disabled: true`（config 原样保留） |
| 4 | `delegation → tool-subagent-fork`（fork） | 启用 | `disabled: true`（config 原样保留） |

其余插件表（persona 前缀、agent-instructions、time、bash/pwsh、fs、jobs、schedule、skill、goal、planning、compaction、workflow-ptc/tool-workflow 已禁用、ask-user、todo、web、executor-ptc、present、plugin-manager/market 已禁用）与 ptc 完全一致。

参考只读来源（未修改）：`scratch/dsh-msg/dsh-harness/packages/bundle/web-app/presets/ptc.patch.yml`。

## 打包形态

配置型 bundle（照 `cordis-plugin-development → templates/mcp` 模板）：
`dsh/package.json`（`dsh.bundle.patch`）+ `dsh/cordis.patch.yml` + `dsh/locale/*.json` + `dsh/icon.svg`。
无 Host/Client 代码、无依赖、无构建步骤——`install_bundle` 直装。这是有意偏离 dsh-llm-wiki 重型 packaging（Host entry re-export `../dist`）之处：纯预设不需要 JS。

## 安装

1. DSH 命令（desktop profile，当前会话所在 profile）：
   `plugin_manager` → `action: install_bundle`，`target` 填本仓库的绝对 `dsh/` 目录。
2. 绝不手改 profile 的 `package.json` / `cordis.patch.yml`（由 `install_bundle` 写）。
3. 重启 profile（替换安装需 restart 加载新模块世代；本包无 JS，一般 HMR 即生效）。

## 切换预设

设置 → Agent 预设 → 选择 **PTC Teams**。只影响之后的新会话；已在 running 的会话保持原预设。

## 验证

- 设置 → Agent 预设列表出现 `PTC Teams`。
- 新会话工具表无 `subagent` / `subagent_fork`，其余与 ptc 一致。
- 见 `REPORT.md`（装机验证记录）。
