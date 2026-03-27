# 🎮 Xiuxian Pet (Cultivation Desktop Pet)

[中文](/README.md) | **English**

<p align="center">
  <img src="../img/2.png" width="200" alt="Xiuxian Pet Logo">
</p>

<p align="center">
  <b>A desktop pet that cultivates to immortality —陪你摸鱼、修炼、飞升！</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10+-blue.svg" alt="Python 3.10+">
  <img src="https://img.shields.io/badge/PyQt6-GUI-green.svg" alt="PyQt6">
  <img src="https://img.shields.io/badge/Go-Plugin-orange.svg" alt="Go Plugin">
  <img src="https://img.shields.io/badge/License-GPLv3%20Non--Commercial-red.svg" alt="GPLv3 Non-Commercial">
  <img src="https://img.shields.io/badge/Version-1.7.1-brightgreen.svg" alt="v1.7.1">
</p>

---

## ✨ About

**Xiuxian Pet** is a casual, humor-styled desktop pet game inspired by *A Record of a Mortal's Journey to Immortality* (凡人修仙传). A cute little pet lives on your desktop — it cultivates on its own, breaks through realms, equips gear, and even chats with you!

> 🎯 **Core Idea**: Tired of work? Check how far your pet has cultivated! It might have already ascended to immortality while you're still debugging~ 😏

---

## 🖼️ Screenshots

<p align="center">
  <img src="../img/1.png" width="400" alt="Game Interface">
  <img src="../img/3.png" width="400" alt="Battle Scene">
</p>

---

## 🚀 Core Features

### 🎭 Desktop Pet System

- **Auto Cultivation**: Pet meditates and cultivates on its own; Wisdom attribute affects cultivation speed
- **Manual Cultivation**: Click to cultivate and gain experience; Insight attribute affects effectiveness
- **Realm Breakthrough**: Qi Condensation (13 layers) → Foundation Establishment → Core Formation → Nascent Soul → Spirit Severing → Void Refinement → Body Integration → Mahayana → True Immortal → Golden Immortal → Taiyi → Daluo → Dao Ancestor — the complete path to immortality
- **Sub-Realms**: Each major realm is divided into Early / Mid / Late / Perfection stages (Qi Condensation has 13 layers)
- **Six Attributes**: Attack, Health, Defense, Wisdom, Insight, Spiritual Power
- **Normal Distribution Growth**: Attribute growth uses Gaussian random factors — every level-up is unique
- **Effort Bonus**: The more you click to cultivate, the more bonus attribute growth you get
- **Realm Suppression**: Larger realm gaps lead to exponential combat power differences (10^n multiplier)
- **Skin System**: Supports GIF animated skins — make your pet stand out
- **Click-to-Accelerate Animation**: The faster you click, the faster the GIF animates; gradually slows down when you stop
- **Floating Bubble Text**: "+X Cultivation" and other tips float up as animated bubbles
- **System Tray Tooltip**: Hover over the tray icon to view character info
- **Realm Hiding**: Spend Spirit Stones to hide your true cultivation level from other players

### 🛡️ Equipment System

- **Multiple Slots**: Weapons, armor, magic treasures, and more
- **Equip / Unequip**: Free to mix and match; multiple treasures can be equipped simultaneously
- **Attribute Bonuses**: self_buff (self-enhancement) + enemy_buff (enemy debuff)
- **Spiritual Power**: Treasures consume spiritual power; your spiritual power attribute limits how many you can equip
- **Equipment Decomposition**: Break down unwanted gear to obtain Spirit Stones and Geng Gold
- **Equipment Locking**: Lock important gear to prevent accidental decomposition
- **Inventory Management**: 200 slots, expandable (+50 slots/upgrade, cost doubles each time)
- **Equipment Drops**: Chance to drop gear from battles and secret realms
- **Cultivation Requirements**: Higher-tier gear requires sufficient cultivation points
- **Equipment Enhancement**: True Form / Divine Seal two-tier enhancement with decaying success rates; failure refunds half the materials
- **Six Equipment Slots**: Weapon, Armor, Leggings, Helmet, Boots, Treasure — mix and match freely

### 🗡️ Monster Hunting System

- **7 Difficulty Levels**: Easy → Beginner → Low → Mid → High → Expert → Hell
- **Corrupted / Divine Modifiers**: Can stack on any difficulty level for significantly boosted experience rewards
- **Auto Hunting**: Set-and-forget automatic combat mode
- **Random PVP Match**: May encounter other players while hunting

### ⚔️ Combat System

- **Turn-Based Combat**: Complete turn-based battle flow
- **First Strike**: Higher realm attacks first; same realm is probability-based
- **Critical Hits**: Wisdom attribute affects crit rate
- **Dodge**: Insight attribute affects dodge rate
- **Treasure Combat**: Automatically use treasures in battle for temporary buffs
- **Spirit Burn**: When no treasures are available, burn spiritual power for damage boost
- **Realm Multiplier**: Realm differences create exponential combat power gaps
- **PVP**: Battle against other players
- **Combat Power Rating**: Comprehensive rating system based on attributes, realm, and equipment
- **Realm Name Colors**: Different realms display in different colors — identify cultivation level at a glance
- **Narrative Text**: Rich cultivation-style battle descriptions

### 🤖 AI Smart Chat

- **Local LLM**: Powered by Ollama — chat without internet
- **Smart Detection**: Monitors system idle state (CPU / Memory / GPU / Input devices) and auto-starts conversations
- **Multiple Dialects**: Dongbei, Tianjin, Beijing, Sichuan dialects and more styles available
- **Bubble Dialog**: Cute bubble chat box with typewriter animation
- **Clipboard Reading**: Auto-reads clipboard content as conversation context
- **Screen Capture**: Captures screen as conversation input
- **Local Knowledge Base**: Vector DB-based local knowledge base with auto-import of chat history
- **Manual Chat**: Hotkey-triggered manual chat mode
- **Chat History**: Auto-save and compress chat history

### 🗺️ Secret Realm Exploration

- **Random Maze Generation**: DFS-based random maze generation with tunable loop probability
- **Seven Secret Realms**:
  | Realm | Open Days | Min Realm | Difficulty |
  |-------|-----------|-----------|------------|
  | 🩸 Blood Forbidden Land | Mon/Wed/Fri/Sun | Foundation | ⭐ |
  | 🏛️ Void Heaven Palace | Tue/Thu/Sat/Sun | Core Formation | ⭐⭐ |
  | 👹 Falling Demon Valley | Mon/Wed/Sun | Nascent Soul | ⭐⭐⭐ |
  | ⛰️ Kunwu Mountain | Tue/Thu/Fri | Nascent Soul | ⭐⭐⭐⭐ |
  | 💀 Nether River Land | Mon/Wed | Spirit Severing | ⭐⭐⭐⭐⭐ |
  | 🌙 Cold Moon Realm | Tue/Thu | Spirit Severing | ⭐⭐⭐⭐⭐ |
  | 🎆 New Year Plaza | Fri/Sun | Void Refinement | ⭐⭐⭐ |
- **WASD Movement**: Smooth keyboard movement
- **Auto Pathfinding**: A* algorithm auto-navigates to the exit
- **Auto Combat**: Automatically fight during pathfinding
- **Four Map Elements**: Humans (blue), creatures (red), resources (purple), bonuses (green)
- **Six Strength Levels**: Stronger enemies near the spawn point
- **Fog of War**: Only explored areas within vision range are visible
- **Zoom**: Mouse wheel to zoom the map
- **Health System**: Spend Spirit Stones to revive if defeated
- **Clear Rewards**: Spirit Stones, cultivation points, attribute bonuses
- **Daily Limits**: Each secret realm has a daily entry limit
- **FPS Display**: Real-time frame rate monitoring
- **Time Tracking**: Record clear times and beat personal bests

### 💰 Economy System

- **Three Currencies**: Spirit Stones, Geng Gold, Immortal Jade
- **Spirit Stones**: From cultivation, battles, secret realms, equipment decomposition
- **Geng Gold**: From equipment decomposition
- **Immortal Jade**: From cultivation

### 🔌 Plugin System (Highlight Feature)

> 🌟 **This is our proudest feature!**

We designed a **cross-language plugin architecture** that lets third-party developers create plugins in any language (Python, Go, C++, etc.)!

#### Plugin Features

- 🛡️ **Process Isolation**: Plugins run in separate processes — crashes don't affect the main app
- 🔐 **Encrypted Communication**: AES-encrypted communication between Python and Go
- 📡 **Standardized Protocol**: Unified JSON message format, easy to develop
- 🔄 **Auto Restart**: Plugins auto-restart after crash for high availability
- 📦 **Creative Workshop**: Built-in plugin marketplace, one-click install
- 💓 **Heartbeat Monitoring**: Real-time plugin online status monitoring
- 🔑 **MD5 Verification**: Plugin file integrity verification

#### Communication Protocol

Plugins communicate with the main program via a Go bridge server (`xiuxian_ps.exe`):

```
Plugin Process  ──TCP(plaintext)──▶  Go Bridge :15433  ──AES-encrypted TCP──▶  Python Main
```

- Message format: `[4-byte big-endian length][JSON body]`
- Message structure: `type`, `plugin_id`, `msg_uuid`, `timestamp`, `data`
- Heartbeat: every 30 seconds; auto-unregister after 5 minutes of inactivity

#### Plugin API

Call game methods via `exec_method`. All responses follow the format `{"status":1/0, "msg":"...", "data":...}`:

| Command | Parameters | Description |
|---------|-----------|-------------|
| `get_player_attr` | `(attr_name, default)` | Get player attribute; `"root"` returns all info |
| `set_player_attr` | `(attr_name, value)` | Set player attribute |
| `get_plugin_attr` | `(attr_name, default)` | Get plugin-private data (namespaced) |
| `set_plugin_attr` | `(attr_name, value)` | Store plugin private data |
| `sys_exit` | `(exit_code, verify_code)` | Safe exit (verify code must be `get_version()`) |
| `send_player_peer_info` | None | Get player P2P node info |
| `recv_player_peer_info` | `(peer_data)` | Receive remote player P2P data |

#### How Easy Is It to Create a Plugin?

```go
package main

import (
    "anti-cheat/acore"
    "anti-cheat/atypes"
    "os"
)

func main() {
    // Initialize plugin
    plugin := acore.NewAntiCPlugin(
        "my_plugin",    // unique plugin ID
        "My Plugin",    // plugin name
        "1.0.0",        // version
        "windows",      // platform
        "1.7.1",        // client version
        15,             // detection interval (seconds)
    )
    // Connect to bridge service TCP 127.0.0.1:15433
    if !plugin.Connect("127.0.0.1", 15433) { os.Exit(1) }
    // Register plugin
    if !plugin.Register() { plugin.Stop(); os.Exit(1) }
    // Start heartbeat (every 30s)
    plugin.StartHeartbeat()

    // Call game API example
    plugin.SendMessage(atypes.Message{
        Type: "exec_method",
        Data: map[string]interface{}{
            "method_name": "get_player_attr",
            "args":        []interface{}{"level", 0},
        },
    })
    // Your plugin logic...
    select {}
}
```

### 🛡️ Security

- **Anti-Reverse Engineering**: Detects IDA, x64dbg, OllyDbg, Ghidra, WinDbg, Cheat Engine, etc. (regex matching, resistant to renaming)
- **DLL Injection Detection**: Real-time monitoring of abnormal module loading
- **Remote Thread Injection Detection**: Checks if thread start addresses are within module ranges
- **Single Instance Lock**: PID file lock prevents multi-instance abuse
- **Double AES Encryption**: Fernet + AES-CBC double-layer encryption for game data
- **SQLite Storage**: Transactions guarantee data atomicity

### 🐾 P2P Pet System

- **LAN Discovery**: UDP port 15440 auto-broadcast to discover other pets on the local network
- **Pet Sync**: Real-time sync of position, skin, realm, attributes, and more
- **HTTP Data Pull**: Fetch complete encrypted data via HTTP port 15441 (AES-256, key = player UUID)
- **Message Forwarding**: Change events forwarded to remote players via the plugin bridge architecture
- **Real-Time Push**: Position moves, skin changes, chat messages, etc. are broadcast in real-time

### 🌐 WireGuard Tunnel

Builds a **virtual LAN** using WireGuard, allowing remote players to connect as if they were on the same local network for cross-region multiplayer.

#### How It Works

```
Player A (Home)              WireGuard Server            Player B (Home)
┌──────────────┐            ┌──────────────┐            ┌──────────────┐
│ Physical NIC │            │  Hub Mode    │            │ Physical NIC │
│ 192.168.1.x  │  WireGuard │              │  WireGuard │ 192.168.2.x  │
│              │◀════════▶│ 123.56.x.x    │◀════════▶│              │
│ Virtual NIC  │  UDP:10220 │              │  UDP:10220 │ Virtual NIC  │
│ 10.0.0.5     │            │              │            │ 10.0.0.6     │
│              │            └──────────────┘            │              │
│ P2P UDP:15440│◀════  10.0.0.0/24 Virtual Subnet ═══▶│ P2P UDP:15440│
│ HTTP:15441   │◀═══════════════════════════▶         │ HTTP:15441   │
└──────────────┘           Virtual LAN                  └──────────────┘
```

#### Multiplayer Setup

1. **Install Service**: First-time setup requires admin privileges to install the WireGuard system service (`WireGuardManager`)
2. **Connect**: Auto-generates key pair → Server DHCP assigns tunnel IP (`10.0.0.x`) → Registers public key → Establishes encrypted tunnel
3. **Auto Discovery**: P2P pet plugin's UDP broadcasts automatically reach all players within the tunnel via the virtual NIC
4. **Real-Time Sync**: Skin, position, realm, and other data are transmitted via AES-encrypted HTTP

#### Tunnel Features

- **Embedded Binaries**: WireGuard core and CLI tools are bundled — no separate installation needed
- **Auto Key Management**: Key pairs generated locally; server-side registration and assignment
- **Service Monitoring**: Checks tunnel service status every 5 seconds; verifies connectivity every 15 seconds
- **Traffic Stats**: Real-time display of tunnel transfer data (`wg show`)
- **Auto Reconnect**: Full lifecycle management — start / stop / restart / uninstall
- **Cross-Platform**: Supports Windows, Linux, macOS
- **Server-Independent P2P**: Real-time P2P interactions bypass the central server; only tunnel registration depends on the server

### 🏯 Faction System

- **Rank Hierarchy**: Sect Master, Elder, Core Disciple, Inner Disciple, Outer Disciple — each with unique permissions
- **Sect Barrier**: Collective defense buff for faction members
- **Cultivation Boost**: Contribute Spirit Stones for accelerated cultivation speed

### 🏪 Auction House

- **List / Delist / Reprice**: Flexible item trading management
- **Advanced Search**: Filter by realm, enhancement level, buff type
- **Auto Appraisal**: Smart equipment market value estimation

### 🏆 Leaderboards

- **Combat Power**: Comprehensive power ranking
- **Equipment**: Strongest gear showcase
- **Secret Realm Records**: Clear speed rankings
- **Anti-Cheat Board**: Public list of detected cheaters

### 📬 Mail & Announcements

- **Mail System**: Send and receive system and personal messages
- **Announcement Board**: View latest server announcements

### 🌐 Built-in Browser

- **Multi-Tab**: Browse multiple pages simultaneously
- **Custom UA / Headers / Proxy**: Flexible request configuration
- **DevTools**: Built-in debugging tools
- **Cookie Persistence**: Auto-keep login sessions

### ☁️ Cloud Save

- **Upload / Download**: Sync game data to the server
- **Key Recovery**: Restore encryption keys via server backup
- **Skin Community Sharing**: Upload and download community-shared pet skins

### ⚙️ System Features

- **Always on Top**: Pet window always stays on top
- **Theme Switching**: Multiple UI themes supported
- **Cross-Platform Auto-Start**: Windows (registry / startup folder), Linux (.desktop), macOS (launchd)
- **Global Exception Handler**: Auto-logs crash errors
- **Auto-Save**: Game data auto-saved every 10 seconds to prevent data loss
- **Data Backup**: Backup support for game data and key files
- **Log Management**: Loguru logging, 50MB auto-rotation, max 10 files retained
- **Server Connection**: Custom server address support — leaderboard, factions, auction house, world chat
- **Client Auto-Update**: Built-in update detection and download with progress bar and speed display
- **CPS Anti-Macro**: Max 10 clicks per second to prevent auto-clicker scripts
- **Social Features**: Faction system, auction house, world chat, leaderboards

---

## 📦 Installation & Running

### Requirements

- Windows 10/11
- Python 3.10+

### Run the Game

```bash
python client/main.py
```

Or run directly:

```bash
xiuxian_xxx.exe
```

### Command Line Args

```bash
# Specify server address
python client/main.py -s 127.0.0.1:15432

# Disable anti-debug mode
python client/main.py -ad
```

---

## 🎨 Tech Stack

| Technology | Purpose |
|------------|---------|
| Python 3.10+ | Main application |
| PyQt6 | GUI framework |
| Go 1.21+ | Plugin server |
| SQLite + AES | Data storage |
| Ollama | Local AI model |
| WireGuard | P2P tunneling |
| Loguru | Logging |
| pynvml | GPU detection |
| pynput | Input device monitoring |
| Pillow | Screen capture |
| pyperclip | Clipboard reading |

---

## 🤝 Contributing

Contributions of all forms are welcome!

1. Fork this repository
2. Create your branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Create a Pull Request

### Plugin Development

Check out the [Plugin Development Guide](docs/plugin_development.md) to learn how to create plugins for Xiuxian Pet.

---

## 📜 License

This project is released under **GNU GPLv3** with an additional **non-commercial use** clause:

- ✅ Allowed: Personal learning, research, non-profit educational use
- ❌ Prohibited: Commercial sales, paid licensing, for-profit use

See the [LICENSE](LICENSE) file for full terms.

---

## ⚠️ Disclaimer

### 1. Tunnel / NAT Traversal Features

**The tunnel features provided by this software are for learning and research purposes only. Users must comply with:**

- **Legal Use**: Users must ensure usage complies with all applicable local laws and regulations (including but not limited to cybersecurity, data protection, and personal information laws)
- **No Illegal Use**: Must not be used to bypass network restrictions, access illegal content, conduct network attacks, steal data, or any illegal activities
- **Network Security**: Users bear all risks associated with using tunnel features, including but not limited to unauthorized access and data breaches
- **Privacy**: This software does not log user communications. P2P data transmitted through the tunnel is end-to-end encrypted by the WireGuard protocol and cannot be decrypted by the server. Users must protect sensitive data and their own tunnel keys
- **Key Management**: Users are responsible for safeguarding their tunnel keys; security issues arising from key leakage are the user's sole responsibility

### 2. Online Multiplayer

**The multiplayer feature is based on WireGuard virtual LAN technology, designed for private group play among trusted users. By using this feature, you acknowledge and agree to the following:**

- **Trust Requirement**: Multiplayer requires explicit trust between all participants. Users should only establish tunnel connections with known, trusted friends or authorized users
- **Data Security**: P2P data transmitted within the tunnel (including player info, skins, positions, etc.) is AES-256 encrypted, but there remains a technical possibility of interception by other members within the same tunnel
- **User Responsibility**: Any disputes, data losses, privacy breaches, or other issues arising from multiplayer interactions shall be resolved among participants. Developers assume no liability whatsoever
- **No Abuse**: Must not use multiplayer features for harassment, fraud, distribution of illegal content, or any other harmful behavior
- **Minor Protection**: Minor users should use multiplayer features under guardian supervision. Guardians bear supervisory responsibility for minors' online activities
- **Content Compliance**: Any content shared via multiplayer features (including custom skins, chat messages, etc.) must comply with applicable laws and regulations

### 3. Software Nature

- This software is free/open-source and provided "as is" without any warranty
- Developers are not liable for any direct or indirect losses from using this software
- Including but not limited to: data loss, system damage, network issues, legal disputes

### 4. Updates & Maintenance

- Developers reserve the right to modify, suspend, or terminate the service at any time
- No guarantee of permanent availability or continuous updates
- Users should update to the latest version promptly for security fixes

### 5. Final Interpretation

The developers reserve the right of final interpretation of this disclaimer. Use of this software constitutes acceptance of the above terms.

---

**If you do not agree with the above terms, please stop using this software immediately.**

---

## 🙏 Acknowledgements

- Inspiration: *A Record of a Mortal's Journey to Immortality* (凡人修仙传)
- UI Framework: PyQt6
- AI Engine: Ollama

---

<p align="center">
  <b>⭐ If you find this project helpful, please give it a Star! ⭐</b>
</p>


<h3 align="center">💬 Join the Community</h3>
<p align="center">
  <img src="../img/4.jpg" width="200" alt="QQ Group">
</p>

---

<p align="center">
  <img src="../img/2.png" width="100" alt="Xiuxian Pet Logo">
</p>

<p align="center">
  <i>May all cultivators achieve immortality! 🎉</i>
</p>
