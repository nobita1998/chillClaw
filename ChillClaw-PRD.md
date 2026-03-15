# ChillClaw（躺赚龙虾）— 项目文档

## 一、项目背景

### 1.1 比赛信息

- **赛事**：Binance x OpenClaw AI Agent Hackathon
- **主题**：用 AI 建设加密和币安一起逐浪 Web3
- **核心命题**：币安有哪些功能或服务可以通过 AI 实现创新和优化？
- **参赛要求**：使用 OpenClaw（小龙虾）搭建币安主题的 AI Agent，录制演示视频或图文展示
- **提交方式**：在 X/币安广场发帖，RT 官方推文并写明作品名称、功能亮点，附演示素材或链接
- **截止时间**：2026 年 3 月 18 日 23:59（UTC+8）
- **奖项**：第 1 名 10 BNB，第 2 名 8 BNB，第 3 名 6 BNB，优胜奖 20 名 × 1 BNB

### 1.2 比赛核心评判逻辑

参考 Claude Code Hackathon 获奖作品的共性规律：获奖产品不是 AI 聊天机器人，而是用 AI 加上自己的行业经验，做出**自动化一个痛苦的手动流程**的工具。产品需要：

1. **用到币安的 AI Agent Skills**（这是比赛硬性要求）
2. **对币安有明确价值**（提升用户参与度、交易量、留存率）
3. **解决真实用户痛点**（不是纯 demo，要有实际场景）

### 1.3 作者背景

作者（推特 @NNNNNNOBITA）是币安 Alpha/Booster 深度用户，已有产品：

- **Binance Booster 奖励解锁日历**（https://booster-unlock.vercel.app/）— 追踪 Booster 活动锁仓代币的解锁日期
- 长期在推特和币安广场输出 Alpha 积分 EV 计算、Booster 收益分析等内容
- 预测市场跨平台套利经验丰富（Polymarket/Opinion/Predict.fun）

---

## 二、产品定义

### 2.1 产品名称

- **英文**：ChillClaw
- **中文**：躺赚龙虾

### 2.2 一句话定位

基于 OpenClaw 的币安一站式智能助手 — 覆盖 Alpha 投研、Booster 活动追踪、理财优化三大场景，让币安用户"躺着也能赚"。

### 2.3 核心价值主张

币安用户日常面临三大场景的痛点：

| 场景 | 痛点 | ChillClaw 方案 |
|------|------|----------------|
| **Alpha** | 新币上线太快，来不及研究，怕买到垃圾币 | 自动发现新上线 → **自动投研**（Surf AI 专业报告 + 合约审计 + Smart Money 信号） |
| **Booster** | 活动多记不住，解锁时间容易忘，不知道该卖还是拿 | **发现活动** + 匹配持仓推荐参与 + **解锁通知** + 卖出分析 |
| **理财 Earn** | 产品太多、利率天天变、到期了忘续存 | **扫描持仓**闲置资产 + 扫描理财活动 → **推荐最优产品** + **到期提醒** |

ChillClaw 把三大场景的手动流程全部自动化，全程用户只需要看推送和对话追问。

### 2.4 对币安的价值

- **提升 Alpha 交易量**：自动投研降低决策门槛，让更多用户敢买新币
- **提升活动参与率**：主动推送匹配的活动，让更多用户参与 Launchpool/Booster
- **提升 Booster 闭环**：从"参与任务"到"追踪解锁"到"做出交易决策"，全程有 AI 陪伴
- **提升理财 TVL**：主动推荐最优理财产品 + 到期提醒续存，减少资金闲置
- **展示 Skills 生态协作**：币安 Skills + Surf AI + OpenNews MCP + 自建 Booster Calendar 四方协作，正是币安开放 Skills 想看到的生态效果

---

## 三、技术架构

### 3.1 三层架构

```
┌──────────────────────────────────────────────────────────────────────┐
│                           DATA LAYER                                 │
│                                                                      │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌─────────────┐ │
│  │ Binance      │ │ Surf AI      │ │ OpenNews     │ │ Booster     │ │
│  │ Skills (7个) │ │              │ │ MCP          │ │ Calendar    │ │
│  │              │ │ 专业投研报告 │ │              │ │             │ │
│  │ 行情+下单    │ │ 代币评分     │ │ 新闻抓取     │ │ 解锁日程    │ │
│  │ 持仓分析     │ │ 链上分析     │ │ 活动公告     │ │ 活动数据    │ │
│  │ 代币数据     │ │ 社交信号     │ │ AI评分       │ │ 锁仓状态    │ │
│  │ 热搜排行     │ │ 技术指标     │ │ 多空信号     │ │             │ │
│  │ SM信号       │ │ KOL舆情     │ │              │ │             │ │
│  │ 合约审计     │ │              │ │              │ │             │ │
│  └──────┬───────┘ └──────┬───────┘ └──────┬───────┘ └─────┬───────┘ │
│         │                │                │               │          │
└─────────┼────────────────┼────────────────┼───────────────┼──────────┘
          │                │                │               │
          ▼                ▼                ▼               ▼
┌──────────────────────────────────────────────────────────────────────┐
│                          AGENT LAYER                                 │
│                                                                      │
│  ┌─────────────────────────────────────────┐  ┌────────────────────┐ │
│  │          🦞 OpenClaw Agent              │  │ ⏰ Cron 触发器     │ │
│  │                                          │  │                    │ │
│  │  ┌────────────────────────────────────┐  │  │ Alpha 新币扫描     │ │
│  │  │ 场景 A: Alpha 投研                 │  │◄─│ Booster 解锁提醒   │ │
│  │  │ 发现新币 → Surf AI 投研 → 输出报告 │  │  │ 理财到期检查       │ │
│  │  ├────────────────────────────────────┤  │  │ 持仓扫描           │ │
│  │  │ 场景 B: Booster 活动              │  │  └────────────────────┘ │
│  │  │ 发现活动 → 匹配持仓 → 解锁通知    │  │                        │
│  │  ├────────────────────────────────────┤  │                        │
│  │  │ 场景 C: 理财优化                  │  │                        │
│  │  │ 扫描闲置 → 比较利率 → 推荐+到期   │  │                        │
│  │  └────────────────────────────────────┘  │                        │
│  │                                          │                        │
│  │  ⑤ 记忆系统 — 用户偏好+持仓+历史决策   │                        │
│  └─────────────────────────────────────────┘                        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
          │                 │                  │
          ▼                 ▼                  ▼
┌──────────────────────────────────────────────────────────────────────┐
│                          OUTPUT LAYER                                │
│                                                                      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐               │
│  │ 📡 Alpha     │  │ 🎯 Booster   │  │ 💰 理财      │               │
│  │ 投研报告     │  │ 活动推荐     │  │ 产品推荐     │               │
│  │ 买入建议     │  │ 解锁提醒     │  │ 到期提醒     │               │
│  │              │  │ 卖出分析     │  │ 利率对比     │               │
│  └──────────────┘  └──────────────┘  └──────────────┘               │
│                                                                      │
│                    👤 Telegram / Discord 用户                        │
└──────────────────────────────────────────────────────────────────────┘
```

### 3.2 数据源详情

#### 数据源 A：Binance AI Agent Skills（7 个）

币安于 2026 年 3 月 4 日发布首批 Skills，官方公告：https://www.binance.com/en/support/announcement/detail/bafb9dda6cbb47d5882a4090c31d4c64

| Skill | 功能 | 本项目用途 | 需要 API Key |
|-------|------|------------|-------------|
| Binance Spot | CEX 账户余额/持仓 + 实时行情 + 下单 | 理财场景：扫描 CEX 闲置资产、下单 | ✅ 需要 |
| Query Address Info | **链上**钱包持仓分布、估值、24h变化 | 查鲸鱼地址、匹配用户链上持仓 | ❌ 公开数据 |
| Query Token Info | 代币元数据：价格、流动性、持有人、交易活跃度 | 解锁日分析代币基本面 | ❌ 公开数据 |
| Crypto Market Rank | 热搜、SM流入排行、Meme叙事、交易员盈亏 | 判断代币热度趋势 | ❌ 公开数据 |
| Meme Rush | Meme币生命周期追踪、叙事主题映射 | 辅助判断（非核心） | ❌ 公开数据 |
| Trading Signal | SM买卖信号：触发价、现价、maxGain、exitRate | 解锁日判断卖/持 | ❌ 公开数据 |
| Query Token Audit | 合约审计：mint权限、冻结功能、owner特权 | Alpha 新币风险检查 | ❌ 公开数据 |

#### 数据源 B：Surf AI（专业投研）

官网：https://asksurf.ai

Surf AI 是专业加密投研平台（Pantera + Coinbase Ventures 投资 $15M），提供 AI 驱动的代币研究报告。覆盖 40+ 公链、10万+ KOL、200+ 技术指标，被 80% 顶级交易所和研究机构使用。

| 功能 | 用途 |
|------|------|
| Token Research Report | Alpha 新币自动投研：评分、链上数据、社交信号 |
| Token Scorecard | 综合评级（链上活跃度、持币分布、流动性、社交热度） |
| Chain Activity Tracking | 链上资金流向分析 |
| KOL Sentiment Analysis | 10万+ KOL 对代币的舆情分析 |

本项目主要用于 **Alpha 场景**：新币上线后，自动调用 Surf AI 生成专业投研报告，降低用户研究门槛。

#### 数据源 C：OpenNews MCP

GitHub: https://github.com/6551Team/opennews-mcp

提供加密新闻聚合、AI 评分、交易信号。本项目主要使用：

| 功能 | 用途 |
|------|------|
| `get_news_by_engine("listing")` | 抓取币安上新/活动类公告（Alpha 新币 + Booster 活动） |
| `search_news("Binance Launchpool")` | 搜索 Launchpool 活动信息 |
| `search_news("Binance Booster")` | 搜索 Booster 活动更新 |
| `search_news("Binance Earn")` | 搜索理财产品更新 |
| `get_news_by_signal` | 辅助判断市场情绪 |

安装方式（Claude Code）：
```bash
claude mcp add opennews \
  -e OPENNEWS_TOKEN=<your-token> \
  -- uv --directory /path/to/opennews-mcp run opennews-mcp
```

安装方式（OpenClaw）：
```bash
export OPENNEWS_TOKEN="<your-token>"
cp -r openclaw-skill/opennews ~/.openclaw/skills/
```

API Token 获取：https://6551.io/mcp

#### 数据源 D：Booster Calendar（自建）

已有产品：https://booster-unlock.vercel.app/

API endpoint 已就绪，暴露 JSON 格式的活动数据：

```
GET https://booster-unlock.vercel.app/api.json
```

数据包括：
- 各项目 TGE 日期和解锁时间表
- 活动状态（进行中/已结束/即将解锁）
- 收益详情链接
- 锁仓期状态

### 3.3 数据源 × 场景映射

| 场景 | 主要数据源 | 辅助数据源 |
|------|-----------|-----------|
| **Alpha 投研** | Alpha123（新币发现 + 合约地址）、Surf AI（投研报告）| Query Token Audit（合约审计）、Trading Signal（SM 信号） |
| **Booster 活动** | Booster Calendar（解锁数据）、OpenNews（活动发现）| Query Address Info（持仓匹配）、Binance Spot（实时价格）、Trading Signal（卖出分析） |
| **理财 Earn** | Query Address Info（持仓扫描）| Binance Spot（利率查询）、OpenNews（理财活动公告） |

### 3.4 权限与 API Key 配置

#### 需要 Key 的数据源

| Key | 用于场景 | 获取方式 | 必要性 |
|-----|---------|---------|--------|
| **Surf AI API Key** | Alpha 投研（投研报告、代币评分） | https://asksurf.ai | 必需 |
| **Binance API Key** | CEX 持仓扫描、理财推荐、下单执行（场景 C） | 币安账户 → API 管理 | 理财场景必需 |
| **OpenNews Token** | 币安活动发现（Launchpool/Booster/Earn） | https://6551.io/mcp | 必需 |

#### 无需 Key 的数据源（公开可用）

| 数据源 | 说明 |
|--------|------|
| Alpha123 API | Alpha 新币发现：`https://alpha123.uk/api/data?fresh=1` |
| Booster Calendar API | 自建解锁日历：`https://booster-unlock.vercel.app/api.json` |
| Binance 公开 API | 实时价格查询：`https://api.binance.com/api/v3/ticker/price` |
| Binance Skills（5个） | Query Address Info / Token Info / Token Audit / Market Rank / Trading Signal — 均为链上公开数据 |

#### 链上数据 vs CEX 账户数据

| 类型 | Skill | 数据来源 | 需要 API Key？ |
|------|-------|---------|---------------|
| 链上公开数据 | Query Address Info | 链上钱包（BSC/Base/Solana） | ❌ |
| 链上公开数据 | Query Token Info / Audit / Market Rank / Trading Signal | 链上数据 | ❌ |
| **CEX 账户** | **Binance Spot** | **币安中心化交易所账户** | **✅** |

> - `Query Address Info` 查的是**链上钱包地址**的持仓，不是币安 CEX 账户。用户需提供钱包地址。
> - 要查**币安交易所账户**的现货余额、理财产品，必须使用 `Binance Spot` Skill + API Key。

> ⚠️ **安全提醒**：Binance API Key 务必只开启读取权限和现货交易权限，**禁止开启提现权限**，并设置 IP 白名单。

### 3.5 开发工具

| 工具 | 用途 |
|------|------|
| Claude Code | 开发环境：写代码、调试 MCP 连接、验证 Skills API |
| OpenClaw | 生产环境：持久运行、Telegram 渠道、Cron 定时任务、演示录制 |

开发路径：先在 Claude Code 中跑通所有数据源连接和业务逻辑 → 迁移到 OpenClaw 接 Telegram + Cron → 录演示视频。

MCP Server 和 Skill 格式两边通用（遵循 AgentSkills 开放标准），迁移基本只需改配置。

---

## 四、核心功能流程

### 场景 A：Alpha 投研

#### 流程 A1：新币发现 → 自动投研

**触发条件**：Cron 定时扫描（如每小时）或 OpenNews 检测到 Alpha 新币上线公告

**流程**：
1. OpenNews MCP 抓取 Binance Alpha 新币上线公告
2. Agent 调 **Surf AI** 生成专业投研报告（评分、链上数据、社交信号、技术指标）
3. Agent 调 **Query Token Audit** 进行合约安全审计（mint 权限、冻结功能、owner 特权）
4. Agent 调 **Trading Signal** 查 Smart Money 是否已在买入
5. LLM 综合所有数据，生成投研结论和操作建议
6. 推送投研报告到 Telegram

**推送示例**：
> 📡 Alpha 新币上线：XXX (XToken)
>
> 🔬 **Surf AI 投研报告**
> 综合评分：78/100
> 链上：持币地址 12K，巨鲸占比 15%，24h 活跃地址 ↑45%
> 社交：KOL 提及 ↑340%，情绪正面 82%
> 赛道：AI + DePIN
>
> 🔒 **合约审计**：✅ 无 mint 权限 | ✅ 无冻结功能 | ⚠️ owner 可暂停交易
> 📊 **Smart Money**：过去 24h 净买入 $1.5M，3 个知名地址建仓
>
> 💡 综合评级：⭐⭐⭐⭐ 值得关注
> 建议：可小仓位建仓，关注 $0.12 支撑位
>
> 需要更详细的分析吗？

#### 流程 A2：用户主动查询代币

**触发条件**：用户在 Telegram 问 "帮我看看 XXX 这个币"

**流程**：
1. 调 Surf AI 获取该代币完整投研数据
2. 调 Query Token Info 获取实时基本面
3. 调 Query Token Audit 检查合约安全
4. 调 Trading Signal 获取 SM 信号
5. LLM 生成个性化分析（结合用户持仓和历史偏好）

---

### 场景 B：Booster 活动

#### 流程 B1：活动发现 → 匹配推送

**触发条件**：Cron 定时（如每小时）或 OpenNews 检测到新活动公告

**流程**：
1. OpenNews MCP 抓取 Binance 活动公告（Launchpool、Booster、Megadrop 等）
2. Booster Calendar 检测数据变更（新活动上线）
3. Agent 用 **Query Address Info** 查用户钱包持仓
4. LLM 匹配：用户当前资产 × 活动参与要求（如"需要质押 BNB"且用户有 BNB）
5. 推送个性化参与建议到 Telegram

**推送示例**：
> 🎯 新 Booster 活动上线：Sentio (ST)
> 你的钱包有 12.5 BNB 闲置，此活动需质押 BNB 参与，积分门槛 65 分。
> 总奖池 25,000,000 枚 ST，预估收益 $XXX。
> 需要了解更多吗？

#### 流程 B2：解锁通知 → 卖出分析

**触发条件**：Booster Calendar 中某代币解锁日到达（提前 1 天触发）

**流程**：
1. Booster Calendar 触发解锁提醒
2. Agent 调 **Query Token Info** 查当前价格、流动性、持有人数
3. Agent 调 **Trading Signal** 查 Smart Money 近期对该代币的买卖动向
4. Agent 调 **Crypto Market Rank** 查该代币的热度趋势
5. LLM 综合分析，生成行动建议（卖出 / 持有 / 分批出）
6. 推送解锁日报到 Telegram

**推送示例**：
> 📅 BTW 明日解锁（第 2/5 次）
> 你持有 1,200 枚 BTW，当前价 $0.45，估值约 $540。
> 📊 Smart Money 过去 48h 净流出 $150K，卖盘厚度为买盘 4 倍。
> 📈 Market Rank 热度排名下降 15 位。
> 💡 建议：解锁后设 $0.42 限价单先出一半，剩余观察 2 天。
> 想详细了解可以追问我。

---

### 场景 C：理财优化

#### 流程 C1：持仓扫描 → 产品推荐

**触发条件**：Cron 每日扫描一次（如每天 8:00）或用户主动询问

**流程**：
1. Agent 调 **Query Address Info** 扫描用户钱包，识别闲置资产（现货中未参与任何活动/理财的资产）
2. Agent 查询当前可用的理财产品和利率（Binance Earn 活期/定期/Launchpool）
3. Agent 对比各产品收益率，匹配用户闲置资产
4. LLM 生成个性化理财建议
5. 推送推荐到 Telegram

**推送示例**：
> 💰 持仓优化建议
>
> 你有以下闲置资产：
> • 2,000 USDT — 现货闲置
> • 0.5 BNB — 现货闲置
>
> 📊 当前最优选择：
> | 资产 | 方案 | 预估年化 |
> |------|------|----------|
> | 1,000 USDT | 锁仓 30 天定期 | 3.2% |
> | 1,000 USDT | Launchpool (ST) | APR ~12% |
> | 0.5 BNB | Launchpool (ST) | APR ~8.5% |
>
> 💡 建议：USDT 一半锁仓吃稳定收益，一半进 Launchpool 博新币
> 需要帮你操作吗？

#### 流程 C2：到期提醒 → 续存建议

**触发条件**：Cron 每日检查理财到期日（提前 1 天触发）

**流程**：
1. Agent 检查用户当前参与的理财产品到期时间
2. 对比当前市场最优利率和新活动
3. LLM 生成续存或转换建议
4. 推送到期提醒

**推送示例**：
> ⏰ 理财到期提醒
> 你的 2,000 FDUSD 定期（30天）明天到期，本期收益 $5.33。
>
> 📊 续存选项：
> • 续存 30 天定期：3.1%（利率微降）
> • 转活期：0.8%（灵活支取）
> • 转 Launchpool：新池子 APR 15%（3天后结束）
>
> 💡 建议：Launchpool 收益更高，建议转入，结束后再存定期
> 需要帮你操作吗？

---

### 通用流程：用户对话 → 实时应答

**触发条件**：用户在 Telegram 主动发消息

**流程**：
1. 用户发送自然语言消息
2. Agent 识别意图（Alpha 投研 / Booster 查询 / 理财咨询 / 下单等）
3. 按需调用对应 Skills / Surf AI / MCP 获取数据
4. LLM 推理生成回答
5. 支持追问和上下文记忆

**对话示例**：
- 用户："帮我看看最近有什么新币值得买"
  → Agent 调 OpenNews 查最新 Alpha 上线 + Surf AI 批量投研
- 用户："我有 3000U 闲钱，放哪好？"
  → Agent 查当前理财利率 + Launchpool + 定期产品，对比推荐
- 用户："上次 OPN 解锁我没卖亏了，这次 BTW 情况一样吗？"
  → Agent 调历史记忆 + 当前数据对比回答
- 用户："这个币安全吗？"
  → Agent 调 Query Token Audit + Surf AI 合约分析

---

## 五、关键设计原则

### 5.1 龙虾只做需要"脑子"的事

- **Cron 脚本负责**：定时触发、数据变更检测、API 轮询
- **龙虾（LLM）负责**：综合多源数据做判断、生成自然语言分析、处理用户对话
- 一条原则：如果 if-else 能写完的逻辑，不要交给龙虾

### 5.2 推送内容必须有决策价值

- ❌ "BTW 明天解锁了" — 脚本就能做
- ✅ "BTW 明天解锁，当前价 $0.45，SM 在出货，建议尽快出" — 需要 LLM 综合判断

### 5.3 Skills 使用要有深度

不是调一个 Skill 返回原始数据就完事，而是**串联多个 Skills 形成完整工作流**：
- Alpha 投研 = Surf AI 报告 + Token Audit + Trading Signal + OpenNews → LLM 综合投研
- 解锁分析 = Calendar + Token Info + Trading Signal + Market Rank → LLM 综合判断
- 活动匹配 = OpenNews + Address Info → LLM 个性化推荐
- 理财优化 = Address Info（闲置扫描）+ 利率对比 → LLM 最优推荐

---

## 六、开发计划

### Phase 1：验证数据源连通性（Claude Code）

- [ ] 安装 OpenNews MCP，验证能抓到 Binance Alpha / Booster / Earn 相关公告
- [ ] 接入 Binance Skills API，验证 Query Address Info / Token Info / Trading Signal / Token Audit 能正常返回数据
- [ ] 验证 Booster Calendar API (`api.json`) 可被外部读取
- [ ] 接入 Surf AI API，验证能获取代币投研报告

### Phase 2：实现核心业务逻辑（Claude Code）

- [ ] 实现场景 A：Alpha 投研（新币发现 + Surf AI 投研 + 合约审计 + SM 信号 → 投研报告）
- [ ] 实现场景 B：Booster 活动（活动发现 + 持仓匹配 + 解锁通知 + 卖出分析）
- [ ] 实现场景 C：理财优化（持仓扫描 + 产品推荐 + 到期提醒）
- [ ] 实现通用对话能力（意图识别 + 多场景路由 + 上下文记忆）

### Phase 3：迁移到 OpenClaw + 演示

- [ ] 部署 OpenClaw，接入 Telegram 渠道
- [ ] 迁移 Skills 和 MCP 配置
- [ ] 配置 Cron 定时任务（Alpha 扫描 / 解锁提醒 / 理财到期检查）
- [ ] 用真实钱包数据录制演示视频

### Phase 4：参赛提交

- [ ] 在 X 发帖：作品名称 + 功能亮点 + 演示视频
- [ ] 在币安广场同步发帖
- [ ] RT 官方比赛推文

---

## 七、演示视频脚本（建议）

1. **开场**（10s）：你好我是 NOBITA，这是 ChillClaw 🦞 — 躺赚龙虾
2. **痛点展示**（15s）：币安有 Alpha、Booster、理财这么多赚钱机会，普通用户跟不上、记不住、选不对
3. **场景演示 1 — Alpha 投研**（30s）：
   - 展示龙虾自动推送 Alpha 新币投研报告
   - Surf AI 评分 + 合约审计 + Smart Money 信号，一目了然
   - 用户追问"这个币安全吗？"，龙虾秒回合约审计详情
4. **场景演示 2 — Booster 活动**（30s）：
   - 展示新 Launchpool 活动推送，匹配用户持仓
   - 展示解锁通知：BAS 明天解锁 + SM 在出货 + 卖出建议
5. **场景演示 3 — 理财优化**（25s）：
   - 展示持仓扫描："你有 2000 USDT 闲置"
   - 推荐最优理财方案：一半锁仓 + 一半 Launchpool
   - 到期提醒："你的定期明天到期，建议转 Launchpool"
6. **收尾**（15s）：ChillClaw = Surf AI 投研 + 币安 Skills + Booster Calendar + OpenNews，四方数据协作，让你在币安躺着也能赚
7. **已有产品展示**（5s）：闪过 booster-unlock.vercel.app 截图 — "基于我已有的真实产品升级"

总时长约 2 分 10 秒。

---

## 八、相关资源

| 资源 | 链接 |
|------|------|
| 币安 Skills 公告 | https://www.binance.com/en/support/announcement/detail/bafb9dda6cbb47d5882a4090c31d4c64 |
| Surf AI（投研平台） | https://asksurf.ai |
| OpenNews MCP | https://github.com/6551Team/opennews-mcp |
| OpenTwitter MCP | https://github.com/6551Team/opentwitter-mcp |
| OpenClaw GitHub | https://github.com/openclaw/openclaw |
| ClawHub（Skill 市场） | https://clawhub.ai |
| Booster Calendar（已有产品） | https://booster-unlock.vercel.app/ |
| Booster Calendar API | https://booster-unlock.vercel.app/api.json |
| 作者推特 | https://x.com/NNNNNNOBITA |
