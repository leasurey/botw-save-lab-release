# BOTW Save Lab · 正式发行版

《塞尔达传说：旷野之息》Wii U 1.5.0 离线存档编辑器（**免安装 Windows EXE，无需 Python 环境**）。

本仓库只放**发行产物与说明**；源码与构建脚本见上游仓库 [leasurey/botw-save-lab](https://github.com/leasurey/botw-save-lab)。

## 下载使用
1. 打开右侧 **Releases**，下载最新的 `botw-save-lab-v13.7-windows.zip`。
2. 解压后双击 `botw_lab.exe`（整个文件夹保持在一起）。
3. 把你的 `game_data.sav` 放到 `botw_lab.exe` 同一级。
4. 控制台里直接按回车，走「完整流程」，浏览器会自动打开网页编辑器。

详细步骤见 [使用说明.md](./使用说明.md)。

## 功能
- savdata 旗标 / 背包物品 / 马匹 / 玩家位置 / 挑战任务编辑
- 写回前自动备份 `.bak`，写回后自动回读校验
- 覆盖原档前显示 SHA-256 并需手动确认

## 安全说明
- 目标版本：Wii U 1.5.0 日版（signature 18203 / 0x471B），其他版本不保证兼容。
- 普通使用无需管理员权限。
- 第三方转换器 `BotW-Save-Editor.exe` 为可选备用；主程序用内置 Python 转换器即可运行。

## 许可
原创代码 MIT。第三方文件与游戏素材的版权归原作者，再分发前请见 [NOTICES.md](./NOTICES.md) 确认授权。
