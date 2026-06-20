# TowerClash

A fast-paced PvP minigame for Paper servers where players spawn on isolated pillars, receive random items every few seconds, and must knock opponents off to survive. Last player standing wins.

## Features

- **Multiple game modes** voted on before each match — Classic, Chaos, Low Gravity, Casual
- **Random loot system** — fully configurable loot tables per game mode
- **Auto arena setup** — pillar spawn points are detected automatically from your build
- **Voting system** — players vote for game mode and starting hearts via a GUI compass
- **Scoreboard & boss bar** — live player count and game timer
- **PlaceholderAPI support** — expose stats to other plugins
- **SQLite / MySQL** — persistent player statistics (wins, kills, deaths, streaks)
- **WorldEdit / FAWE** — used for saving and restoring arena schematics
- **Build height limit** — prevents players from building too high above pillars
- **Particle wall** — visual border warning when approaching the edge

## Requirements

| Requirement | Version |
|---|---|
| Paper / Spigot | 1.21.X |
| Java | 17+ |
| WorldEdit or FAWE | 7.x |

**Optional:** PlaceholderAPI

## Installation

1. Drop `TowerClash.jar` into your `plugins/` folder
2. Install WorldEdit or FastAsyncWorldEdit
3. Start the server — config files are generated in `plugins/TowerClash/`
4. Set up at least one arena (see below)

## Arena Setup

```
/tower wand                         
```
Select the two corners of your arena with left and right click.
```
/tower create <name>                
/tower setminplayers <amount> <name>
/tower save <name>                  
/tower enable <name>                
```

Pillars are detected automatically — they must be at least 10 blocks tall with 2 blocks of air above the top and 5 solid blocks below.

## Commands

| Command | Description | Permission |
|---|---|---|
| `/tower join [arena]` | Join a game | `tower.play` |
| `/tower leave` | Leave current game | `tower.play` |
| `/tower vote` | Open vote menu | `tower.play` |
| `/tower stats [player]` | View player stats | `tower.play` |
| `/tower gui` | Open arena selector | `tower.play` |
| `/tower sethub` | Set hub/lobby location | `tower.admin` |
| `/tower wand` | Get selection wand | `tower.admin` |
| `/tower create <name>` | Create arena | `tower.admin` |
| `/tower setminplayers <amount> <name>` | Set min players | `tower.admin` |
| `/tower save <name>` | Save schematic | `tower.admin` |
| `/tower enable <name>` | Enable arena | `tower.admin` |
| `/tower delete <name>` | Delete arena | `tower.admin` |
| `/tower start <gameId>` | Force start game | `tower.admin` |
| `/tower stop <gameId>` | Force stop game | `tower.admin` |
| `/tower arenas [retry]` | List arenas / retry loading | `tower.admin` |
| `/tower debug` | Debug info | `tower.admin` |
| `/tower reload` | Reload configs | `tower.admin` |

Aliases: `/tc`, `/towerclash`

## Game Modes

| Mode | Description |
|---|---|
| **Classic** | Standard play — blocks, snowballs, TNT, ender pearls |
| **Chaos** | More TNT, lava, fire charges — items arrive faster |
| **Low Gravity** | Reduced gravity, elytra, feathers and cobwebs |
| **Casual** | No TNT or lava, more blocks |

## Configuration Files

| File | Purpose |
|---|---|
| `config.yml` | General settings, timers, UI, hub location |
| `loot.yml` | Loot tables and item weights per game mode |
| `combat.yml` | Knockback multipliers and damage settings |
| `game.yml` | Game mode variants |
| `arenas.yml` | Arena data (auto-managed) |
| `messages.yml` | All player-facing messages |
| `scoreboard.yml` | Scoreboard layout |

## PlaceholderAPI Placeholders

| Placeholder | Value |
|---|---|
| `%towerclash_wins%` | Player win count |
| `%towerclash_kills%` | Player kill count |
| `%towerclash_deaths%` | Player death count |
| `%towerclash_games%` | Total games played |
| `%towerclash_winrate%` | Win rate percentage |
| `%towerclash_kd%` | Kill/death ratio |
| `%towerclash_beststreak%` | Best kill streak |

## License

This project is not open source. You may not redistribute or resell this plugin without permission.

---

Made by [Dutchwilco](https://dutchcoding.nl)
