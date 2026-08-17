
# 音乐与音效

Hacknet 的配乐以氛围化电子乐为主，多来自独立电子音乐人的授权。以下清单整理自游戏 `Content/Music` 与 `Content/DLC/Music` 目录。


## 基础游戏曲目（Content/Music）

| 曲目 | 备注 |
|---|---|
| `Bit(Ending).ogg` | 结局主题曲，Bit 的最终演说配乐 |
| `Broken_Boy.ogg` | 剧情场景曲 |
| `Irritations.ogg` | 剧情场景曲 |
| `Revolve.ogg` | 剧情场景曲 |
| `Traced.ogg` | 被追踪时的氛围曲 |
| `The_Quickening.ogg` | 主线加速推进时的曲目 |
| `out_run_the_wolves.ogg` | 剧情场景曲 |
| `tetrameth.ogg` | 剧情场景曲 |
| `TheAlgorithm.ogg` | The Algorithm 相关授权曲 |
| `Ryan10.ogg` / `Ryan3.ogg` | 剧情场景曲 |
| `Roller_Mobster_Clipped.ogg` | **M.O.O.N. - Roller Mobster**（《霓虹恶魔》配乐） |
| `Rico_Puestel-Roja_Drifts_By.ogg` | **Rico Puestel - Roja Drifts By** |

### 环境音（Ambient）

| 曲目 | 备注 |
|---|---|
| `AmbientDrone_Clipped.ogg` | 环境氛围底噪 |
| `dark_drone_008.ogg` | 暗调氛围音 |
| `spiral_gauge_up.ogg` / `spiral_gauge_down.ogg` | 压力表上升/下降氛围音 |


## DLC《Labyrinths》曲目（DLC/Music）

| 曲目 | 备注 |
|---|---|
| `HOME_Resonance.ogg` | **HOME - Resonance**（vaporwave 名曲，致敬网络文化） |
| `CrashTrack.ogg` | 航班坠机危机的终局曲 |
| `World_Chase.ogg` | 场景曲 |
| `Slow_Motion.ogg` | 慢镜氛围曲 |
| `DreamHead.ogg` | 场景曲 |
| `Userspacelike.ogg` | 场景曲 |
| `snidelyWhiplash.ogg` | 场景曲 |
| `Remi2.ogg` / `RemiDrone.ogg` / `Remi_Finale.ogg` | Remi 主题曲（终局相关） |


## 小知识

- 大部分曲目经过"剪辑"（文件名带 `_Clipped`），在游戏内按场景循环播放。
- 音乐通过 `ChangeSong` 类机制切换，DLC 还会用到 `changeSongDLC`（如 Bibliotheque 加入时切歌）。
- 想要官方 OST 可以关注游戏 Steam 页面与音乐人的 Bandcamp/Spotify 页。
