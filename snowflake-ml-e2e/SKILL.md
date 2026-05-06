---
name: snowflake-ml-e2e
description: "Generate end-to-end Snowflake ML demo notebooks. Use when: building ML demos, creating E2E ML pipelines, generating Snowflake Workspaces notebooks for training/inference/monitoring, SE demo prep. Triggers: ML demo, E2E ML, end-to-end ML, generate ML notebook, build ML pipeline, fraud detection demo, churn demo, forecasting demo, full ML demo, quick ML demo."
---

# Snowflake ML End-to-End Demo Generator

Generate complete, executable Snowflake Workspaces Notebooks (.ipynb) for end-to-end ML demos based on user requirements.

## When to Use

- Solutions Engineer needs a customized ML demo notebook
- User wants to build an E2E ML pipeline on Snowflake
- Creating training materials or enablement content
- Prototyping ML workflows for customer POCs

## Workflow

```
Ask Group 1 (Use Case) → Ask Group 2 (Features) → Ask Group 3 (Training)
    → Ask Group 4 (Registry) → Ask Group 5 (Serving) → Ask Group 6 (Ops)
    → Ask Group 7 (Monitoring) → Generate Notebook
```

## Stopping Points

- After all questions collected: confirm selections summary
- After notebook generated: offer to adjust or add sections

## Shortcuts

- User says **"quick demo"** or **"simple"**: synthetic data, no feature store, sklearn, warehouse target, SQL batch inference, no monitoring
- User says **"full demo"** or **"everything"**: enable all options
- User specifies a database/schema: use those names throughout

---

## Step 1: Use Case & Data

**Ask** (use ask_user_question, 3 questions):

1. Business use case:
   - Churn prediction, Fraud detection, Demand forecasting, Anomaly detection, Customer segmentation, or custom

2. Model task type:
   - Binary Classification / Multi-Class Classification / Regression / Time-Series Forecasting / Clustering

3. Data source:
   - Generate synthetic data (fastest for demos)
   - Existing Snowflake table (ask for `DATABASE.SCHEMA.TABLE`)
   - Upload file to stage

## Step 2: Feature Engineering

**Ask** (2 questions):

4. Include Snowflake Feature Store?
   - Yes (entities, managed feature views, PIT training sets)
   - No (raw DataFrame)

5. If yes: Online serving needed?
   - Yes (real-time feature retrieval)
   - No (offline/batch only)

## Step 3: Training Configuration

**Ask** (4 questions):

6. Framework: scikit-learn / XGBoost / LightGBM / PyTorch / CatBoost

7. Training scale: Single-node / Distributed (XGBEstimator/LightGBMEstimator) / Many Model Training

8. Include HPO? Yes (Tuner API) / No (defaults)

9. Include experiment tracking? Yes (log params + metrics) / No (direct register)

## Step 4: Model Registry & Versioning

**Ask** (2 questions):

10. Include Snowflake Datasets (versioned snapshots)? Yes / No

11. Target platform: WAREHOUSE / SNOWPARK_CONTAINER_SERVICES / BOTH

## Step 5: Serving & Inference

**Ask** (2 questions):

12. Serving mode: Batch SQL / Batch Python / Online SPCS / All approaches

13. If online: Include Snowflake Gateway (stable endpoint + A/B)? Yes / No

## Step 6: Operationalization

**Ask** (2 questions):

14. Include ML Jobs (@remote decorator)? Yes / No

15. Include pipeline orchestration (DAG)? Yes / No

## Step 7: Monitoring

**Ask** (2 questions):

16. Include ML Observability (Model Monitor)? Yes / No

17. Include ML Lineage (GET_LINEAGE)? Yes / No

**Ask** final question:

18. Database and schema to use? (default: `ML_DEMO.<USE_CASE>`)

---

## Output: Snowflake Workspaces Notebook

### Notebook Structure

Only include sections the user selected:

```
[Markdown] # End-to-End ML Demo: {Use Case}
[Markdown] ## 1. Setup & Configuration
[SQL]      USE ROLE / CREATE DATABASE / CREATE SCHEMA / CREATE WAREHOUSE / CREATE COMPUTE POOL / CREATE STAGE
[Python]   Session setup + imports
[Markdown] ## 2. Data Preparation
[Python]   Synthetic data generation OR table load
[Markdown] ## 3. Feature Engineering          ← if Feature Store selected
[Python]   Entity + FeatureView + generate_training_set
[Markdown] ## 4. Dataset Creation             ← if Datasets selected
[Python]   Dataset.create() with version
[Markdown] ## 5. Training                     ← always
[Python]   Train with experiment tracking / HPO as selected
[Markdown] ## 6. Model Registry               ← always
[Python]   reg.log_model() with metrics + task + sample_input_data
[Markdown] ## 7. Inference                    ← always
[Python/SQL] Batch SQL / Python / SPCS service as selected
[SQL]      Gateway creation                   ← if Gateway selected
[Markdown] ## 8. ML Jobs                      ← if selected
[Python]   @remote decorator pattern
[Markdown] ## 9. Pipeline                     ← if DAG selected
[Python]   DAG + DAGTask definition
[Markdown] ## 10. Monitoring                  ← if selected
[Python]   Create inference log table
[SQL]      CREATE MODEL MONITOR
[SQL]      Query drift + performance metrics
[Markdown] ## 11. Lineage                     ← if selected
[SQL]      GET_LINEAGE query
[Markdown] ## Summary — Objects Created       ← always
[Markdown] ## Cleanup (Optional)              ← always
[SQL]      Commented DROP statements
```

### Code Conventions

- Session: `from snowflake.snowpark.context import get_active_session`
- APIs: `snowflake-ml-python` (Registry, FeatureStore, Dataset, Experiment, ML Jobs)
- SQL cells: use `"language": "sql"` in cell metadata
- Always include `task` parameter in `log_model()` (required for Model Monitor)
- Always include `sample_input_data` (required for schema inference)
- Add `print()` statements showing results at each step (live demo flow)
- Use `SYSADMIN` role; note where elevated privileges needed (SPCS, EAI)
- Synthetic data: 50K rows default, ~3% minority class for classification
- Adapt synthetic schema to use case (transaction data for fraud, customer data for churn, product data for demand)

### Demo Best Practices

- Keep total execution under 5 minutes (small data unless distributed selected)
- End with summary table of all Snowflake objects created
- Cleanup section with commented DROP statements
- Comments explaining Snowflake-specific advantages at each step

### File Output

Write notebook as `.ipynb` to user's working directory. Filename: `{use_case}_e2e_demo.ipynb`

---

## Notes

- All frameworks listed have built-in Model Registry support (no CustomModel needed)
- For distributed training, create compute pool with MIN_NODES=1, MAX_NODES=3-4
- For SPCS inference, reuse same compute pool or create dedicated inference pool
- Gateway requires the inference service to be running first
- Model Monitor requires: prediction score column, timestamp column, ID column, and optionally actual/ground truth column
- ML Lineage is automatic when using Feature Store + Registry together
