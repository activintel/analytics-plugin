# 📊 Data Analytics Plugin for ACTIVI

**Plugin Type**: Multi-node  
**Platform**: [Active Intelligence Automation Systems (activi.ai)](https://activi.ai)  
**Maintainer**: [Activi Team](https://activi.ai)

---

## 🧩 Overview

The **Data Analytics Plugin** is a modular collection of interoperable **ACTIVI nodes** designed to support end-to-end analytics workflows.

Each node is purpose-built to handle a specific step in the analytics lifecycle—from ingest and transform to analysis and visualization.

This plugin provides a low-code interface for data professionals, enabling powerful insights and automation without writing custom scripts.

---

## 🧠 Supported Nodes

| Node Name             | Description |
|-----------------------|-------------|
| `load_csv`            | Ingests structured CSV data into the workflow |
| `filter_data`         | Applies row-level filters based on field values |
| `group_by`            | Performs aggregations (sum, avg, count, etc.) by category |
| `describe_data`       | Computes descriptive statistics |
| `generate_chart`      | Produces bar/line/pie charts as PNG or base64 |
| `export_csv`          | Converts processed data into downloadable CSV format |
| `insight_summary`     | Uses LLM to auto-summarize trends and anomalies |

Each node contains:
- `config_schema.json`: Form UI for parameter selection  
- `input_schema.json`: Expected data format from upstream node  
- `output_schema.json`: Data format for downstream compatibility  
- `process.py`: Executable logic that runs during workflow execution

---

## 📦 Plugin Structure

