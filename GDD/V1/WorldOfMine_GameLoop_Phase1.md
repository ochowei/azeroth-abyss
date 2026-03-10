# 魔獸之礦：第一階段核心遊戲循環

**範圍**：遊戲開局 → 300 km（解鎖黑石深淵戰鬥系統）

---

## 核心循環

```mermaid
flowchart TD
    START([遊戲開始]) --> DRILL

    DRILL["⛏️ 泰坦鑽井機挖掘\n自動產出資源"]
    DRILL --> RESOURCE["📦 資源累積\n銅礦 / 錫礦 / 寶石"]

    RESOURCE --> UPGRADE["🔧 升級\n鑽機 / 設施 / 英雄"]
    RESOURCE --> DEEPEN["📏 推進深度"]

    UPGRADE --> DRILL
    DEEPEN --> MILESTONE{"里程碑？"}

    MILESTONE -- 10 km --> HERO["🏛️ 招募英雄\n強化產出"]
    MILESTONE -- 15 km --> AH["🏪 拍賣行\n資源兌換"]
    MILESTONE -- 45 km --> SQUAD["🚁 探險小隊\n自動獲取材料"]
    MILESTONE -- 繼續深挖 --> DRILL

    HERO --> DRILL
    AH --> DRILL
    SQUAD --> DRILL

    MILESTONE -- 300 km --> PHASE2([⚔️ 黑石深淵\n進入第二階段])
```

---

## 里程碑一覽

| 深度 | 解鎖 | 效益 |
|:---:|:---|:---|
| 10 km | 英雄祭壇 | 招募英雄，被動強化採礦效率 |
| 15 km | 拍賣行 | 多餘資源兌換稀缺材料 |
| 45 km | 探險小隊 | 掛機派遣，自動取得技能材料 |
| 300 km | 黑石深淵 | 解鎖戰鬥系統，進入第二階段 |

---

*文件版本：v1.1 | 2026-03-10*
