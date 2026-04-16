# McKinsey Supply Chain 4.0 — Replication Agent

**Replicating a McKinsey consumer goods supply chain engagement using Python + n8n + Google Trends.**  
Built by **Ridhi Jain** as part of an independent AI consulting portfolio.

> *"Take a BCG/Bain/McKinsey case study, replicate it with AI automation, and see how it varies from the original."*

---

## The McKinsey Case Study

**Source:** "Supply Chain 4.0 in Consumer Goods"  
**URL:** mckinsey.com/capabilities/operations/our-insights/supply-chain-40-in-consumer-goods  
**Year:** 2018 (framework still industry-standard in 2026)  
**McKinsey's method:** 6 consultants, 3 months, ₹5 Cr+ engagement fee  
**Our method:** Python agent + n8n, 21 days, ₹0

---

## The 5-Lever Framework (What McKinsey Does)

```
McKinsey Supply Chain 4.0
│
├── L1: Demand Sensing         → Replace lagging forecasts with real-time demand signals
├── L2: Supply Visibility      → Control tower: see every unit in real time  
├── L3: Inventory Optimisation → AI-driven reorder points, kill dead stock
├── L4: Logistics Optimisation → Dynamic routing, ML carrier selection
└── L5: Digital Integration    → Unify online-offline, fix conversion friction
```

---

## What We Built vs What McKinsey Does

| Lever | McKinsey Tool | Our Tool | Match | Gap |
|-------|--------------|----------|-------|-----|
| L1 Demand Sensing | Proprietary POS data from 2.7L retailers | Google Trends API + calibration | ✅ HIGH | Normalized index vs unit counts |
| L2 Supply Visibility | IoT + RFID control tower | Google Sheets + Zapier | ⚠️ MEDIUM | 24hr lag vs real-time |
| L3 Inventory Optimisation | OR-Tools min-cost flow (Gurobi) | Python greedy heuristic | ✅ HIGH | ~5-10% sub-optimal |
| L4 Logistics | Live GPS + ML carrier selection | Static lead-time table | ❌ LOW | No live logistics data |
| L5 Digital | A/B testing 200+ variants | PageSpeed audit only | ⚠️ MEDIUM | No A/B test capability |

**Important data note:** Google Trends and other public signals are **directional proxies**, not equivalent to proprietary retailer-level transaction data used in full consulting engagements.

---

## Quick Start

```bash
git clone https://github.com/ridhijain709/mckinsey-supply-chain-replication
cd mckinsey-supply-chain-replication
pip install -r requirements.txt

# Run full 5-lever analysis on Honasa/Mamaearth
python mckinsey_agent.py

# Run on a different company
python mckinsey_agent.py --company Nykaa

# Show detailed McKinsey vs AI comparison
python mckinsey_agent.py --compare
```

**Sample output:**
```
LEVER 3: INVENTORY OPTIMISATION  🟢
  Current:  300.0 ₹ Crore dead stock
  Target:   50.0 ₹ Crore
  Impact:   ₹69.10 Cr  |  Confidence: HIGH
  Gap: McKinsey uses OR-Tools. We use greedy heuristic (~5-10% sub-optimal)

TOTAL IMPACT:  ₹146.9 Cr potential
High-confidence: ₹91.6 Cr (conservative)
McKinsey cost:   ₹5 Cr | Our cost: ₹0
```

---

## n8n Workflow (Daily Automation)

Import `n8n/workflow.json` into your n8n instance.

```
8 AM trigger
    ↓
[Google Trends API] + [Google Sheets inventory]
    ↓
[Lever 1: Demand Sensing code node]
    ↓
[Lever 3: Inventory Optimisation code node]
    ↓
[Build Slack message]
    ↓
[Slack HITL alert] + [Google Sheets log]
```

**Setup (all free):**
1. n8n: self-host on your laptop (`npx n8n`) or n8n.cloud (free tier)
2. Add env vars: `SLACK_WEBHOOK_URL`, `SHEET_ID`
3. Connect Google Sheets OAuth2 credential
4. Import `n8n/workflow.json`
5. Test → Activate

---

## How This Project Follows the Assignment

**Step 1: "Read blogs on AI for consultation"**  
→ Read McKinsey's actual case study at the URL above. That IS the consultation blog.

**Step 2: "Take a McKinsey case study"**  
→ McKinsey Supply Chain 4.0, Consumer Goods, 2018.

**Step 3: "Replicate with Claude Code / n8n"**  
→ `mckinsey_agent.py` replicates the 5-lever Python logic  
→ `n8n/workflow.json` automates it to run daily

**Step 4: "See how it varies from the original"**  
→ The `--compare` flag shows exactly where our agent matches McKinsey and where it falls short

**Step 5: "This allows you to see how you need to improve"**  
→ Three clear next steps:
1. Add Google Maps Distance Matrix API → fix Lever 4 (logistics)
2. Add OR-Tools → improve Lever 3 accuracy from ~90% to 99%
3. Add A/B testing via Google Optimize → fix Lever 5

---

## Project Structure

```
mckinsey-supply-chain-replication/
│
├── mckinsey_agent.py           ← Main: run python mckinsey_agent.py
│
├── data/
│   ├── company_data.csv        ← Honasa Consumer Q3 FY26 real data
│   └── mckinsey_levers.csv     ← McKinsey benchmark targets per lever
│
├── n8n/
│   └── workflow.json           ← Import into n8n for daily automation
│
├── output/                     ← Auto-generated on first run
│   ├── mckinsey_replication_*.json
│   ├── lever_summary_*.csv
│   └── analysis_summary_*.txt  ← Stakeholder-ready summary
│
└── README.md
```

---

## How to Share Results with Stakeholders

Run `python mckinsey_agent.py` and share `output/analysis_summary_*.txt`.

The message shows:
- Which levers you replicated successfully (L1, L3)
- Where you failed (L4 logistics — biggest gap)  
- What you need to fix it (Google Maps Distance Matrix API)
- 3 specific follow-up questions for decision-makers

This is a real technical conversation, not "AI-generated content."

---

## AI Output Quality Checks (Before Sharing)

- Verify every numeric claim against `output/mckinsey_replication_*.json` and source CSVs.
- Keep all assumptions explicit (e.g., proxy data, static lead times, heuristic optimization).
- Label confidence levels clearly (HIGH / MEDIUM / LOW) in stakeholder summaries.
- Remove or rewrite any statement that implies parity with proprietary McKinsey datasets.

---

## Publishing Recommendation

For non-technical audiences, publish a narrative version on Medium/Substack (business problem, key insights, and decisions) and link this repository as technical backup for methods and reproducibility.

---

## Author

**Ridhi Jain** — Independent AI Automation & Strategy Consultant  
Muzaffarnagar / Delhi NCR · BBA Final Year, Maa Shakumbhari University  
Founder, TruthGrid · Author, *Aks: Reflections of the Soul*

[Portfolio](https://ridhijain708.github.io/ridhiii/) · [LinkedIn](https://linkedin.com/in/ridhi-jain) · [GitHub](https://github.com/ridhijain709)
