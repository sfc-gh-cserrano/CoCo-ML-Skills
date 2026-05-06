---
name: ml-discovery
description: "Conduct ML migration or development discovery sessions. Use when: assessing ML workloads for Snowflake migration, running discovery workshops, evaluating current ML infrastructure, creating project assessment documents. Triggers: ML discovery, ML migration assessment, ML project assessment, discovery session, current state assessment, ML gap analysis, migration feasibility."
---

# ML Discovery & Project Assessment

Conduct structured discovery sessions for ML migration or development projects. Produces an HTML assessment deck documenting findings, capability gaps, and investigation areas.

## When to Use

- Customer wants to evaluate migrating ML workloads to Snowflake
- Solutions Engineer needs to run a discovery workshop
- Team needs a structured project assessment for ML capabilities
- Assessing feasibility of Snowflake ML for an existing ML platform

## Workflow

```
Ask Mandatory Fields (Account, Use Case #, ACV) → Ask Group 1 (Context)
    → Summarize → Ask Group 2 (Models) → Summarize
    → Ask Group 3 (Infra) → Summarize → Ask Group 4-5 (Features/Training)
    → Summarize → Ask Group 6-7 (Serving/Governance) → Summarize
    → Ask Group 8-9 (Monitoring/Migration) → Generate Assessment Deck
```

## Stopping Points

- After each question group: summarize answers, confirm accuracy
- Before generating output: confirm all critical info collected
- After output: ask if any sections need revision

---

## Step 0: Mandatory Fields (ALWAYS ASK FIRST)

**These fields are REQUIRED before proceeding. Do not skip.**

**Ask** (use ask_user_question with type: "text" for all three):

1. **Account Name** — Customer/prospect account name (e.g., "Acme Corp")
2. **Use Case Number** — Internal tracking ID (e.g., "UC-2026-0142")
3. **ACV** — Annual Contract Value associated with this opportunity (e.g., "$250K")

**Do NOT proceed to Step 1 until all three fields are collected.**

These fields will appear in the assessment deck header and are used for internal tracking.

---

## Step 1: Project Context

**Ask** (use ask_user_question tool):
- Primary goal: Migrate / Build new / Hybrid
- Business outcomes desired
- Timeline: POC (2-4 wk) / Pilot (1-3 mo) / Full (3-12 mo)
- Team composition (roles + counts)

**Summarize** answers before proceeding.

## Step 2: Current ML Landscape

**Ask:**
- Model count in production (1-5 / 5-20 / 20-50 / 50+)
- Model types (classification, regression, forecasting, anomaly, NLP, etc.)
- Frameworks (sklearn, XGBoost, LightGBM, PyTorch, TF, Prophet, Spark, etc.)
- Typical model size (< 100MB / 100MB-1GB / 1-10GB / 10GB+)
- Top 3-5 business-critical models (free text)

## Step 3: Infrastructure & Platform

**Ask:**
- Where models run (Cloud VMs, SageMaker/Vertex/Azure ML/Databricks, K8s, on-prem, notebooks)
- Training data location (Snowflake, S3/GCS/ADLS, lakehouse, on-prem DB)
- Data egress from Snowflake (none / <1GB / 1-100GB / 100GB+ / streaming)
- Orchestration (Airflow, Prefect, Dagster, cron, dbt, Databricks Workflows)
- Experiment tracking / registry (MLflow, W&B, Neptune, platform-native, none)

## Step 4: Feature Engineering

**Ask:**
- Feature engineering method (SQL/dbt, Python, feature store platform, ad-hoc)
- Centralized feature store? (yes/planned/no-duplicated)
- Common transformations (aggregations, time-based, categorical, NLP, geospatial, streaming, cross-entity)
- Point-in-time correctness concern? (yes-leakage / yes-compliance / somewhat / no)
- Features shared across teams? (yes / somewhat / no)

## Step 5: Training & Experimentation

**Ask:**
- Retraining frequency (real-time / daily / weekly / monthly / ad-hoc)
- Training data volume (<1M / 1M-100M / 100M-1B / 1B+ rows)
- Distributed training needed? (yes-data / yes-speed / no / exploring)
- GPU training? (yes-DL / yes-boosted-trees / no / want to explore)
- HPO approach (manual / grid-random / bayesian / platform-managed / none)
- Experiment tracking method (dedicated tool / notebooks / git / none)

## Step 6: Model Serving & Inference

**Ask:**
- Inference patterns (batch / real-time REST / streaming / SQL/BI embedded / edge)
- Volume (<1K / 1K-100K / 100K-10M / 10M+ predictions/day)
- Latency requirements (<50ms / <100ms / <500ms / <1s / no requirement)
- Deployment method (Docker+K8s / managed endpoints / app code / SQL UDFs / serialized files)
- A/B testing? (yes / planned / no)
- External prediction consumers? (yes-API / yes-tables / no)

## Step 7: Governance & Compliance

**Ask:**
- Governance requirements (model lineage / data lineage / RBAC / approval workflows / audit logging / regulatory)
- Data residency (no egress allowed / prefer no / yes / depends on classification)
- RBAC patterns needed (DS full access / ML eng deploy / analysts invoke / external limited)
- PII in features? (yes-masked / yes-raw / no)

## Step 8: Monitoring & Observability

**Ask:**
- Current monitoring (Evidently/WhyLabs/Arize / custom dashboards / manual / none)
- Metrics tracked (accuracy/F1/AUC / drift / volume+latency / business KPIs / none)
- Degradation handling (automated retrain / manual review / only when noticed / no process)
- Ground truth feedback loop (hours / weeks / partial / none)

## Step 9: Migration-Specific (if goal = migrate or hybrid)

**Ask:**
- Priority order (simplest first / most critical / closest to Snowflake data / big bang)
- Biggest pain points (cost / data movement / governance / tool sprawl / slow TTL / scaling / skill gaps)
- Models that CANNOT migrate? (free text)
- CI/CD process (GitHub Actions / GitLab / Jenkins / platform-native / manual / none)

---

## Output: HTML Assessment Deck

### Document Framing Rules (CRITICAL)

- This is a PROJECT ASSESSMENT, not a final recommendation
- Use assessment language: "reported", "observed", "estimated", "to validate", "pending confirmation"
- NEVER state definitive recommendations — frame as "considerations", "potential approaches", "hypotheses to test"
- Every section MUST include open questions or investigation items
- Include "Assessment Status: Draft" on hero slide
- Distinguish self-reported vs validated data

### Deck Structure (10 slides)

| Slide | Title | Content |
|-------|-------|---------|
| 1 | Assessment Overview | **Customer info table** (Account, UC#, ACV, Timeline, Goal), key stats grid, technology pills, "DRAFT" status line |
| 2 | Current State | Model inventory table, team, maturity scores (self-reported), open questions |
| 3 | Infrastructure Map | Mermaid diagram of current flow, reported pain points, investigation items |
| 4 | Capability Gaps | Current vs Snowflake equivalent table, preliminary effort badges, assumptions to validate |
| 5 | Architecture Considerations | Potential target-state Mermaid diagram, design principles, pending decision points |
| 6 | Platform Comparison | Tool mapping table with migration notes, conditional consolidation statement |
| 7 | Validation Approach | Suggested pilot model + rationale, what POC validates, blockers to resolve |
| 8 | Risks & Open Questions | Risk cards (Low/Med/TBD badges), critical unknowns box |
| 9 | Evaluation Criteria | Metrics table (Current/Target/How to Measure), unquantified metrics box |
| 10 | Investigation Areas | Technical/Business/Organizational columns, suggested next action |

### Design System

- Dark theme: `#0F1B2D` background, `#1B2A4A` sidebar, `#29B5E8` primary, `#7C3AED` accent
- Sidebar navigation with scroll-spy (IntersectionObserver)
- Slides: `height: 100vh`, `overflow-y: auto`
- Animations: staggered `anim-fade` with delays (d1-d6), hero keyframe animations
- Progress bar at top
- Callout boxes: `highlight-box` (blue/purple), `context-box` (green), `warning-box` (orange)
- Badges: `badge-green` (Low), `badge-orange` (Med), `badge-blue` (TBD/Optional)
- Cards with hover elevation, tables with row hover highlights
- Mermaid diagrams with dark theme variables
- Font: system stack; Code: JetBrains Mono

### Output File

Write the HTML deck to the user's working directory as `discovery_output.html`.

---

## Notes

- Adapt questions based on responses (skip Group 9 if goal is "Build new")
- If user says "quick assessment", reduce to Groups 1-3 + 6 + generate
- Always ask in batches using ask_user_question (3-4 questions per call)
- Summarize after each group to confirm accuracy before proceeding
