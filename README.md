# 🏰 TowerClash

**A fast-paced PvP/Survival minigame for Minecraft where players start on isolated pillars and receive random items to eliminate opponents as the world border shrinks.**

[![Java Version](https://img.shields.io/badge/Java-17%2B-orange.svg)](https://www.oracle.com/java/)
[![Minecraft Version](https://img.shields.io/badge/Minecraft-1.13--1.21.8-brightgreen.svg)](https://papermc.io/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Installation](#installation)
- [Configuration](#configuration)
- [Commands & Permissions](#commands--permissions)
- [Game Mechanics](#game-mechanics)
- [Arena Setup](#arena-setup)
- [API & Placeholders](#api--placeholders)
- [Dependencies](#dependencies)
- [Building](#building)
- [Support](#support)

## 🎮 Overview

TowerClash is a unique Minecraft minigame that combines PvP combat with strategic resource management. Players spawn on individual pillars high above the void and must use randomly distributed items to eliminate their opponents while surviving increasingly dangerous conditions.

### 🎯 Game Objective
Be the last player standing! Use blocks, projectiles, utility items, and mob spawners strategically to knock opponents off their pillars or eliminate them through combat.

### 🔄 Game Flow
1. **Pre-game**: Players join the lobby and wait for the game to start
2. **Grace Period**: Brief period where PvP is disabled
3. **Active Phase**: Players receive random items every few seconds via the loot system
4. **Sudden Death**: World border begins shrinking, forcing players closer together
5. **Victory**: Last surviving player(s) win and receive rewards

## ✨ Features

- **🏗️ Dynamic Arena System**: Customizable pillar layouts with circle/grid patterns
- **📦 Advanced Loot System**: Weighted item distribution with configurable tables
- **🎮 Multiple Game Modes**: Classic, Chaos, Casual, and variant modes
- **👥 Flexible Team Sizes**: Solo, duos, or custom team configurations
- **🔒 Private Games**: Host private matches with full control
- **📊 Statistics Tracking**: Comprehensive player stats and leaderboards
- **💰 Economy Integration**: Vault support for rewards and transactions
- **🏷️ PlaceholderAPI**: Full placeholder support for external plugins
- **🌍 Multi-Arena Support**: Run multiple concurrent games
- **⚡ High Performance**: Optimized for minimal server impact

## 🚀 Installation

1. **Download** the latest release from the releases page
2. **Place** the TowerClash.jar file in your server's `plugins` folder
3. **Restart** your server
4. **Configure** the plugin using the generated configuration files
5. **Set up** your first arena using the setup commands

### Requirements
- **Minecraft Server**: PaperMC 1.13+ (recommended: 1.21+)
- **Java Version**: Java 17 or higher
- **Optional Dependencies**: Vault, PlaceholderAPI, WorldEdit, ProtocolLib

## ⚙️ Configuration

TowerClash uses multiple configuration files for maximum customization:

### Main Configuration (`config.yml`)
# Database settings (SQLite or MySQL)
database:
  type: "sqlite"
  
# UI Settings
ui:
  scoreboard:
    enabled: true
  actionbar:
    enabled: true
  bossbar:
    enabled: true

# Economy rewards (requires Vault)
rewards:
  win: 100.0
  kill: 10.0
  participation: 5.0
### Game Settings (`game.yml`)
# Player limits and timing
game:
  min_players: 2
  max_players: 16
  team_size: 1

timers:
  pregame_seconds: 15
  grace_seconds: 5
  item_interval_seconds: 5
  sudden_death_after_seconds: 120
  border_shrink_every_seconds: 10
### Loot Tables (`loot.yml`)
Customize item distribution with weighted entries:
tables:
  default:
    entries:
      - material: STONE
        amount: 8
        weight: 90
      - material: ENDER_PEARL
        amount: 1
        weight: 4
### Arena Configuration (`arenas.yml`)
Define arena layouts and pillar patterns:
arenas:
  classic_01:
    world: "towerclash_world"
    center: [0, 80, 0]
    pillar:
      count: 16
      pattern: "CIRCLE"
      radius: 22
      height: 48
      size: "1x1"
## 🎯 Commands & Permissions

### Player Commands
| Command | Description | Permission |
|---------|-------------|------------|
| `/tower join [arena]` | Join queue or specific arena | `tower.play` |
| `/tower leave` | Leave current game or queue | `tower.play` |
| `/tower stats [player]` | View player statistics | `tower.play` |
| `/tower private create <arena>` | Create private game | `tower.private` |

### Admin Commands
| Command | Description | Permission |
|---------|-------------|------------|
| `/tower setup wand` | Get arena setup tool | `tower.admin.setup` |
| `/tower setup create <id>` | Create new arena | `tower.admin.setup` |
| `/tower start <gameId>` | Force start game | `tower.admin.start` |
| `/tower stop <gameId>` | Force stop game | `tower.admin.stop` |
| `/tower reload` | Reload configurations | `tower.admin.reload` |
| `/tower loot test <rolls>` | Test loot table | `tower.admin.loot` |

### Permission Nodes
- `tower.play` - Basic gameplay access (default: true)
- `tower.private` - Create private games (default: op)
- `tower.admin` - Full administrative access (default: op)

## 🎲 Game Mechanics

### Loot System
- **Weighted Distribution**: Items have different spawn chances
- **Timed Delivery**: Items distributed every X seconds (configurable)
- **Category Balance**: Mix of blocks, projectiles, utility, and special items
- **Mode Variants**: Different loot tables for different game modes

### Spawn & Movement System
- **Centered Pillar Spawning**: Players spawn at the exact center of their pillar (X+0.5, Z+0.5) to prevent edge spawning issues
- **Pre-game Freeze**: During countdown, players cannot move horizontally but can jump and look around freely
- **Smooth Game Start**: Movement restriction automatically lifts when the game enters active state
- **No Barriers**: Clean pillar design without barrier blocks that could cause swimming animation bugs

### Combat System
combat:
  kb_scalar_melee: 1.0
  kb_scalar_projectile: 1.15
  fall_damage: true
  void_kill_y: 0

limits:
  max_ender_pearls_per_60s: 1
  tnt_terrain_damage: false
### Game Variants
- **Classic**: Standard gameplay with balanced loot
- **Chaos Mode**: Increased TNT and dangerous items
- **Casual Mode**: Safer items, no explosives
- **Double Items**: Receive twice as many items
- **Low Gravity**: Reduced fall damage and modified physics

## 🏗️ Arena Setup

### Quick Setup
1. Use `/tower setup wand` to get the setup tool
2. Select the arena area with the wand
3. Run `/tower setup create <arenaId>` to create the arena
4. Use `/tower setup maxplayers <amount>` to add pillars
5. Use `/tower setup spawn <1,2,3...>` to add pillar locations
6. Run `/tower setup save` to save the configuration

### Manual Configuration
Edit `arenas.yml` directly for advanced setups:
arenas:
  my_arena:
    world: "world"
    center: [0, 100, 0]
    pillar:
      count: 12
      pattern: "CIRCLE"  # CIRCLE or GRID
      radius: 20
      height: 40
      size: "2x2"       # 1x1 or 2x2
      top_block: STONE
    void_y: 0
    border_size: 100
## 📊 API & Placeholders

### PlaceholderAPI Support
TowerClash provides extensive placeholder support:

| Placeholder | Description |
|-------------|-------------|
| `%towerclash_wins%` | Player's total wins |
| `%towerclash_games%` | Player's total games |
| `%towerclash_kills%` | Player's total kills |
| `%towerclash_deaths%` | Player's total deaths |
| `%towerclash_winrate%` | Player's win percentage |
| `%towerclash_kd%` | Player's kill/death ratio |
| `%towerclash_streak%` | Player's current win streak |
| `%towerclash_best_streak%` | Player's best win streak |
| `%towerclash_in_game%` | Whether player is in a game |
| `%towerclash_arena%` | Current arena name |

### Developer API
// Get the main plugin instance
TowerClash plugin = TowerClash.getInstance();

// Access managers
GameManager gameManager = plugin.getGameManager();
StatsManager statsManager = plugin.getStatsManager();
ArenaManager arenaManager = plugin.getArenaManager();

// Check if player is in game
boolean inGame = gameManager.isPlayerInGame(player);

// Get player statistics
PlayerStats stats = statsManager.getPlayerStats(player.getUniqueId());
## 🔌 Dependencies

### Required
- **Java 17+**
- **PaperMC 1.13-1.21.8**

### Optional (Soft Dependencies)
- **[Vault](https://github.com/MilkBowl/Vault)** - Economy integration and rewards
- **[PlaceholderAPI](https://github.com/PlaceholderAPI/PlaceholderAPI)** - Placeholder support
- **[WorldEdit](https://github.com/EngineHub/WorldEdit)** - Enhanced arena setup tools
- **[ProtocolLib](https://github.com/dmulloy2/ProtocolLib)** - Advanced packet manipulation

## 🔨 Building

### Prerequisites
- Java 17 JDK
- Maven 3.6+
- Git

### Build Steps
# Clone the repository
git clone https://github.com/yourusername/TowerClash.git
cd TowerClash

# Compile and package
mvn clean package

# The compiled JAR will be in target/TowerClash-1.0.0.jar
### Development Setup
# Install to local Maven repository
mvn clean install

# Run tests
mvn test

# Generate documentation
mvn javadoc:javadoc
## 📈 Performance

TowerClash is designed for optimal performance:
- **Async Operations**: Database operations and heavy computations run asynchronously
- **Efficient Packet Handling**: Batched updates for UI elements
- **Memory Management**: Automatic cleanup of finished games
- **Configurable Limits**: Maximum concurrent games and player limits
- **Optimized Algorithms**: Efficient loot distribution and game state management

## 🐛 Troubleshooting

### Common Issues

**Q: Players can't join games**
A: Check permissions (`tower.play`) and ensure arenas are properly configured.

**Q: Items aren't being distributed**
A: Verify loot table configuration in `loot.yml` and check console for errors.

**Q: Players spawn beside pillars instead of on top**
A: This has been fixed in recent versions. Players now spawn centered at X+0.5, Y+1.0, Z+0.5 relative to the pillar block.

**Q: Players can move during countdown**
A: The movement freeze system prevents horizontal movement during pre-game. Ensure you're running the latest version.

**Q: Swimming animation bug on pillars**
A: The barrier system has been removed. Update to the latest version to fix this issue.

**Q: Database errors**
A: Ensure proper database configuration in `config.yml` and check file permissions.

**Q: Performance issues**
A: Reduce `max_concurrent_games` in config and optimize arena sizes.

### Debug Mode
Enable debug logging in your server configuration:
# In your server's logging configuration
loggers:
  nl.dutchcoding.towerclash:
    level: DEBUG
## 🤝 Support

- **Discord**: [Join our Discord](https://discord.gg/your-invite)
- **Issues**: [GitHub Issues](https://github.com/yourusername/TowerClash/issues)
- **Wiki**: [GitHub Wiki](https://github.com/yourusername/TowerClash/wiki)
- **Email**: support@dutchcoding.nl

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Credits

- **Developer**: [Dutchwilco](https://github.com/dutchwilco)
- **Organization**: [DutchCoding](https://dutchcoding.nl)

---
