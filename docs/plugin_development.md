# 修仙宠物游戏插件开发完整指南

## 文档说明

本指南为修仙宠物游戏插件开发提供全流程、标准化指导，涵盖系统架构、通信协议、开发流程、代码示例及问题排查。文档结构清晰、内容完整，既适用于开发者快速上手插件开发，也可直接提供给 AI 解析插件工作原理，无核心内容简化。

## 目录

- [1. 插件系统概述](#1-插件系统概述)
- [2. 插件架构与通信流程](#2-插件架构与通信流程)
- [3. 插件开发标准化流程](#3-插件开发标准化流程)
- [4. 消息协议详解](#4-消息协议详解)
- [5. 快速上手示例代码](#5-快速上手示例代码)
- [6. 常见问题解答（FAQ）](#6-常见问题解答faq)

---

## 1. 插件系统概述

### 1.1 系统核心设计

插件系统采用TCP 通信的分布式架构，支持开发者创建独立于主程序的功能扩展模块。插件通过标准化 JSON 消息协议与游戏主程序交互，具备高扩展性、高稳定性，可跨语言（Python/Go/Java 等）开发。

### 1.2 核心组成模块

| 模块名称 | 核心职责 |
|---------|---------|
| PluginCore | 管理插件全生命周期（注册 / 运行 / 注销）、消息路由与分发 |
| PluginManager | 负责插件注册审核、注销处理、在线状态监控、版本兼容性校验 |
| GoBridge | Python 主程序与插件的通信中间层，默认监听端口 15433，处理 TCP 消息转发 |
| MessageHandlers | 解析不同类型的 JSON 消息，映射到游戏内置函数，返回处理结果给插件 |

---

## 2. 插件架构与通信流程

### 2.1 架构示意图

```
插件客户端（自定义实现）
  ↓↑
TCP 双向通信
  ↓↑
Go桥接服务（内置核心服务）
  ↓↑
TCP 双向通信
  ↓↑
游戏主程序（Python）
```

### 2.2 核心通信流程

1. 游戏主程序启动后，自动拉起 Go 桥接服务并监听 15433 端口；
2. 插件客户端初始化 TCP 连接，向 Go 桥接服务发起连接请求；
3. 插件发送 `register` 类型消息完成身份验证与注册；
4. 注册成功后，插件与主程序通过 JSON 消息双向通信（如读写玩家属性、调用内置方法）；
5. 插件需每隔固定时间发送 `heartbeat` 心跳包，维持连接活性；
6. 插件退出前需主动发送 `unregister` 注销消息，释放资源；
7. 若连接断开，插件需实现重连逻辑，重新完成注册流程。

---

## 3. 插件开发标准化流程

### 3.1 项目初始化

- 创建支持 TCP 通信的项目工程（推荐 Python/Go，适配 Go 桥接服务）；
- 引入 JSON 序列化 / 反序列化依赖（如 Python 的 `json`、Go 的 `encoding/json`）；
- 配置日志模块，记录连接状态、消息收发、异常信息（需包含 `msg_uuid`、`timestamp` 便于调试）。

### 3.2 基础功能实现

必须实现以下核心能力，保障插件生命周期管理：

- **TCP 连接 / 重连逻辑**：处理连接超时、断开后的自动重连，最大重连次数 / 间隔可配置；
- **核心消息交互**：封装 `register`（注册）、`heartbeat`（心跳）、`unregister`（注销）消息的构造与发送；
- **通用消息封装**：实现消息发送 / 接收的通用方法（TCP 通信无加密，直接传输 JSON 字符串）；
- **异常处理**：捕获连接中断、JSON 解析失败、消息超时等异常，保证插件鲁棒性。

### 3.3 业务功能开发

基于游戏提供的消息协议，实现具体业务逻辑：

- 玩家属性读写（如修改修炼值、境界、背包物品）；
- 调用游戏内置方法（如强制退出、获取插件属性）；
- 自定义业务逻辑（如宠物养成、自动修炼、资源兑换）。

### 3.4 测试与调试

- 使用游戏内置插件调试工具，监控 TCP 消息收发日志；
- 接入 Wireshark 抓包分析 TCP 数据，验证消息格式正确性；
- 模拟断网、高延迟场景，测试重连与消息重试逻辑；
- 验证各内置函数调用结果，确保参数类型 / 格式匹配。

### 3.5 发布与维护

- 声明插件兼容的游戏客户端版本、运行平台；
- 添加日志监控（如关键操作记录、错误堆栈）；
- 适配 Windows/Linux/macOS 多平台；
- 提供插件版本更新说明、配置文档。

---

## 4. 消息协议详解

### 4.1 通用 JSON 消息结构

所有通信消息遵循统一结构，字段不可缺失（`data` 字段随消息类型动态变化）：

```json
{
  "type": "消息类型（如register/heartbeat/get_player_attr）",
  "plugin_id": "插件唯一标识ID（自定义，不可重复）",
  "msg_uuid": "消息全局唯一UUID（防止重复处理）",
  "timestamp": "消息发送时间戳（毫秒级）",
  "data": {}
}
```

### 4.2 核心消息类型及模板

| 消息类型 | 用途 | 完整 JSON 模板 |
|---------|------|---------------|
| register | 插件注册 | `{ "type":"register", "plugin_id":"pet_plugin_001", "msg_uuid":"uuid-123", "timestamp":1735689600000, "data":{ "plugin_name":"宠物自动修炼插件", "plugin_version":"1.0.0", "platform":"windows", "client_version":"2.5.0", "author":"开发者A", "description":"自动完成宠物修炼任务", "release_date":"2025-12-01" } }` |
| unregister | 插件注销 | `{ "type":"unregister", "plugin_id":"pet_plugin_001", "msg_uuid":"uuid-124", "timestamp":1735689700000, "data":{ "reason":"用户主动退出" } }` |
| heartbeat | 维持连接 | `{ "type":"heartbeat", "plugin_id":"pet_plugin_001", "msg_uuid":"uuid-125", "timestamp":1735689800000, "data":{ "timestamp":1735689800000 } }` |
| get_player_attr | 获取玩家属性 | `{ "type":"get_player_attr", "plugin_id":"pet_plugin_001", "msg_uuid":"uuid-126", "timestamp":1735689900000, "data":{ "attr_name":"cultivation", "default_value":0.0 } }` |
| set_player_attr | 设置玩家属性 | `{ "type":"set_player_attr", "plugin_id":"pet_plugin_001", "msg_uuid":"uuid-127", "timestamp":1735690000000, "data":{ "attr_name":"cultivation", "attr_value":1.23e10 } }` |
| exec_method | 执行游戏内置方法 | `{ "type":"exec_method", "plugin_id":"pet_plugin_001", "msg_uuid":"uuid-128", "timestamp":1735690100000, "data":{ "method_name":"sys_exit", "args":[0, "get_version()"] } }` |

### 4.3 内置函数说明

游戏主程序提供以下内置函数，插件通过 `exec_method` 消息调用：

| 函数 Key | 方法名 | 参数类型 | 功能解释 |
|---------|-------|---------|---------|
| null | Null_Func | 无参数 | 空函数，占位用，调用后返回 `{status:0, msg:"null"}` |
| sys_exit | SysExit | (int, str) | 强制退出程序：int 为退出码，str 为校验码，退出时保存数据、日志并调用 `os._exit()` |
| get_player_attr | Get_Player_Attr | (str, any) | 获取玩家属性：参数 1 为属性名（"root" 返回所有信息），参数 2 为默认值 |
| set_player_attr | Set_Player_Attr | (str, any) | 设置玩家属性：参数 1 为属性名，参数 2 为属性值 |
| get_plugin_attr | Get_Plugin_Attr | (str, any) | 获取插件属性：参数 1 为属性名（"root" 返回全部，支持 xxx.xxx 递归指定），参数 2 为默认值 |
| set_plugin_attr | Set_Plugin_Attr | (str, any) | 设置插件属性：参数 1 为属性名（支持 xxx.xxx 递归指定），参数 2 为属性值 |

### 4.4 游戏玩家属性字段

玩家属性为一级层级结构，核心字段如下（均支持通过 `get/set_player_attr` 读写）：

| 字段名 | 类型 | 示例值 | 说明 |
|-------|------|-------|------|
| uuid | str | 123e4567-e89b-12d3-a456-426614174000 | 角色唯一标识符 |
| name | str | 天行者 | 角色名称 |
| attack | float | 123456.78 | 攻击力，决定伤害基础值 |
| health | float | 987654.32 | 生命值 |
| defense | float | 54321.0 | 防御力 |
| wisdom | float | 12345.67 | 智慧值，影响技能 / 修炼效率 |
| insight | float | 6789.01 | 洞察力，影响解谜 / 策略 |
| realm | str | 筑基期 | 境界：炼气期(13)/筑基期(4)/结丹期(4)/元婴期(4)/化神期(4)/炼虚期(4)/合体期(4)/大乘期(4)/真仙境(4)/金仙境(4)/太乙境(4)/大罗境(4)/道祖境(1) |
| realm_level | int | 2 | 境界内等级（练气期最大 13，其他 4，道祖 1） |
| cultivation | float | 1.23e10 | 当前修炼值 |
| cultivation_speed_bonus | float | 1.5 | 修炼加成倍率 |
| total_clicked | int | 45 | 总点击次数（修炼 / 互动） |
| total_cultivation | float | 1.5e10 | 累积修炼总量 |
| payload | dict | `{"weapon":["uuid5"]}` | 装备 / 宝物载荷列表（按类型分类） |
| inventory | dict | `{"uuid5":{"name":"玄铁剑","level":"元婴期"}}` | 背包数据（物品属性 / 等级 / 增益） |
| inventory_capacity | int | 100 | 背包容量上限 |
| stone | float | 50000.0 | 基础资源（修炼 / 购买） |
| gold | float | 1.0e20 | 高级资源 / 货币 |
| faction_name | str | 真天宗 | 所属门派 / 阵营 |
| faction_work | str | 长老 | 门派职务（宗主 / 长老 / 真传弟子/核心弟子/杂役弟子） |
| client_version | str | 1.0.0 | 客户端版本 |
| combat_power | float | 1.0e8 | 战斗力总值 |
| settings | dict | `{"win_style":"red"}` | 客户端设置 |
| mytool | dict | `{"main_dir":"D:/Tools"}` | 工具路径信息 |
| account_key | str | abcdef123456 | 账户唯一密钥 |
| cheater | bool | false | 是否作弊状态 |
| cheater_use_time | float | 0.0 | 作弊累计使用时间 |
| win_auto_start | bool | true | 开机自动启动 |
| xianyu | float | 1.0e10 | 仙玉（货币） |
| hide_realm | dict | `{"enable":false,"realm":"大乘期"}` | 隐藏境界信息及到期时间 |
| cultivation_threshold | float | 1.0e12 | 修炼升级门槛值 |
| srealm_maze | dict | `{"血色禁地":{"usetime":0.0}}` | 仙境迷宫数据 |
| spiritual | float | 5000.0 | 当前灵力值 |
| use_spiritual | float | 100.0 | 已使用灵力 |
| fight_vs_ikun | dict | `{"last_vs_time":0.0,"vs_num":0}` | 人机对战记录 |
| plugins | dict | `{"pet_plugin_001":{}}` | 插件专属数据区域 |

---

## 5. 快速上手示例代码

### 5.1 Python 版插件客户端（生命周期管理）

完整实现 TCP 连接、注册、心跳、注销、业务调用等全生命周期管理：

```python
import socket, json, time, uuid
from threading import Thread

class PluginConnection:
    def __init__(self, host='127.0.0.1', port=15433, plugin_id=None):
        self.host = host
        self.port = port
        self.plugin_id = plugin_id or str(uuid.uuid4())
        self.sock = None
        self.running = False

    def connect(self):
        self.sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.sock.connect((self.host, self.port))
        self.running = True
        Thread(target=self.receive_loop, daemon=True).start()
        self.register_plugin()
        Thread(target=self.heartbeat_loop, daemon=True).start()

    def send(self, msg: dict):
        self.sock.sendall((json.dumps(msg)+'\n').encode('utf-8'))

    def receive_loop(self):
        buffer = ""
        while self.running:
            data = self.sock.recv(4096)
            if not data:
                break
            buffer += data.decode('utf-8')
            while '\n' in buffer:
                line, buffer = buffer.split('\n', 1)
                self.handle_message(json.loads(line))

    def handle_message(self, msg: dict):
        print("Received:", msg)

    def register_plugin(self):
        msg = {
            "type": "register",
            "plugin_id": self.plugin_id,
            "msg_uuid": str(uuid.uuid4()),
            "timestamp": int(time.time()*1000),
            "data": {
                "plugin_name": "示例插件",
                "plugin_version": "1.0",
                "platform": "Windows",
                "client_version": "1.0.0",
                "author": "开发者",
                "description": "示例说明",
                "release_date": "2025-12-05"
            }
        }
        self.send(msg)

    def heartbeat_loop(self):
        while self.running:
            msg = {
                "type": "heartbeat",
                "plugin_id": self.plugin_id,
                "msg_uuid": str(uuid.uuid4()),
                "timestamp": int(time.time()*1000),
                "data": {"timestamp": int(time.time()*1000)}
            }
            self.send(msg)
            time.sleep(5)

    def unregister_plugin(self, reason="正常退出"):
        msg = {
            "type": "unregister",
            "plugin_id": self.plugin_id,
            "msg_uuid": str(uuid.uuid4()),
            "timestamp": int(time.time()*1000),
            "data": {"reason": reason}
        }
        self.send(msg)
        self.running = False
        self.sock.close()
```

### 5.2 Go 版插件客户端（生命周期管理）

完整实现 TCP 连接、注册、心跳、注销等生命周期管理：

```go
package core

import (
    "encoding/binary"
    "encoding/json"
    "fmt"
    "io"
    "log"
    "net"
    "os"
    "strings"
    "time"
    "github.com/google/uuid"
)

// ANSI 颜色代码
const (
    Reset  = "\033[0m"
    Red    = "\033[31m"
    Green  = "\033[32m"
    Yellow = "\033[33m"
    Blue   = "\033[34m"
    Purple = "\033[35m"
    Cyan   = "\033[36m"
)

// 自定义 Writer 实现彩色日志
type ColorWriter struct {
    writer io.Writer
}

func (cw *ColorWriter) Write(p []byte) (n int, err error) {
    logStr := string(p)
    switch {
    case strings.Contains(logStr, "[+]"):
        logStr = Green + logStr + Reset
    case strings.Contains(logStr, "[-]"):
        logStr = Red + logStr + Reset
    case strings.Contains(logStr, "[*]"):
        logStr = Yellow + logStr + Reset
    }
    return cw.writer.Write([]byte(logStr))
}

type PluginConnection struct {
    Host     string
    Port     int
    PluginID string
    Conn     net.Conn
    Running  bool
}

type Message struct {
    Data      interface{} `json:"data"`
    MsgUUID   string      `json:"msg_uuid"`
    PluginID  string      `json:"plugin_id"`
    Timestamp float64     `json:"timestamp"`
    Type      string      `json:"type"`
}

func initLog(plugin_id string) {
    logDir := "./log"
    if _, err := os.Stat(logDir); os.IsNotExist(err) {
        err := os.MkdirAll(logDir, 0755)
        if err != nil {
            fmt.Fprintf(os.Stderr, "创建日志目录失败: %v\n", err)
            return
        }
    }
    logFile, err := os.OpenFile(fmt.Sprintf("./log/%v.log", plugin_id), os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0666)
    if err != nil {
        fmt.Fprintf(os.Stderr, "打开日志文件失败: %v\n", err)
        return
    }
    colorWriter := &ColorWriter{writer: os.Stdout}
    fileAndConsoleWriter := io.MultiWriter(logFile, colorWriter)
    log.SetOutput(fileAndConsoleWriter)
    log.SetPrefix("")
    log.SetFlags(log.Ldate | log.Ltime | log.Lshortfile)
    log.Printf("[+][Init]日志系统初始化完成，插件ID: %s", plugin_id)
}

func NewPluginConnection(host string, port int, plugin_id string) *PluginConnection {
    initLog(plugin_id)
    return &PluginConnection{
        Host:     host,
        Port:     port,
        PluginID: plugin_id,
    }
}

func (p *PluginConnection) Connect() error {
    addr := fmt.Sprintf("%v:%v", p.Host, p.Port)
    conn, err := net.Dial("tcp", addr)
    if err != nil {
        return err
    }
    p.Conn = conn
    p.Running = true
    go p.receiveLoop()
    p.registerPlugin()
    go p.heartbeatLoop()
    return nil
}

func (p *PluginConnection) create_msg(type1 string, data interface{}) *Message {
    message := Message{
        Type:      type1,
        Data:      data,
        PluginID:  p.PluginID,
        MsgUUID:   uuid.New().String(),
        Timestamp: float64(time.Now().UnixNano()) / 1e9,
    }
    return &message
}

func (p *PluginConnection) send(msg *Message) error {
    data, err := json.Marshal(msg)
    if err != nil {
        return err
    }
    lengthBuf := make([]byte, 4)
    binary.BigEndian.PutUint32(lengthBuf, uint32(len(data)))
    _, err = p.Conn.Write(append(lengthBuf, data...))
    return err
}

func (p *PluginConnection) receiveLoop() {
    for p.Running {
        lengthBuf := make([]byte, 4)
        _, err := io.ReadFull(p.Conn, lengthBuf)
        if err != nil {
            if err == io.EOF {
                log.Fatalln("[-][ReceiveLoop]Connection closed by server")
            }
            if strings.Contains(err.Error(), "forcibly closed by the remote host") {
                log.Fatalln("[-][ReceiveLoop]Connection forcibly closed by the remote host")
            }
            log.Printf("[-][ReceiveLoop]Failed to read message length: %v\n", err)
            continue
        }
        msgLength := binary.BigEndian.Uint32(lengthBuf)
        if msgLength > 1024*1024*100 {
            log.Printf("[-][ReceiveLoop]Message too large: %d bytes\n", msgLength)
            break
        }
        msgBuf := make([]byte, msgLength)
        _, err = io.ReadFull(p.Conn, msgBuf)
        if err != nil {
            log.Printf("[-][ReceiveLoop]Failed to read message body: %v\n", err)
            break
        }
        var msg map[string]interface{}
        err = json.Unmarshal(msgBuf, &msg)
        if err != nil {
            log.Printf("[-][ReceiveLoop]Failed to unmarshal JSON: %v\n", err)
            continue
        }
        p.handleMessage(msg)
    }
    p.Running = false
}

func (p *PluginConnection) handleMessage(msg map[string]interface{}) {
    log.Printf("[+][HandleMessage]Rec-msg => %v\n", msg)
}

func (p *PluginConnection) registerPlugin() {
    data := map[string]interface{}{
        "plugin_name":    "ws-to-tcp",
        "plugin_version": "1.0.0",
        "platform":       "Windows",
        "client_version": "1.4.6",
        "author":         "system",
        "description":    "监听ws协议并转发为tcp协议,可以用来将websocket流量转发到插件服务器上",
        "release_date":   "20251206",
    }
    msg := p.create_msg("register", data)
    p.send(msg)
}

func (p *PluginConnection) heartbeatLoop() {
    for p.Running {
        data := map[string]interface{}{"timestamp": float64(time.Now().UnixNano()) / 1e9}
        msg := p.create_msg("heartbeat", data)
        p.send(msg)
        time.Sleep(30 * time.Second)
    }
}

func (p *PluginConnection) unregisterPlugin(reason string) {
    data := map[string]interface{}{"reason": reason}
    msg := p.create_msg("unregister", data)
    p.send(msg)
    p.Running = false
    p.Conn.Close()
}
```

---

## 6. 常见问题解答（FAQ）

| 问题现象 | 解决方案 |
|---------|---------|
| 插件连接失败 | 1. 确认游戏主程序已启动且 Go 桥接服务开启；<br>2. 检查端口（默认 15433）和地址正确性；<br>3. 关闭防火墙 / 安全软件拦截；<br>4. 用 `telnet 127.0.0.1 15433` 验证网络连通性 |
| 注册失败 | 1. 检查 `plugin_id` 是否与已注册插件重复；<br>2. 验证注册消息字段完整性（如 `release_date` 格式为 YYYYMMDD）；<br>3. 确认 JSON 格式合法（无语法错误） |
| 连接不稳定 | 1. 完善心跳机制（间隔≤10 秒）和断线重连逻辑；<br>2. 添加消息超时重试（重试次数 / 间隔可配置）；<br>3. 监控网络延迟，避免高频发送消息 |
| 调试困难 | 1. 使用游戏内置插件调试工具查看通信日志；<br>2. 用 Wireshark 抓包分析 TCP 数据；<br>3. 插件内添加详细日志（含 `msg_uuid`、`timestamp`、消息内容） |
| 功能调用无响应 | 1. 检查 `method_name` 是否与内置函数 Key 完全匹配（如 `"get_player_attr"`）；<br>2. 验证参数类型 / 格式（如 `sys_exit` 的校验码）；<br>3. 确认插件已完成注册且 `isConnected` 为 true |
| 玩家属性读写失败 | 1. 检查 `attr_name` 是否与玩家属性字段完全匹配（如 `"cultivation"` 而非 `"cultivate"`）；<br>2. 确认属性值类型与游戏要求一致（如数值型字段不可传字符串）；<br>3. 调用 `get_player_attr("root")` 查看所有属性，确认字段存在 |

---

## 7. WebSocket 插件说明

针对页面插件（由于只能通过 ws 进行连接），所以开发了 ws-to-tcp 插件，这个插件主要功能是开放 `0.0.0.0:15434` 端口，并且将发送给 ws 的请求转发给 tcp，然后在将 tcp 的返回转发给 ws。

### 插件流程图

```
PY系统 <=> GO桥接 <=> ws2tcp插件 <=> ws插件（你需要实现的）
```

需要注意的是，即便开放 ws 端口，也要遵守插件规范（比如注册、心跳、注销、方法调用、属性读写、系统调用等）。

### WebSocket 示例代码

下面是一个示例（这个示例不包含心跳，需要补上（30s））：

```python
import asyncio
from datetime import datetime
import time
import websockets
import json

async def test_websocket():
    uri = "ws://127.0.0.1:15434/ws"
    
    try:
        async with websockets.connect(uri) as websocket:
            print("连接到WebSocket服务器")
            
            # 发送注册消息
            register_msg = {
                "type": "register",
                "plugin_id": "python-test-plugin-001",
                "msg_uuid": "test_register_msg",
                "timestamp": time.time(),
                "data": {
                    "plugin_name": "Python测试插件",
                    "plugin_version": "1.0.0",
                    "author": "Your Name",
                    "platform": "Windows",
                    "client_version": "1.0.0",
                    "description": "这是一个Python测试插件",
                    "release_date": "2023-07-01"
                }
            }
            await websocket.send(json.dumps(register_msg))
            print("已发送注册消息")
            
            # 等待服务器响应
            response = await websocket.recv()
            print(f"收到服务器响应: {response}")
            
            # 发送查询消息
            query_msg = {
                "type": "get_player_attr",
                "plugin_id": "python-test-plugin-001",
                "msg_uuid": "test_register_msg",
                "timestamp": time.time(),
                "data": {
                    "attr_name": "name",
                    "default_value": ""
                }
            }
            await websocket.send(json.dumps(query_msg))
            print("已发送查询消息")
            
            # 等待服务器响应
            response = await websocket.recv()
            print(f"收到查询响应: {response}")
            
            # 发送注销消息
            unregister_msg = {
                "type": "unregister",
                "plugin_id": "python-test-plugin-001",
                "msg_uuid": "test_register_msg",
                "timestamp": time.time(),
                "data": {
                    "reason": "test_completed"
                }
            }
            await websocket.send(json.dumps(unregister_msg))
            print("已发送注销消息")
            
            # 等待确认
            response = await websocket.recv()
            print(f"收到注销确认: {response}")
            
    except Exception as e:
        print(f"连接错误: {e}")

# 运行测试
asyncio.run(test_websocket())
```
