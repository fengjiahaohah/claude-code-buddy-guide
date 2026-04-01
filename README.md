# Claude Code Buddy System 完全指南

> 基于源码逆向分析的 Buddy 电子宠物系统深度指南

## 目录

- [系统概述](#系统概述)
- [如何激活](#如何激活)
- [宠物属性详解](#宠物属性详解)
- [所有物种 ASCII 精灵图](#所有物种-ascii-精灵图)
- [修改宠物（UserID 替换法）](#修改宠物userid-替换法)
- [Shiny Legendary 全物种 UserID 速查表](#shiny-legendary-全物种-userid-速查表)
- [配置文件详解](#配置文件详解)
- [技术架构](#技术架构)
- [常见问题](#常见问题)

---

## 系统概述

Buddy System 是 Claude Code 中隐藏的电子宠物彩蛋。输入 `/buddy` 后会孵化一只独一无二的 ASCII 宠物，它常驻在终端输入框旁边，拥有：

- 闲置动画（idle fidget）
- 眨眼动画
- 对话气泡（偶尔评论你的代码）
- `/buddy pet` 触发爱心飘浮动画

### 时间窗口

| 时间 | 行为 |
|------|------|
| 2026 年 4 月 1-7 日 | 启动时显示彩虹 `/buddy` 提示 |
| 2026 年 4 月及之后 | 命令永久可用 |

### 前提条件

- 需要 Anthropic 编译的正式版 Claude Code（npm 安装）
- `feature('BUDDY')` 编译时宏为 `true` 的版本
- 源码仓库或自编译版本**无法使用**（feature flag 会被死代码消除）

---

## 如何激活

1. 确保安装了最新版 Claude Code：
   ```bash
   npm update -g @anthropic-ai/claude-code
   ```

2. 启动 Claude Code，输入：
   ```
   /buddy
   ```

3. AI 会为你生成一只宠物的名字和性格，孵化完成后宠物出现在输入框右侧

4. 可以使用 `/buddy pet` 触发爱心互动

5. 使用 `/buddy mute` 可以静音宠物（隐藏但不删除）

---

## 宠物属性详解

### 稀有度（5 档）

| 稀有度 | 概率 | 颜色 | 属性下限 | 星级 |
|--------|------|------|---------|------|
| Common | 60% | 灰色 | 5 | ★ |
| Uncommon | 25% | 绿色 | 15 | ★★ |
| Rare | 10% | 蓝色 | 25 | ★★★ |
| Epic | 4% | 黄色 | 35 | ★★★★ |
| **Legendary** | **1%** | **红色** | **50** | **★★★★★** |

### 物种（18 种）

duck, goose, blob, cat, dragon, octopus, owl, penguin, turtle, snail, ghost, axolotl, capybara, cactus, robot, rabbit, mushroom, chonk

### 眼睛（6 种）

`·`, `✦`, `×`, `◉`, `@`, `°`

### 帽子（8 种，Common 不戴帽子）

none, crown, tophat, propeller, halo, wizard, beanie, tinyduck

### 闪亮（Shiny）

- **1% 概率**出现，类似宝可梦异色
- 由确定性 PRNG 决定，与 UserID 绑定

### 属性值（5 项）

| 属性 | 含义 |
|------|------|
| DEBUGGING | 调试能力 |
| PATIENCE | 耐心值 |
| CHAOS | 混乱度 |
| WISDOM | 智慧值 |
| SNARK | 毒舌度 |

每个物种有一个 peak 属性（高值）、一个 dump 属性（低值），其余在中间范围。稀有度越高，所有属性的下限越高。

---

## 所有物种 ASCII 精灵图

### Duck（鸭子）
```
    __
  <({E} )___
   (  ._>
    `--´
```

### Goose（鹅）
```
     ({E}>
     ||
   _(__)_
    ^^^^
```

### Cat（猫）
```
   /\_/\
  ( {E}  {E})
  (  ω  )
  (")_(")
```

### Dragon（龙）
```
  /^\  /^\
 <  {E}  {E} >
 (   ~~   )
  `-vvvv-´
```

### Ghost（幽灵）
```
   .----.
  / {E}  {E} \
  |      |
  ~`~``~`~
```

### Penguin（企鹅）
```
  .---.
  ({E}>{E})
 /(   )\
  `---´
```

### Owl（猫头鹰）
```
   /\  /\
  (({E})({E}))
  (  ><  )
   `----´
```

### Octopus（章鱼）
```
   .----.
  ( {E}  {E} )
  (______)
  /\/\/\/\
```

### Turtle（乌龟）
```
   _,--._
  ( {E}  {E} )
 /[______]\
  ``    ``
```

### Snail（蜗牛）
```
 {E}    .--.
  \  ( @ )
   \_`--´
  ~~~~~~~
```

### Axolotl（美西螈）
```
}~(______)~{
}~({E} .. {E})~{
  ( .--. )
  (_/  \_)
```

### Capybara（水豚）
```
  n______n
 ( {E}    {E} )
 (   oo   )
  `------´
```

### Robot（机器人）
```
   .[||].
  [ {E}  {E} ]
  [ ==== ]
  `------´
```

### Rabbit（兔子）
```
   (\__/)
  ( {E}  {E} )
 =(  ..  )=
  (")__(")
```

### Mushroom（蘑菇）
```
 .-o-OO-o-.
(__________)
   |{E}  {E}|
   |____|
```

### Cactus（仙人掌）
```
 n  ____  n
 | |{E}  {E}| |
 |_|    |_|
   |    |
```

### Blob（果冻）
```
   .----.
  ( {E}  {E} )
  (      )
   `----´
```

### Chonk（胖墩）
```
  /\    /\
 ( {E}    {E} )
 (   ..   )
  `------´
```

> `{E}` 会被实际眼睛字符替换

---

## 修改宠物（UserID 替换法）

### 原理

宠物的稀有度、物种、眼睛、帽子、是否闪亮、属性值全部由 `hash(userID + "friend-2026-401")` 通过确定性 PRNG 生成。替换 `userID` 就能改变宠物。

### 前提

- **必须没有使用 OAuth 登录**（即没有执行过 `claude login`）
- 如果用了 OAuth，`oauthAccount.accountUuid` 优先级最高，无法通过修改 `userID` 改变宠物

### 步骤

#### 1. 查看当前配置

```bash
cat ~/.claude.json | jq '{userID, companion}'
```

#### 2. 备份配置

```bash
cp ~/.claude.json ~/.claude.json.bak
```

#### 3. 替换 userID 并清除旧宠物

选择下表中你喜欢的物种对应的 userID：

```bash
jq 'del(.companion) | .userID = "这里填你选的userID"' ~/.claude.json > /tmp/claude_tmp.json && mv /tmp/claude_tmp.json ~/.claude.json
```

#### 4. 重启 Claude Code 并孵化

```bash
claude
# 输入 /buddy
```

#### 5. 不满意？重复步骤 3-4

---

## Shiny Legendary 全物种 UserID 速查表

> 搜索约 36 万次找到的全物种 Shiny Legendary（最稀有组合：1% × 1% = 万分之一）

| 物种 | userID | 帽子 | 属性亮点 |
|------|--------|------|---------|
| axolotl | `53701760fde831b493b7f0c77d2bbde5cd958925dec377f8503343d21a6e7816` | tinyduck | SNARK:100 |
| blob | `8b599cd0ab10ac8937127e0af1e078793a06ab1895be1c34f296993bdc4c47bf` | tinyduck | SNARK:100 |
| cactus | `7a58a7fbf8ea2694221c7b92e1b541108e5d62568b7c99ea0a38c95ff33b1d28` | tinyduck | CHAOS:100 |
| capybara | `9030ffcdf9eceda9b63a7d0b0e72c33be827ca91a78d12c545cb88b9647e51a2` | tinyduck | SNARK:100 |
| cat | `dde04c87e340cd69026ba78705056ead79e3656272a1ac93f46b0f01671d68fc` | propeller | CHAOS:100 |
| chonk | `a68eb5ce7b2458a513c5e0f2204350b7d62a26ad99676edc608f1c5c73f9828e` | crown | CHAOS:100 |
| dragon | `9a4f1d6941ef94d4595d7460f21d6aa3a9b1e008fbb88db9c1d15666a0759648` | none | WISDOM:100 |
| duck | `6a7a94f4d01ba2df0aa2a14f856b51632247a567d276ef1c58b4ebb7e9a4d572` | tinyduck | CHAOS:100 |
| ghost | `6cfa144a92cf7682755a8332ef095f92ee3fac45642c350b6030b647e65c2afa` | none | DEBUG:100 |
| goose | `a4759b82af354755e2527bbe6dd3f67a93a9a34ee1f90e37be4a7e6c122b2d66` | wizard | CHAOS:100 |
| mushroom | `383eb472f5ea2247bc92e13b295a1c352a9e0097eaa08265aa0a6499c28e1628` | none | CHAOS:100 |
| octopus | `8d9c91768bc2fd123fdc36c2d3821f257eb02d2e7cfa9f89e767b5465593e97c` | crown | CHAOS:100, PAT:85 |
| owl | `c52ef497a6b662b84d71780905374b6720c51b1a308c9d16ea557cdee9aba1c3` | propeller | CHAOS:100, WIS:88 |
| penguin | `7c041f8c20f966c4bf8ff70833657f2524c661b8df2647b9cbca7f9ecb711964` | beanie | WISDOM:100 |
| rabbit | `cfd76febd92e9281d68a27fd222749177e7972dd8ba4a6349672f49a63e748d5` | halo | CHAOS:100 |
| robot | `ba8872cd550b2980a266eeec85fd21e4374a3fe95d7588230d6027020e6242f9` | crown | WISDOM:100 |
| snail | `3e9ae44b78f9240654fe660116a5fa8c47f07eccdd457cfa0f32fffbcb64f4f8` | halo | SNARK:100 |
| turtle | `c7352855d846d77c46d77fc383bce06005a26ca35cd53b3e9f8d3c9a29771e1c` | propeller | WISDOM:100 |

### 一键替换命令示例

以 Shiny Legendary Cat 为例：

```bash
cp ~/.claude.json ~/.claude.json.bak && \
jq 'del(.companion) | .userID = "dde04c87e340cd69026ba78705056ead79e3656272a1ac93f46b0f01671d68fc"' \
  ~/.claude.json > /tmp/claude_tmp.json && \
mv /tmp/claude_tmp.json ~/.claude.json
```

---

## 配置文件详解

### 文件位置

```
~/.claude.json          ← 主配置文件（生产环境）
```

文件名实际为 `.claude${fileSuffixForOauthConfig()}.json`，生产环境无后缀。

### 相关字段

```jsonc
{
  // 用户标识（非 OAuth 用户的宠物由此决定）
  "userID": "a1b2c3...（64位hex）",

  // OAuth 账户（优先级高于 userID）
  "oauthAccount": {
    "accountUuid": "xxxx-xxxx-xxxx"  // OAuth 用户的宠物由此决定
  },

  // 宠物灵魂数据（只有 name/personality/hatchedAt）
  "companion": {
    "name": "Fluffy",
    "personality": "snarky but lovable",
    "hatchedAt": 1743523200000
  },

  // 是否隐藏宠物（不删除）
  "companionMuted": false
}
```

### 数据安全设计

- **Bones（稀有度/物种/眼睛/帽子/属性）不持久化**，每次从 userID 重新计算
- `getCompanion()` 执行 `{ ...stored, ...bones }`，bones 始终覆盖 stored 中的旧字段
- 因此**无法通过编辑配置文件伪造稀有度**
- 物种名在源码中用 `String.fromCharCode` 编码，避免构建产物泄露明文

---

## 技术架构

### 模块结构

```
src/buddy/
├── types.ts                 ← 数据类型 & 常量定义
├── companion.ts             ← 核心算法（哈希、PRNG、稀有度判定）
├── sprites.ts               ← ASCII 精灵图渲染引擎（18物种×3帧）
├── CompanionSprite.tsx      ← React UI 组件（动画、气泡、pet互动）
├── useBuddyNotification.tsx ← 启动通知 & /buddy 触发器检测
└── prompt.ts                ← AI 模型 prompt 集成
```

### 随机数生成链路

```
userID + SALT("friend-2026-401")
  → hashString() [FNV-1a 或 Bun.hash]
    → mulberry32() [确定性 PRNG]
      → rng() #1 → rollRarity()     稀有度
      → rng() #2 → pick(SPECIES)    物种
      → rng() #3 → pick(EYES)       眼睛
      → rng() #4 → pick(HATS)       帽子（非 Common 才调用）
      → rng() #5 → < 0.01           闪亮判定
      → rng() #6+ → rollStats()     属性生成
```

### 动画系统

- **Tick 频率**：500ms
- **Idle 序列**：`[0,0,0,0,1,0,0,0,-1,0,0,2,0,0,0]`（15 步 = ~7.5 秒循环）
  - `-1` = 眨眼（眼睛替换为 `-`）
  - `1` / `2` = fidget 帧
- **兴奋模式**：说话或 pet 时快速循环所有帧
- **气泡显示**：~10 秒，最后 ~3 秒淡出
- **Pet 爱心**：2.5 秒飘浮动画

### 终端适配

| 宽度 | 模式 | 表现 |
|------|------|------|
| < 100 列 | 窄模式 | `(✦ω✦) 名字`，一行显示 |
| >= 100 列 | 完整模式 | 5 行 ASCII 精灵 + 气泡 |

---

## 常见问题

### Q: 为什么我输入 /buddy 提示 Unknown skill？

**A:** 你的版本中 Buddy 代码被编译时删除了。需要更新到包含该功能的版本：

```bash
npm update -g @anthropic-ai/claude-code
```

### Q: 我用了 OAuth 登录，能改宠物吗？

**A:** 不能。OAuth 用户的宠物由 `accountUuid` 决定，这个值来自 Anthropic 服务端，无法修改。只能换一个 Anthropic 账号。

### Q: 宠物的名字和性格能改吗？

**A:** 名字和性格存储在 `~/.claude.json` 的 `companion.name` 和 `companion.personality` 中，可以直接编辑。但物种、稀有度等无法通过编辑配置改变。

### Q: companionMuted 是什么？

**A:** 设置为 `true` 可以隐藏宠物，但不会删除。宠物数据保留，随时可以取消静音。

### Q: 为什么物种名在源码中用 fromCharCode 编码？

**A:** 注释说有一个物种名跟 Anthropic 的"模型代号金丝雀"冲突。构建时会 grep 产物检查是否泄露模型代号，运行时构造字符串可以绕过这个检查。

---

> 本文档基于 Claude Code v2.1.88 源码逆向分析生成
