# 魔獸之礦：第一階段核心遊戲循環流程圖

## 概述

第一階段定義為遊戲開局至抵達 **300 km 深度**（解鎖黑石深淵戰鬥系統）。
本階段的核心體驗是：**建立採礦基地 → 累積資源 → 招募英雄 → 解鎖設施 → 推進深度**。

---

## 主循環總覽

```mermaid
flowchart TD
    START([🟡 遊戲開始 / 新玩家進入]) --> TUTORIAL

    TUTORIAL[["📜 引導教學\n— 操控泰坦鑽井機\n— 收取第一批銅礦\n— 認識 UI 介面"]]
    TUTORIAL --> MAIN_LOOP

    subgraph MAIN_LOOP ["🔄 核心採礦循環（持續進行）"]
        direction TB
        DRILL["⛏️ 泰坦鑽井機自動挖掘\n（法力功率驅動）"]
        COLLECT["📦 收取資源\n銅礦 / 錫礦 / 寶石"]
        HEAT_CHECK{"🌡️ 鑽頭過熱？"}
        COOL["❄️ 等待冷卻\n或使用冷卻符文"]
        SPEND["💰 消耗資源"]

        DRILL --> COLLECT
        COLLECT --> HEAT_CHECK
        HEAT_CHECK -- 是 --> COOL --> DRILL
        HEAT_CHECK -- 否 --> SPEND --> DRILL
    end

    SPEND --> UPGRADE_BRANCH
    SPEND --> DEPTH_CHECK

    subgraph UPGRADE_BRANCH ["🔧 升級決策"]
        direction LR
        UPG_CHOICE{"玩家選擇"}
        UPG_DRILL["升級鑽井機\n地精電鑽 → 侏儒精準鑽頭\n→ 泰坦合金鑽頭"]
        UPG_WELL["佈置奧術之源\n12×12 法力網格\n魔力導管 + 冷卻符文"]
        UPG_FACILITY["建造 / 升級設施\n（見里程碑解鎖）"]

        UPG_CHOICE --> UPG_DRILL
        UPG_CHOICE --> UPG_WELL
        UPG_CHOICE --> UPG_FACILITY
    end

    UPGRADE_BRANCH --> MAIN_LOOP

    subgraph DEPTH_CHECK ["📏 深度里程碑檢查"]
        direction TB
        D10{"≥ 10 km？"}
        D15{"≥ 15 km？"}
        D45{"≥ 45 km？"}
        D300{"≥ 300 km？"}

        D10 -- 是 --> UNLOCK_ALTAR
        D15 -- 是 --> UNLOCK_AH
        D45 -- 是 --> UNLOCK_SQUAD
        D300 -- 是 --> PHASE2_TRIGGER

        D10 -- 否 --> MAIN_LOOP
        D15 -- 否 --> MAIN_LOOP
        D45 -- 否 --> MAIN_LOOP
        D300 -- 否 --> MAIN_LOOP
    end
```

---

## 里程碑解鎖分支

```mermaid
flowchart LR
    subgraph M10 ["🏛️ 10 km — 英雄祭壇"]
        UNLOCK_ALTAR(["解鎖英雄祭壇"])
        RECRUIT_HERO["招募英雄\n布蘭恩・銅鬚 / 蓋茲洛\n米歐浩斯・曼納斯多姆"]
        HERO_SKILL{"英雄技能觸發"}
        SKILL_PROD["艾澤里特產量 +500%\n（布蘭恩）"]
        SKILL_BUILD["建造時間 -30%\n（蓋茲洛）"]
        SKILL_STORM["法力風暴\n速度 ×10（隨機）\n或鑽頭停機（風險）\n（米歐浩斯）"]
        HERO_UPGRADE["奧術之源轉化\n靈魂碎片\n提升英雄星等"]

        UNLOCK_ALTAR --> RECRUIT_HERO
        RECRUIT_HERO --> HERO_SKILL
        HERO_SKILL --> SKILL_PROD
        HERO_SKILL --> SKILL_BUILD
        HERO_SKILL --> SKILL_STORM
        RECRUIT_HERO --> HERO_UPGRADE
    end

    subgraph M15 ["🏪 15 km — 拍賣行"]
        UNLOCK_AH(["解鎖拍賣行"])
        TRADE["資源兌換\n銅礦 ⇄ 秘銀 ⇄ 艾澤里特"]
        BLACK_MKT{"黑市出現？\n（隨機觸發）"}
        RARE_ITEM["取得稀有道具\n（限時購買）"]

        UNLOCK_AH --> TRADE
        TRADE --> BLACK_MKT
        BLACK_MKT -- 是 --> RARE_ITEM
        BLACK_MKT -- 否 --> TRADE
    end

    subgraph M45 ["🚁 45 km — 探險小隊"]
        UNLOCK_SQUAD(["解鎖探險小隊"])
        DISPATCH["派遣小隊\n（設定時間 / 目標地點）"]
        EXPEDITION_RESULT{"探索結果"}
        EXP_SUCCESS["取得技能材料\n專業道具 / 秘法符文"]
        EXP_FAIL["小隊損耗\n需補充人員"]

        UNLOCK_SQUAD --> DISPATCH
        DISPATCH --> EXPEDITION_RESULT
        EXPEDITION_RESULT -- 成功 --> EXP_SUCCESS
        EXPEDITION_RESULT -- 失敗/損耗 --> EXP_FAIL
        EXP_SUCCESS --> DISPATCH
        EXP_FAIL --> DISPATCH
    end
```

---

## 資源流向圖

```mermaid
flowchart TD
    subgraph RES_IN ["⛏️ 資源輸入"]
        COPPER["銅礦 / 錫礦\n（基礎產出）"]
        GEM["寶石\n（侏儒精準鑽頭加成）"]
        MITHRIL["秘銀 / 真銀\n（中層岩層）"]
        AZERITE["艾澤里特\n（布蘭恩英雄加成）"]
    end

    subgraph RES_PROC ["🔨 資源處理"]
        SMELT["精煉爐\n銅礦+錫礦 → 青銅錠"]
        TRADE_RES["拍賣行兌換\n（15 km 解鎖）"]
    end

    subgraph RES_OUT ["📤 資源消耗"]
        DR_UPG["鑽井機升級"]
        FAC_BUILD["設施建造"]
        HERO_REC["英雄招募費用"]
        SQUAD_EQ["探險小隊裝備"]
        ARCANE["奧術之源\n靈魂碎片升星"]
    end

    COPPER --> SMELT
    GEM --> TRADE_RES
    MITHRIL --> SMELT
    AZERITE --> ARCANE

    SMELT --> DR_UPG
    SMELT --> FAC_BUILD
    TRADE_RES --> HERO_REC
    TRADE_RES --> SQUAD_EQ
```

---

## 第一階段結束條件 → 進入第二階段

```mermaid
flowchart TD
    PHASE2_TRIGGER(["🔴 達到 300 km 深度"])
    UNLOCK_DUNGEON["解鎖黑石深淵副本\n戰鬥系統啟用"]
    FIRST_BOSS["首次面對\n黑鐵矮人衛兵（精英）"]
    CARD_INTRO[["💳 卡牌戰鬥教學\n英勇氣概 / 聖盾術 / 火球術"]]
    WIN{"勝利？"}
    LOOT["獲得副本掉落\n進階材料 / 英雄碎片"]
    RETRY["重整陣容\n強化英雄 / 升級鑽機"]
    DEEPER["繼續深挖\n目標：501 km 泰坦祭壇"]

    PHASE2_TRIGGER --> UNLOCK_DUNGEON
    UNLOCK_DUNGEON --> FIRST_BOSS
    FIRST_BOSS --> CARD_INTRO
    CARD_INTRO --> WIN
    WIN -- 是 --> LOOT --> DEEPER
    WIN -- 否 --> RETRY --> FIRST_BOSS
```

---

## 玩家決策優先序（第一階段建議路線）

| 優先級 | 行動 | 目標 |
|:---:|:---|:---|
| 1 | 升級泰坦鑽井機至侏儒精準鑽頭 | 提升採礦速度與寶石發現率 |
| 2 | 抵達 10 km，招募布蘭恩・銅鬚 | 艾澤里特產量 ×5 |
| 3 | 佈置奧術之源基礎網格 | 開始累積英雄靈魂碎片 |
| 4 | 抵達 15 km，啟用拍賣行 | 多餘資源兌換為稀缺材料 |
| 5 | 升級布蘭恩至 2★ | 強化被動產出效益 |
| 6 | 抵達 45 km，解鎖探險小隊 | 自動獲取技能材料 |
| 7 | 持續深挖 45 km → 300 km | 第二英雄槽 + 黑石深淵 |
| 8 | 升級泰坦合金鑽頭（最終形態） | 無視岩層阻力，加速推進 |

---

## 狀態機簡圖（玩家狀態）

```mermaid
stateDiagram-v2
    [*] --> 新手教學
    新手教學 --> 基礎採礦 : 完成教學

    基礎採礦 --> 資源精煉 : 累積足夠資源
    資源精煉 --> 設施建造 : 精煉完成
    設施建造 --> 英雄招募 : 10km 英雄祭壇
    英雄招募 --> 英雄強化 : 奧術之源啟動

    基礎採礦 --> 鑽機升級 : 消耗資源
    鑽機升級 --> 基礎採礦 : 升級完成（產能提升）

    英雄強化 --> 探險派遣 : 45km 探險小隊
    探險派遣 --> 材料收集 : 探索成功
    材料收集 --> 英雄強化 : 材料回饋

    英雄強化 --> 深淵門口 : 達到 300km
    探險派遣 --> 深淵門口 : 達到 300km
    深淵門口 --> [*] : 進入第二階段（黑石深淵副本）
```

---

*文件版本：v1.0 | 建立日期：2026-03-10 | 對應 GDD 版本：V1*
