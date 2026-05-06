# ML E2E Enablement — Cortex Code Skills

This repository contains two Cortex Code custom skills for Snowflake ML enablement workflows.

## Skills

### `ml-discovery-skill`

A structured discovery session tool for assessing ML workloads before migration or new development on Snowflake.

**What it does:**
- Walks through a guided questionnaire covering project context, current ML landscape, infrastructure, feature engineering, training, serving, governance, monitoring, and migration specifics
- Collects mandatory tracking fields (Account, Use Case #, ACV) upfront
- Summarizes answers after each question group for confirmation
- Generates a 10-slide HTML assessment deck (`discovery_output.html`) with dark-themed design, Mermaid diagrams, capability gap tables, risk cards, and investigation areas

**When to use:** ML migration assessments, discovery workshops, gap analyses, feasibility evaluations.

---

### `snowflake-ml-e2e`

An end-to-end ML demo notebook generator that produces executable Snowflake Workspaces Notebooks (.ipynb).

**What it does:**
- Collects requirements across use case/data, feature engineering, training, registry, serving, operationalization, and monitoring
- Generates a complete notebook with only the sections the user selected
- Supports shortcuts: "quick demo" for minimal setup, "full demo" for all capabilities
- Covers: synthetic data generation, Feature Store, Datasets, HPO (Tuner API), experiment tracking, Model Registry, batch/online inference, ML Jobs (`@remote`), DAG pipelines, Model Monitor, and ML Lineage

**Supported frameworks:** scikit-learn, XGBoost, LightGBM, PyTorch, CatBoost

**When to use:** SE demo prep, customer POC prototyping, training materials, E2E ML pipeline creation.

---

## Usage

Reference either skill by name in Cortex Code (e.g., `@ml-discovery-skill` or `@snowflake-ml-e2e`) to start the guided workflow.
