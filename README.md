# Tesla OBC Gen-3 NPI — Supply Chain Program Portfolio

> **Program Manager:** Shruthi Wilson | USC Marshall MS Supply Chain 2025 | Ellis Bangalore 3 Years Operations & Sourcing
>
> **Program:** 11kW EV Onboard Charger | NPI Duration: 26 Weeks | SOP: Aug 7 2026 | Volume: 150,000 units/yr

---

## Program Overview

This portfolio simulates a complete Tesla OBC Gen-3 NPI Supply Chain Program from sourcing strategy through SOP. The program covers 9 commodity categories, 18 global suppliers, and $2.79M in annual savings achieved through structured should-cost modeling and supplier negotiation

**The OBC Gen-3 (11kW Onboard Charger)** is the power electronics component responsible for converting AC grid power to DC battery power in Tesla EVs. It contains IGBTs, MCUs, gate drivers, magnetics, capacitors, thermal interface materials, an aluminum housing, PCBs, and PCBA assembly — spanning 9 distinct supply chain commodities.

---

## Project Deliverables

| Phase | Deliverable | Tool | Status |
|-------|------------|------|--------|
| 1 | NPI Program Gantt Chart (30 tasks, 4 phases) | MS Project | ✅ Complete |
| 2 | RFQ Tracker + Supplier Universe (17 suppliers) | Excel | ✅ Complete |
| 3 | Should-Cost Model (170 elements, 9 commodities) | Excel | ✅ Complete |
| 4 | Risk Register (20 risks, P×I scoring) | Excel | ✅ Complete |
| 5 | SQL Database (4 tables, 6 queries) | SQLite | ✅ Complete |
| 6 | Power BI Dashboard (6 visuals, 3 KPI cards) | Power BI | ✅ Complete |
| 7 | Executive Summary (Week 18 Status Report) | Word | ✅ Complete |

---

## Cost Savings Summary

| Commodity | Supplier | Should-Cost | Final PPA | Reduction | Annual Saving |
|-----------|---------|-------------|-----------|-----------|---------------|
| Magnetics | TDK + Vishay India | $31.52 | $32.90 | 10.6% | $585,000 |
| PCBA Assembly | Flex + Foxconn | $29.50 | $30.80 | 9.9% | $510,000 |
| Aluminum Housing | Nemak Mexico | $28.40 | $29.50 | 9.5% | $465,000 |
| Capacitors | Murata + KEMET | $18.60 | $19.20 | 10.3% | $330,000 |
| IGBT Power Semi | Infineon Kulim | $19.78 | $20.95 | 10.5% | $367,500 |
| MCU Control IC | NXP Philippines | $8.20 | $8.60 | 12.7% | $187,500 |
| PCB Fabrication | TTM + Tripod TW | $9.90 | $10.15 | 9.4% | $157,500 |
| Thermal Interface | Henkel Germany | $4.20 | $4.40 | 13.7% | $105,000 |
| Gate Driver IC | Texas Instruments | $3.40 | $3.55 | 13.4% | $82,500 |
| **TOTAL** | | **$153.50** | **$160.05** | **11.3% avg** | **$2,790,000** |

---

## Phase 3 — Should-Cost Model

**File:** `OBC_Should_Cost_PROFESSIONAL.xlsx`

The should-cost model is the analytical backbone of the program. It contains **170 cost elements** across 9 commodity sheets, each built bottom-up from engineering first principles.

### Methodology
- **Two-model approach:** Simplified SC (negotiation anchor) + Detailed SC (technical backstop)
- **Wafer economics:** Wafer cost ÷ dies per wafer = die cost (e.g. $900 ÷ 420 dies = $2.143 for IGBT)
- **Overhead benchmarking:** 140% overhead challenged from supplier's 165% using SCEA Cost Engineering Handbook
- **Logistics modeling:** Air vs sea freight analysis — Infineon air→sea saves $462K/yr
- **LME commodity linking:** Copper and aluminum costs linked to live London Metal Exchange prices
- **Working capital:** Component value × 8% WACC × days in transit ÷ 365

### Key Should-Cost Wins
- **Magnetics:** Split award TDK China 60% + Vishay India 40% eliminates $493K/yr Section 301 tariff
- **PCBA:** SMT placement rate challenged $0.0025→$0.0018 + Foxconn dual award = $510K/yr
- **Thermal:** Overhead challenged 185%→140% using Parker Hannifin benchmark = $24K/yr

---

## Phase 4 — Risk Register

**File:** `OBC_Risk_Register.xlsx`

20 risks scored using **Probability × Impact (max 25)** methodology across Supply, Cost, Quality, and Schedule categories.

| Rating | Count | Top Risk |
|--------|-------|---------|
| CRITICAL (16-25) | 1 | R-01: NXP MCU 52-week lead time (Score 20) |
| HIGH (9-15) | 9 | R-02: Infineon single source (Score 15) |
| MEDIUM (4-8) | 10 | R-08: USMCA certification delay (Score 8) |

**Key Risk Mitigation:**
- R-01 NXP MCU: Buffer PO 20,000 units placed Week 4 + Renesas qualification Week 8 → Residual score 12
- R-03 China S301: TDK/Vishay split award executed → Residual score 8 (MITIGATED)

---

## Phase 5 — SQL Database

**Files:** `OBC_NPI_Database.db` | `OBC_NPI_Database.sql`

SQLite relational database with **4 tables connected by supplier_id foreign key.**

```sql
-- Tables
suppliers       (18 rows) -- supplier details, risk ratings, lead times, duty rates
rfq_log         (9 rows)  -- RFQ outcomes, should-cost, quotes, savings
milestones      (30 rows) -- program timeline, status, critical path flag
risk_register   (20 rows) -- risks, P×I scores, mitigations, residual scores
```

### 6 Business Queries

```sql
-- Query 1: Supply Risk Dashboard — ranked by risk + lead time
-- Query 2: Savings Analysis — sorted by annual savings
-- Query 3: Milestone Health — by week number with critical path
-- Query 4: Critical Risks only — WHERE risk_score > 9
-- Query 5: Total Savings Summary — program-level aggregation
-- Query 6: Tariff Exposure — JOIN suppliers + rfq_log for duty calculation
```

**Example JOIN query:**
```sql
SELECT s.supplier_name, s.country, s.total_duty_pct,
       r.final_ppa_usd,
       ROUND(r.final_ppa_usd * s.total_duty_pct / 100 * 150000, 0) AS annual_duty_exposure
FROM suppliers s
JOIN rfq_log r ON s.supplier_id = r.supplier_id
WHERE s.total_duty_pct > 0
ORDER BY annual_duty_exposure DESC;
-- Result: TDK China = $1,411,410/yr largest tariff exposure
```

---

## Phase 6 — Power BI Dashboard

**File:** `Tesla_OBC_NPI_Dashboard.png`

6-visual dashboard with 3 KPI cards built in Power BI Desktop.

**KPI Cards:**
- Total Annual Savings: $2.79M
- Total RFQs Executed: 9
- Highest Risk Score: 20 (R-01 NXP MCU)

**6 Visuals:**
1. Annual Savings by Commodity (horizontal bar)
2. Supplier Lead Time by Supplier (horizontal bar)
3. Milestone Status breakdown (donut chart)
4. Risk Score by Category (treemap)
5. Tariff Exposure by Supplier (waterfall)
6. Supplier Risk overview (table)

---

## Key Sourcing Decisions

### 1. Magnetics Split Award
**Decision:** TDK China 60% + Vishay India 40%
**Why:** HTS 8504.50 carries 3.6% MFN + 25% Section 301 = 28.6% total tariff on China origin. India = 3.6% only.
**Impact:** Eliminates $493,350/yr Section 301 exposure + reduces single-source risk

### 2. PCBA Dual Award
**Decision:** Flex Austin 60% + Foxconn Guadalajara 40%
**Why:** SMT placement rate challenged from $0.0025 to $0.0018 (-28%) using industry benchmarks. Foxconn dual award creates ongoing competitive pressure.
**Impact:** $510,000/yr — largest single saving in program

### 3. NXP Buffer PO
**Decision:** 20,000 unit pre-SOP buffer purchase order
**Why:** 52-week lead time creates full-year supply gap if disrupted. No replacement MCU without 6-month qualification.
**Impact:** Reduces R-01 residual risk score from 20 to 12

### 4. USMCA Optimization
**Decision:** Nemak USMCA certification application submitted Week 4
**Why:** Current duty 5.3% MFN. If USMCA certified (≥75% local content) → 0% duty.
**Impact:** $190,800/yr opportunity if certification approved

---

## About the Program Manager

**Shruthi Wilson**
- MS Supply Chain Management — USC Marshall School of Business (2025)
- B.Tech Aerospace Engineering — Alliance College of Engineering, Bangalore (2019)
- 3 years Operations Manager — Sourcing & Procurement, Ellis Bangalore
- Target Role: Tesla Supply Chain Program Manager (Power Electronics / Energy)

**Skills Demonstrated in This Project:**
- Bottom-up should-cost modeling (semiconductor economics, overhead benchmarking, logistics)
- Supplier negotiation strategy and PPA execution
- NPI program management (Gantt, milestones, critical path)
- Risk management (P×I scoring, mitigation planning)
- SQL database design and business queries
- Power BI dashboard development
- Trade compliance (HTS codes, Section 301, USMCA)
- Automotive quality standards (IATF 16949, AEC-Q101, PPAP Level 3)

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Microsoft Excel | Should-cost model, RFQ tracker, risk register |
| MS Project | NPI Gantt chart and critical path |
| SQLite + DB Browser | Relational database and SQL queries |
| Power BI Desktop | Supply chain analytics dashboard |
| Microsoft Word | Executive status report |
| GitHub | Portfolio hosting and version control |

---

*This is a portfolio simulation project for career development purposes.*
*All supplier names, prices, and program details are illustrative and do not represent actual Tesla programs or supplier agreements.*
