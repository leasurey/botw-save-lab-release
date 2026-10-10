# 更新日志

本文件记录发行版的版本变化。格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/)。

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
