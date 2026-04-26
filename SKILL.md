---
name: equity-market-intelligence
description: Daily secondary market intelligence for a long-only investor focused on Hong Kong and US equities, especially technology, semiconductors, Chinese tech ADRs, and AI/robotics/crypto-related stocks. Triggers when the user asks for "stock market report", "二级日报", "港股美股", "equity daily", "market wrap", "盘面", or when scheduling daily research on HK/US stock markets. Generates a structured daily market brief with macro, sector rotation, stock-specific moves, earnings tracking, and IB research notes, saved as markdown + docx.
---

# Equity Market Intelligence — Daily Brief Generator

## Purpose
Generate a focused daily brief covering Hong Kong and US secondary market movements from the last 24 hours. Long-only perspective only. Covers macro drivers, sector rotation, key stock moves, earnings season tracking, and major investment bank research.

## Pre-Flight Checklist

1. Read `references/framework.md` for the user's equity investment framework.
2. Read `references/search-queries.md` for query templates.
3. Check today's date and scope searches to the last 24 hours.
4. Check the most recent prior brief to avoid duplication.

## Research Workflow

### Phase 1: Parallel Intelligence Gathering

Launch 5 parallel SearchWeb bundles:

- **Bundle A — US Market Overview & Macro** (3-4 queries)
- **Bundle B — Hong Kong Market & Chinese Tech ADRs** (3-4 queries, EN+CN)
- **Bundle C — Sector Rotation & Thematic** (3-4 queries)
- **Bundle D — Earnings Season** (2-3 queries)
- **Bundle E — IB Research & Sentiment** (2-3 queries)

Priority sources:
- US: WSJ, Reuters, Bloomberg, Schwab, Yahoo Finance, Barchart
- HK: 财联社, 华尔街见闻, 华盛通, 格隆汇, 雪球, Xinhua
- IB Research: Morgan Stanley, Goldman Sachs, JPMorgan, Bernstein
- X/Twitter: @DeItaone, @FirstSquawk, @unusual_whales, @Market_Reacts

### Phase 2: Deduplication & Signal Filtering

**Deduplication**: Check against the most recent prior brief. Skip signals with no material new development.

**Signal Quality Filter**:

| Priority | Signal Type | Action |
|----------|-------------|--------|
| **P0** | Major index movement (>1% or new highs/lows) | Include with context |
| **P0** | Earnings release / pre-announcement | Include with expected vs actual |
| **P0** | Major sector rotation (semis vs software, etc.) | Include with flow analysis |
| **P1** | Analyst upgrade/downgrade from tier-1 IB | Include with price target |
| **P1** | Macro event impacting asset prices (Fed, geopolitical, commodity) | Include with market reaction |
| **P2** | General commentary, technical analysis | Skip |

### Phase 3: Deep Dives (Conditional)

For earnings details, IB research summaries, or ambiguous macro signals, use FetchURL to read full articles.

## Report Structure

Generate `Equity_Daily_Brief_YYYYMMDD.md`:

```markdown
# 二级市场日报 — YYYY年MM月DD日

> 本期核心信号：[1-3句话总结当天最重要的市场发现]

---

## 一、大盘概览

### 美股
- S&P 500: [level] ([change])
- Nasdaq Composite: [level] ([change])
- Dow Jones: [level] ([change])
- Russell 2000: [level] ([change])
- VIX: [level]

### 港股
- Hang Seng Index: [level] ([change])
- Hang Seng Tech Index: [level] ([change])
- Hang Seng China Enterprises: [level] ([change])

### A股（参考）
- 上证指数: [level] ([change])
- 深证成指: [level] ([change])
- 创业板指: [level] ([change])

---

## 二、宏观与政策

### 美联储与利率
[Rate decisions, Fed speakers, treasury yield moves, market pricing of cuts/hikes]

### 地缘政治与大宗
[Middle East, US-China, oil prices, gold, USD/CNH, and their market impact]

### 中国政策
[PBOC, fiscal stimulus, regulatory changes impacting HK-listed Chinese companies]

---

## 三、板块轮动

### 领涨板块
[Sector, representative stocks, catalyst]

### 领跌板块
[Sector, representative stocks, reason]

### 值得关注的分化
[Unusual divergences, e.g. semis up / software down, large-cap up / small-cap down]

---

## 四、重点个股动态

### 科技股 / 半导体
[NVDA, AMD, INTC, TSM, AVGO, etc. — news, price move, catalyst]

### 中国科技 ADR / 港股
[BABA, TCEHY, BIDU, JD, PDD, 0700.HK, 9988.HK, etc.]

### AI / 机器人 / 加密概念股
[PLTR, COIN, MSTR, RBLX, robotics stocks, etc.]

### 新上榜 / 异动个股
[Any stock with unusual volume or price move worth noting]

---

## 五、财报季跟踪

### 今日发布
| 公司 | 预期 EPS | 实际 EPS | 预期营收 | 实际营收 | 盘后反应 |
|------|----------|----------|----------|----------|----------|

### 明日/下周预览
| 公司 | 日期 | 预期 EPS | 关键看点 |
|------|------|----------|----------|

---

## 六、投行研报

### 上调
| 公司 | 投行 | 评级变化 | 目标价 | 核心逻辑 |
|------|------|----------|--------|----------|

### 下调
| 公司 | 投行 | 评级变化 | 目标价 | 核心逻辑 |
|------|------|----------|--------|----------|

---

## 七、今日关注

[Specific stocks/sectors worth watching tomorrow, based on earnings calendar, macro catalysts, or technical levels. Long-only perspective only.]

---

## 八、市场火花

[Connect today's market facts to broader investment themes: AI capex cycle, China tech valuation reset, rate cut/hike implications, geopolitical risk pricing, etc. Analytical but grounded in facts. No price targets or trading recommendations.]

---

*本报告基于公开市场信息整理，仅供内部研究参考，不构成投资建议。*
```

### Writing Rules

1. **Facts only in sections 1-7**: No evaluative language. No "this proves...". Save analysis for section 8.
2. **Long-only only**: No short recommendations. Frame downside as "risk to watch" or "potential headwind".
3. **First-time companies**: Every stock/company mentioned for the first time in any brief gets 1 sentence: what it does, market cap tier, key business.
4. **Earnings accuracy**: Always include expected vs actual. Note if it's a beat, miss, or in-line.
5. **No repetition**: Skip signals already covered in prior briefs with no material update.
6. **Concise**: Each signal 2-4 sentences max. Each stock entry 2-3 sentences.
7. **Bilingual**: English company names with Chinese translations where helpful.

## Output & Delivery

1. Save markdown to: `/Users/dingzhaohang/Desktop/高榕/日报/二级/Equity_Daily_Brief_YYYYMMDD.md`
2. Convert to docx:
   ```bash
   pandoc "/Users/dingzhaohang/Desktop/高榕/日报/二级/Equity_Daily_Brief_YYYYMMDD.md" \
     -o "/Users/dingzhaohang/Desktop/高榕/日报/二级/Equity_Daily_Brief_YYYYMMDD.docx" \
     --toc --toc-depth=2
   ```
3. Confirm both files exist and report file sizes.
4. Present 3-5 bullet summary in chat.

## Anti-Patterns

- Do NOT recommend short positions or put options.
- Do NOT make price predictions.
- Do NOT include signals older than 48 hours unless materially updated today.
- Do NOT repeat yesterday's signals.
- Do NOT write evaluative language outside section 8.
- Do NOT spend more than 20 minutes on research.
