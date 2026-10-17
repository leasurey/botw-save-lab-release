<div align="center">

# BOTW 存档实验室

**《塞尔达传说：旷野之息》Wii U 1.5.0 · 离线存档编辑器**

![版本](https://img.shields.io/badge/%E7%89%88%E6%9C%AC-V13.9-2ea44f)
![平台](https://img.shields.io/badge/%E5%B9%B3%E5%8F%B0-Windows%2010%20%7C%2011-0078D4)
![游戏](https://img.shields.io/badge/%E5%AF%B9%E8%B1%A1-Wii%20U%201.5.0-3fb950)
![许可](https://img.shields.io/badge/License-MIT-yellow)

**免安装 · 无需 Python · 无需联网 · 写回前自动备份 · 写后回读校验**

[**⬇️ 下载 V13.9**](https://github.com/leasurey/botw-save-lab-release/releases/tag/v13.9)　·　[使用说明](./使用说明.md)　·　[更新日志](./CHANGELOG.md)　·　[源码仓库](https://github.com/leasurey/botw-save-lab)

</div>

---

## 这是什么

终端程序负责读取、转换、写回 `game_data.sav`；内置网页编辑器负责可视化修改。全程在本机离线运行，**不上传任何存档**。

- 改背包 / 装备 / 马匹 / 位置 / 旗标 / 卢比 / 挑战进度
- 写回前自动 `.bak`，备份失败直接中止
- 写档走「临时文件 + 原子替换」，断电不会留下半截存档

> 仅针对 **Wii U 1.5.0 日版**存档（signature `18203` / `0x471B`）。其他版本/地区不保证兼容。

---

## 下载

| 项目 | 值 |
|---|---|
| 最新版本 | **V13.9**（2026-10-17） |
| 安装包 | [`botw-save-lab-v13.9-windows.zip`](https://github.com/leasurey/botw-save-lab-release/releases/download/v13.9/botw-save-lab-v13.9-windows.zip) |
| 大小 | 10.79 MB（解压后约 27 MB） |
| SHA-256 | `902635ab1b01edc44866be77ed5dad2345a098535292a9a45d4685c028febecb` |
| 运行环境 | Windows 10 / 11 x64，**不需要安装 Python** |

解压后目录（**整个文件夹要放在一起，不要单独把 exe 拖出来**）：

```text
BOTW存档实验室V13.9\
├─ botw_lab.exe          ← 双击启动
├─ _internal\            ← 内置 Python 运行时
├─ editor\               ← 网页编辑器 + 图标 + 挑战数据
├─ BotW-Save-Editor\     ← 备用转换器 + 字段表
└─ 使用说明.txt
```

> 第三方转换器与字段表已随包内置，**无需自行准备**。

---

## 快速开始

1. **放存档**：把你的 `game_data.sav`（从 Cemu / Wii U 导出）放到 `botw_lab.exe` 同一级。
2. **启动**：双击 `botw_lab.exe`（普通使用不需要管理员权限）。
3. **选流程**：菜单里直接按回车，走 **[5] 完整流程**。
4. **改数据**：自动打开浏览器进入网页编辑器，改完点顶栏 **「🎯 一键导出（自动写回）」**。
5. **写回**：回到控制台按提示回车；覆盖原档需输入 `y` 确认。

```text
game_data.sav
   ↓  SAV → JSON
网页编辑器（本机 127.0.0.1）
   ↓  一键导出（自动写回 backups/<日期>/）
终端写回 SAV
   ↓  回读校验 SHA-256
完成（原档保留，改动前有 .bak）
```

> 修改前请先自行备份一份原始存档。

---

## 功能

| 模块 | 能改什么 |
|---|---|
| **savdata 旗标** | 任务/神庙/道具获取标记、卢比、心心、耐力等全部数值字段 |
| **背包** | 数量、耐久、修饰符与强度、装备状态、护甲染色、新增/删除物品、批量修改 |
| **马匹** | 名字、亲密度、鬃毛/缰绳/马鞍、登记状态 |
| **位置** | X/Y/Z 坐标、朝向、地图区域；可从地点库直接传送 |
| **挑战 / 任务** | 主线索、神庙、支线、回忆等完成状态 |
| **存档体检** | 结构校验、字段异常、背包幽灵物品、IsGet 旗标一致性 |

其他：撤销栈（四套独立）、全部还原、对比原文件、JSON 校验、Downloads 自动找回。

---

## 两条导出通道（V13.9）

| 通道 | 入口 | 行为 |
|---|---|---|
| **① 自动写回**（推荐） | 顶栏 `🎯 一键导出（自动写回）` | 直接覆盖 `backups\<日期>\` 的四个 JSON，终端立刻检测到改动 |
| **② 手动导出** | 各 `💾 导出 XXX / 保存修改` 按钮 | 文件落到浏览器下载目录，需要自己搬运；页面会标注「终端看不到这份文件」 |

顶栏会显示当前导出模式；每个文件标注 **✓已写回** / **↓已下载(未写回)**。
**手动下载不会压制之后的自动写回**——这是 V13.9 修掉的一个「修改静默丢失」问题。

---

## 安全机制

- **备份闸**：每次写回前在 `backups\` 生成 `.bak`；备份失败或副本 SHA 不一致 → **立即中止**，不碰活档。
- **条数闸**：savdata 条数必须与目标 SAV 原档一致，截断的 JSON 会被拒绝。
- **原子写**：先写临时文件并 `fsync`，校验字节数后再 `os.replace`。
- **覆盖确认**：打印源/目标 SHA-256，必须输入 `y` 才覆盖原档。
- **只写本机**：写回端点仅监听 `127.0.0.1`，一次性令牌 + 同源校验，只允许写 `backups\<日期>\`。
- **可回滚**：改坏了，关闭 Cemu，把 `backups\` 里对应 `.bak` 改回 `game_data.sav` 即可。

想彻底禁止覆盖原档：

```text
set BOTW_NO_OVERWRITE_ORIG=1
```

---

## 常见问题

| 问题 | 处理 |
|---|---|
| 浏览器没自动打开 | 终端会打印 `http://127.0.0.1:端口/...`，手动复制到浏览器 |
| 提示没找到 `.sav` | 确认 `game_data.sav` 与 `botw_lab.exe` 同一级 |
| 写回失败 | 先关闭正在占用存档的 Cemu / 游戏，再重试 |
| 一键导出提示失败 | 失败文件会自动改为浏览器下载，按页面提示搬运；或检查是否在 `backups\<日期>\` 目录下 |
| 杀软拦截 exe | 未签名的 PyInstaller 程序可能被误报，添加信任即可 |
| 想确认文件完整 | 在文件夹里执行 `botw_lab.exe --selfcheck` |

更多问题见 [使用说明](./使用说明.md)。

---

## 版本历史

| 版本 | 日期 | 主要变化 |
|---|---|---|
| [**V13.9**](https://github.com/leasurey/botw-save-lab-release/releases/tag/v13.9) | 2026-10-17 | 导出拆两条通道、修复撤销与显示缺陷、写回加固、新增 `--selfcheck` |
| [V13.8](https://github.com/leasurey/botw-save-lab-release/releases/tag/v13.8) | 2026-10-17 | 修复网页按钮无响应；一键导出直接写回 `backups/` |
| [V13.7](https://github.com/leasurey/botw-save-lab-release/releases/tag/v13.7) | 2026-10-10 | 首个公开发行版（免安装 EXE） |

完整变更见 [CHANGELOG.md](./CHANGELOG.md)。

---

## 系统要求

- Windows 10 / 11（64 位）
- 约 30 MB 磁盘空间（含解压后的运行时）
- 无需 Python、Node.js 或联网
- 存档：Wii U 1.5.0 日版 `game_data.sav`

---

## 第三方致谢

核心代码为独立实现，参考并使用了以下社区公开资料：

| 来源 | 使用内容 |
|---|---|
| [Mystixor / BotW-Save-Editor](https://github.com/Mystixor/BotW-Save-Editor) | SAV 格式研究、`savdata_list*.json` 字段表、C++ 备用转换器 |
| uking_saves | 背包 / 马匹 / 位置字段哈希布局 |
| [MrCheeze / botw-waypoint-map](https://github.com/MrCheeze/botw-waypoint-map) | 部分传送点坐标 |
| [Marc Robledo / savegame-editors](https://github.com/MarcRobledo/savegame-editors) | 护甲染色字段参考、装备映射、`editor/assets/` 图标 |
| kailous / Botw-Savediter | 部分中文物品名称与分类设计 |

详见 [NOTICES.md](./NOTICES.md)。

---

## 版权与许可

- 原创代码采用 **MIT License**，见 [LICENSE](./LICENSE)。
- 物品名称、任务文本、旗标名称等游戏数据来源于《塞尔达传说：旷野之息》Wii U 版，版权归 **Nintendo** 及相应权利人所有。
- 本项目为社区非官方工具，与 Nintendo 及上述第三方作者无隶属关系。
- 开发过程使用了 AI 编程助手辅助。

> 修改存档有风险，请自行备份并承担使用后果。遇到问题请提交 Issue，**不要上传真实存档或含个人信息的文件**。
