# 🎮 TowerClash - Advanced Minecraft PvP Minigame

**Version:** 1.4.0  
**Author:** Dutchwilco  
**Platform:** PaperMC 1.16 - 1.21.4 | Java 21  

---

## 📖 Overview

**TowerClash** is a fast-paced last-player-standing PvP minigame where players spawn on individual pillars and receive random items every few seconds. Use blocks, projectiles, and utilities to eliminate opponents by knocking them into the void!

### 🎯 Core Features

- ⚔️ **Dynamic Item Distribution** - Weighted loot system with configurable drop rates
- 🏗️ **Schematic-Based Arenas** - Create custom arenas with WorldEdit integration
- 🌍 **Void World Instancing** - Multiple simultaneous games without world interference
- 📊 **MySQL/SQLite Statistics** - Track wins, kills, deaths, streaks
- 🎨 **PlaceholderAPI Integration** - Use stats in other plugins
- 🏆 **Vote System** - Players vote for game modifiers (Chaos, Double Items, etc.)
- 👥 **Private Games** - Create custom matches with friends
- 🔧 **Fully Configurable** - Customize combat, timings, loot, and more

---

## 🚀 Quick Start

### Prerequisites

1. **Paper/Spigot 1.16+** (recommended: 1.21.4)
2. **Java 21**
3. **WorldEdit or FastAsyncWorldEdit** (required for arena creation)
4. **Optional:** Vault, PlaceholderAPI

### Installation

1. Download `TowerClash.jar` and place in `plugins/`
2. Install **WorldEdit** or **FastAsyncWorldEdit**
3. Restart server
4. Configure files in `plugins/TowerClash/`
5. Create your first arena (see below)

---

## 🛠️ Arena Setup (Schematic System)

TowerClash uses a **schematic-based instancing system** where arenas are saved as files and pasted into a void world for each game.

### Step 1: Build Your Arena

1. Build your custom arena in any world
2. Place pillars where players will spawn (1x1 or 2x2 columns)
3. Decorate as desired (builds won't be affected by games)

### Step 2: Select & Save

```
/tower wand
```
- You receive a **Golden Axe**
- **Left-click** a block → Set Position 1 (lowest corner)
- **Right-click** a block → Set Position 2 (highest corner)

```
/tower save <arena-name>
```
- Saves your selected region as a schematic
- Stored in: `plugins/TowerClash/schematics/<arena-name>.schem`

### Step 3: Configure Spawns

Edit `plugins/TowerClash/arenas.yml`:

```yaml
arenas:
  skyisland:
    schematic: "skyisland"
    world: "towerclash_world"
    spawns:
      - x: 10, y: 65, z: 10   # Relative coordinates from schematic origin
      - x: -10, y: 65, z: 10
      - x: 10, y: 65, z: -10
      - x: -10, y: 65, z: -10
    min_players: 2
    max_players: 4
    enabled: true
```

**Important:** Spawn coordinates are **relative** to the schematic paste location!

### Step 4: Test

```
/tower join skyisland
```

---

## 🎮 Game Flow

### 1. Pre-Game (Configurable: 15s default)
- Players join lobby
- Vote system activates (Chaos Mode, Double Items, etc.)
- Countdown begins

### 2. Game Start
- Arena pastes into void world (`towerclash_world`)
- Players teleport to pillar spawns
- 5-second grace period

### 3. Active Game
- **Every 5 seconds:** Players receive 1 random item from loot table
- Combat begins - knock opponents off pillars
- Fall into void = elimination

### 4. Sudden Death (After 120s)
- World border begins shrinking every 10s
- Forces players closer together

### 5. Game End
- Winner determined
- Stats saved to database
- Arena completely deleted from void world
- Position freed for next game

---

## ⚙️ Configuration Files

### `config.yml` - General Settings
```yaml
database:
  type: "SQLITE"  # or MYSQL
  mysql:
    host: "localhost"
    port: 3306
    database: "towerclash"
    username: "root"
    password: "password"

world:
  void_world_name: "towerclash_world"
  arena_spacing: 256  # Blocks between simultaneous games
  auto_create_void_world: true
```

### `game.yml` - Game Mechanics
```yaml
timers:
  pregame_seconds: 15
  grace_seconds: 5
  item_interval_seconds: 5
  sudden_death_after_seconds: 120
  border_shrink_every_seconds: 10

elimination:
  fall_to_void: true
  lives: 1

combat:
  kb_scalar_melee: 1.0
  kb_scalar_projectile: 1.15
  fall_damage: true
  void_kill_y: 0

limits:
  max_ender_pearls_per_60s: 1
  tnt_terrain_damage: false
```

### `loot.yml` - Item Distribution
```yaml
tables:
  default:
    interval_seconds: 5
    rolls_per_tick: 1
    entries:
      - id: "block_stone_8"
        material: STONE
        amount: 8
        weight: 90
        
      - id: "snowball_3"
        material: SNOWBALL
        amount: 3
        weight: 65
        
      - id: "ender_pearl"
        material: ENDER_PEARL
        amount: 1
        weight: 4
        
      - id: "tnt"
        material: TNT
        amount: 1
        weight: 6

    modifiers:
      chaos_mode:
        weight_mult: 1.2
        allow_tnt: true
        allow_lava: true
```

### `arenas.yml` - Arena Definitions
```yaml
arenas:
  classic_arena:
    schematic: "classic_arena"
    world: "towerclash_world"
    spawns:
      - x: 0, y: 65, z: 20
      - x: 0, y: 65, z: -20
      - x: 20, y: 65, z: 0
      - x: -20, y: 65, z: 0
    min_players: 2
    max_players: 8
    enabled: true
```

---

## 🎯 Commands & Permissions

### Player Commands

| Command | Description | Permission |
|---------|-------------|------------|
| `/tower join [arena]` | Join a game (or quick-join) | `tower.play` |
| `/tower leave` | Leave current game | `tower.play` |
| `/tower stats [player]` | View statistics | `tower.play` |
| `/tower private create` | Create private game | `tower.private` |
| `/tower private invite <player>` | Invite to private game | `tower.private` |
| `/tower private start` | Start private game | `tower.private` |

### Admin Commands

| Command | Description | Permission |
|---------|-------------|------------|
| `/tower wand` | Get selection tool | `tower.admin.wand` |
| `/tower save <name>` | Save selected region | `tower.admin.save` |
| `/tower reload` | Reload configs | `tower.admin.reload` |
| `/tower start <arena>` | Force-start game | `tower.admin.start` |
| `/tower stop <arena>` | Force-stop game | `tower.admin.stop` |
| `/tower loot test <n>` | Simulate loot drops | `tower.admin.loot` |

---

## 📊 Statistics & Placeholders

### Database Tracking

- **Wins** - Total victories
- **Games Played** - Total matches
- **Kills** - Total eliminations
- **Deaths** - Times eliminated
- **Current Streak** - Consecutive wins
- **Best Streak** - Highest win streak

### PlaceholderAPI

```
%towerclash_wins%
%towerclash_kills%
%towerclash_deaths%
%towerclash_kd%
%towerclash_games%
%towerclash_winrate%
%towerclash_streak%
%towerclash_best_streak%
```

---

## 🔧 Advanced Features

### Vote System

Players vote before game starts:
- **Chaos Mode** - More TNT, lava, dangerous items
- **Double Items** - Receive 2 items per interval
- **Low Gravity** - Reduced fall speed
- **Short Pillars** - 50% pillar height

### Private Games

Create custom matches:
1. `/tower private create`
2. Receive private game control panel
3. Invite friends: `/tower private invite <player>`
4. Adjust settings (arena, modifiers)
5. Start when ready

### Worldborder Integration

- Individual worldborder for each player during pre-game
- Global shrinking border during Sudden Death
- Configurable shrink rate and timing

---

## 🏗️ Technical Details

### Arena Instancing System

TowerClash uses **schematic-based void world instancing**:

1. **On Game Start:**
   - Schematic pastes at calculated position (256 blocks apart)
   - Players teleport to relative spawn coordinates
   - All blocks tracked for cleanup

2. **During Game:**
   - Player-placed blocks tracked separately
   - Original schematic blocks protected from certain interactions

3. **On Game End:**
   - **Entire arena region deleted** (including player-placed blocks)
   - Position marked as available for next game
   - No reset needed - fresh paste every time

### Performance

- **Async database writes** - Non-blocking stat updates
- **Efficient schematic pasting** - WorldEdit API optimization
- **Memory management** - Arenas fully deleted after use
- **Concurrent games** - Multiple arenas run independently

### Dependencies

**Required:**
- WorldEdit or FastAsyncWorldEdit

**Optional (Soft Dependencies):**
- Vault - Economy rewards
- PlaceholderAPI - Stat placeholders

---

## 🐛 Troubleshooting

### "WorldEdit/FAWE not found!"
- Ensure WorldEdit or FastAsyncWorldEdit is installed
- Check server log for plugin load errors

### Arena doesn't paste
- Verify schematic file exists in `schematics/` folder
- Check `arenas.yml` schematic name matches file name
- Ensure void world has generated

### Players fall through arena
- Spawns may be set too low
- Check spawn Y coordinates in `arenas.yml`
- Ensure spawns are within schematic bounds

### Items not dropping
- Verify `loot.yml` is configured correctly
- Check `game.yml` → `item_interval_seconds`
- Test with `/tower loot test 100`

---

## 📝 Migration Guide (Old System → Schematic System)

If you're upgrading from the old pillar-based system:

### What Changed?

❌ **Old:** Arenas existed in main world, pillars generated dynamically  
✅ **New:** Arenas saved as schematics, pasted into void world per-game

### Migration Steps

1. **Backup** your old `arenas.yml`
2. Rebuild arenas in creative world
3. Use `/tower wand` and `/tower save` to save schematics
4. Configure spawns in new `arenas.yml` format
5. Test thoroughly before enabling

**Note:** Old and new systems are **incompatible** - choose one approach.

---

## 📦 Support & Contributing

**Author:** Dutchwilco  
**Version:** 1.4.0  

For bug reports, feature requests, or contributions, contact me on discord.

---

## 📜 License

Proprietary - All rights reserved by Dutchwilco

---

## 🎉 Credits

Developed with ❤️ by Dutchwilco
