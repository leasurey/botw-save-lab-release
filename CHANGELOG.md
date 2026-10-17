# 更新日志

本文件记录发行版的版本变化。格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/)。

## [V13.9] - 2026-10-17

本次更新来自一次全量缺陷审查（17 条，报告见上游仓库 `BUG_REVIEW_REPORT.md`），重点修复「修改静默不写回」与「撤销不可用」。

### 🐛 修复（会导致修改静默丢失）
- **手动下载不再伪装成「已写回」**：旧版在不支持 File System Access API 的浏览器（Firefox/Safari）里，点「导出」只落到下载目录，却仍标记为已导出，「一键导出」因此跳过该文件，终端也检测不到改动。现在区分 `written`（真写回）与 `manual`（仅下载），两者互不干扰。
- **写回接口 `200 + errors` 不再被当成成功**：服务端改为逐文件返回 `written/errors`，前端只认真实写回的文件；失败项自动转浏览器下载并显示原因。

### 🐛 修复（撤销 / 显示）
- 纯背包会话里「撤销」按钮一直灰着：判定改为四个撤销栈任一非空。
- 马匹页 / 位置页撤销撤错对象（视图名写成 `horse`/`position`，实际是 `horses`/`pos`；`renderPosition()` 不存在）。
- 新增 / 删除 / 批量 / 快速添加 / 修饰符 / 强度 / 染色 / 升星全部纳入撤销栈。
- 护甲染色改完卡片不刷新、马匹亲密度 15% 显示成 0%、关闭页面误报未保存。

### 🛡 加固
- 备份失败立即中止；写档改为临时文件 + 原子替换；savdata 条数必须与目标 SAV 一致。
- 写回端点仅监听 `127.0.0.1`，一次性令牌 + Origin 白名单，只允许写 `backups/<日期>/`。
- 字段表缺失/损坏时仍可导出（字段名退化为 `Unknown_<哈希>`，数值照常逐字节写回）。
- C++ 备用转换器：导出方向在本机必崩 `0xC0000409`（上游二进制问题，短路径同样复现），失败时给出明确诊断；导入方向实测可用。
- 新增 `--selfcheck`，打印解析到的路径并检查资源完整性。

### 📦 发行包
- 与 V13.8 完全同构：`BOTW存档实验室V13.9\`（`botw_lab.exe` + `_internal\` + `editor\` + `BotW-Save-Editor\`），46 个条目 / 10.79 MB。
- `game_data.sav` 与 `backups\` 仍固定在 exe 同级，包内不含任何个人存档。

[V13.9]: https://github.com/leasurey/botw-save-lab-release/releases/tag/v13.9

## [V13.8] - 2026-10-17

### 🐛 修复
- 修复网页编辑器所有按钮点击无反应的问题（`savLoaded`/`invLoaded`/`horseLoaded`/`posLoaded` 四个标志原本被声明在函数内部，IIFE 顶层代码读取时抛 `ReferenceError`；已提升到顶层声明）。
- 未找到存档时的提示不再打印本机绝对路径（含用户名），改为通用文案「把 game_data.sav 放到 botw_lab.exe 所在的文件夹即可」。

### ✨ 新增
- 「一键导出」在本地服务模式下直接把修改写回 `backups/<日期>/`，省去下载四个文件再手工搬运的步骤。
- 新增服务端 `POST /__botw_write` 端点与 `backups/` 路径白名单校验，仅监听 `127.0.0.1`，单次请求体上限 64 MB。

### ⚠️ 兼容性
- 双击 `file://` 打开网页时无本地服务端点，一键导出自动退回原来的浏览器下载流程，行为同 V13.7。

[V13.8]: https://github.com/leasurey/botw-save-lab-release/releases/tag/v13.8

---

## [V13.7] - 2026-10-10

首个公开发行版。

### 新增
- 免安装 Windows EXE：内置 Python 运行时，目标用户无需安装 Python。
- 终端菜单 + 本地网页编辑器，全程离线可用。
- 支持 savdata 旗标、背包物品、马匹、玩家位置、挑战/任务数据编辑。
- 写回前自动 `.bak` 备份，写回后自动回读校验。
- 覆盖原档前进行 SHA-256 校验并需手动确认。
- 可选备用 C++ 转换器 `BotW-Save-Editor.exe`（主程序默认使用内置 Python 转换器）。

### 目标平台
- Wii U 1.5.0 日版 `game_data.sav`（signature 18203 / 0x471B）。

### 已知限制
- 其他游戏版本 / 地区存档不保证兼容。
- 不要在 Cemu 占用存档时直接覆盖写回。

[V13.7]: https://github.com/leasurey/botw-save-lab-release/releases/tag/v13.7
