# 🎮 修仙宠物 (Xiuxian Pet)

<p align="center">
  <img src="img/2.png" width="200" alt="修仙宠物 Logo">
</p>

<p align="center">
  <b>一个会修仙的桌面宠物，陪伴你摸鱼、修炼、飞升！</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10+-blue.svg" alt="Python 3.10+">
  <img src="https://img.shields.io/badge/PyQt6-GUI-green.svg" alt="PyQt6">
  <img src="https://img.shields.io/badge/Go-Plugin-orange.svg" alt="Go Plugin">
  <img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="MIT License">
</p>

---

## ✨ 项目简介

**修仙宠物**是一款休闲、幽默风格的桌面宠物游戏，灵感源自《凡人修仙传》。你的桌面上会有一只可爱的小宠物，它会自动修炼、突破境界、穿戴装备，还能陪你聊天解闷！

> 🎯 **核心理念**：工作累了？来看看你的宠物修炼到什么境界了吧！说不定它已经飞升成仙，而你还在写 bug~ 😏

---

## 🖼️ 游戏截图

<p align="center">
  <img src="img/1.png" width="400" alt="游戏界面">
  <img src="img/3.png" width="400" alt="战斗场景">
</p>

---

## 🚀 核心功能

### 🎭 桌面宠物系统
- **自动修炼**：宠物会自己打坐修炼，无需操心
- **境界突破**：从炼气期到道祖境，体验完整修仙之路
- **属性成长**：攻击、生命、防御、智慧、悟性、灵力六大属性
- **装备系统**：武器、防具、法宝，打造你的专属神器
- **皮肤切换**：支持 GIF 动画皮肤，让你的宠物与众不同

### 🤖 AI 智能聊天
- **本地大模型**：基于 Ollama，无需联网也能聊天
- **智能感知**：检测系统闲置状态，自动找你聊天
- **多地方言**：东北话、天津话、北京话、四川话、孙笑川风格任选
- **气泡对话**：可爱的气泡对话框，逐字显示动画效果

### 🎮 丰富的游戏玩法

| 功能 | 描述 |
|------|------|
| 🏆 **排行榜** | 与全服玩家比拼修为境界 |
| 🏛️ **宗门系统** | 创建或加入宗门，与道友一起修炼 |
| 🗣️ **世界聊天** | 全服玩家实时交流 |
| 🏪 **拍卖行** | 买卖装备，发家致富 |
| 🗺️ **迷宫探索** | 随机迷宫生成，自动寻路探险 |
| ⚔️ **PVP 对战** | 与其他玩家一决高下 |

### 🔌 插件系统（亮点功能）

> 🌟 **这是本项目最引以为傲的功能！**

我们设计了一套**跨语言插件架构**，让第三方开发者可以用任何语言（Python、Go、C++ 等）为游戏开发插件！

#### 插件系统特点

- 🛡️ **进程隔离**：插件运行在独立进程中，崩溃不影响主程序
- 🔐 **加密通信**：Python 与 Go 之间使用 AES 加密通信
- 📡 **标准化协议**：统一的消息格式，易于开发
- 🔄 **自动重启**：插件崩溃后自动重启，高可用性
- 📦 **创意工坊**：内置插件市场，一键安装热门插件

#### 插件 API 接口

```python
# 获取玩家属性
get_player_attr("attack")  # 获取攻击力

# 设置玩家属性
set_player_attr("cultivation", 99999)  # 修改修为

# 执行系统命令
exec_method("sys_exit")  # 安全退出

# 插件私有数据
set_plugin_attr("my_data", {"level": 99})
```

#### 开发一个插件有多简单？

```go
package main

import (
    "anti-cheat/acore"
)

func main() {
    plugin := acore.NewAntiCPlugin("my_plugin", "我的插件", "1.0.0", "windows", "1.0", 15)
    
    // 连接桥接服务
    if !plugin.Connect("127.0.0.1", 15433) {
        return
    }
    
    // 注册插件
    plugin.Register()
    
    // 启动心跳
    plugin.StartHeartbeat()
    
    // 你的插件逻辑...
    select {}
}
```

### 🛡️ 安全防护

- **反作弊系统**：检测 IDA、x64dbg、Cheat Engine 等逆向工具
- **DLL 注入检测**：实时监控异常模块
- **禁止双开**：防止多开刷资源
- **数据加密**：双层 AES 加密存储游戏数据

---

## 📦 安装与运行

### 开箱即用-Windows

- Windows 10/11

### 运行游戏

```bash
xiuxian_xxx.exe
```

---

## 🎨 技术栈

| 技术 | 用途 |
|------|------|
| Python 3.10+ | 主程序开发 |
| PyQt6 | GUI 界面 |
| Go 1.21+ | 插件服务器 |
| SQLite + AES | 数据存储 |
| Ollama | 本地 AI 模型 |
| Loguru | 日志管理 |

---

## 🤝 参与贡献

我们欢迎任何形式的贡献！

1. Fork 本仓库
2. 创建你的分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 创建 Pull Request

### 开发插件

查看 [插件开发文档](docs/plugin_development.md) 了解如何为修仙宠物开发插件。

---

## 📜 许可证

本项目基于 MIT 许可证开源 - 查看 [LICENSE](LICENSE) 文件了解详情。

---

## 🙏 致谢

- 灵感来源：《凡人修仙传》
- UI 设计：PyQt6
- 图标素材：[待补充]

---

<p align="center">
  <b>⭐ 如果本项目对你有帮助，请点个 Star 支持一下！⭐</b>
</p>

<p align="center">
  <img src="img/2.png" width="100" alt="修仙宠物 Logo">
</p>

<p align="center">
  <i>祝各位道友修仙顺利，早日飞升！</i> 🎉
</p>
