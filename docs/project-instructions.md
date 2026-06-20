# Project Instructions

## Topic

Integrated streaming analytics using Kafka, validation, storage,
storage in DuckDB, and visualization.

This project extends the course example by adding a custom derived
field and analyzing the consumed output with a Jupyter notebook.

---

## Start and Run

Follow the instructions in:

[⭐ **Workflow: Apply Example**](https://denisecase.github.io/pro-analytics-02/workflow-b-apply-example-project/)

For peer review, complete only:

1. **Phase 1. Start & Run** – copy the project and confirm it runs.
2. **Skip Phase 2. Change Authorship** – do **not** modify the project ownership.
3. **Phase 3. Read & Understand** – review the project structure and code.

Run commands are provided in `README.md`.

---

## Custom Files

The primary files for this project are:

|File|Purpose|
|-----|-------|
|`src/streaming/kafka_producer_sowers.py`|Produces sales messages to Kafka|
|`src/streaming/kafka_consumer_sowers.py`|Consumes, validates, enriches, stores, and charts messages|
|`Notebook/sowers_sales_analysis.ipynb`|Analyzes the consumed output data|
|`data/output/consumed_sales_sowers.csv`|Processed records written by the consumer|
|`data/output/sales_chart_sowers.png`|Revenue visualization generated from consumed data|

---

## Make a Technical Modification

I modified the consumer by adding a new derived field:

```python
enriched["sowers_sales_level"] = (
    "High" if enriched["total"] >= 100 else "Standard"
)
```

Transactions with totals greater than or equal to 100 are classified
as `High`. Remaining transactions are classified as `Standard`.

The new field is written to:

```text
data/output/consumed_sales_sowers.csv
```

---

## Apply the Skills to a New Problem

I created a Jupyter notebook that analyzes the consumed
sales data using pandas and matplotlib.

The notebook reads:

```text
data/output/consumed_sales_sowers.csv
```

and generates a chart showing total revenue by region.

The analysis revealed that the US-TX region generated the highest
revenue in the sample dataset.

---

## Output Files

After running the producer and consumer successfully, the project creates:

- `project.log`
- `data/output/consumed_sales_sowers.csv`
- `data/output/sales_chart_sowers.png`
- DuckDB storage files

These files demonstrate how streaming data can be enriched and
transformed into business intelligence.
