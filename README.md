# streaming-06-scenarios

> Streaming data analytics: complete pipeline.

This project demonstrates a complete Kafka streaming workflow.

Sales records are produced to a Kafka topic, consumed and validated,
enriched with derived fields, stored in DuckDB, and analyzed using a
Jupyter notebook.

## Custom Project Overview

This project extends the streaming analytics example by adding a
custom derived field named `sowers_sales_level`.

The producer streams sales transactions to the Kafka topic:

```text
streaming-06-scenarios-sowers
```

The consumer validates messages, computes derived values, writes
records to CSV, stores data in DuckDB, and classifies transactions as
either `High` or `Standard`.

I created a Jupyter notebook to analyze the consumed data and visualize
total revenue by region.

## Technologies Used

### Programming and Development

- Python
- VS Code
- Jupyter Notebook

### Streaming and Data Engineering

- Apache Kafka
- Streaming Analytics
- Event Streaming
- Data Engineering

### Data Analysis and Visualization

- Pandas
- Matplotlib
- Data Visualization
- Business Intelligence
- Real-Time Analytics

### Storage and Processing

- DuckDB
- CSV Processing

## Prerequisites

Before running the project, ensure the following software is installed:

- Python 3.12+
- VS Code
- uv package manager
- Java
- Apache Kafka
- Git
- Jupyter Notebook

Verify installations:

```bash
python --version
uv --version
java --version
```

## Clone the Repository

```bash
git clone <repository-url>
cd streaming-06-scenarios
```

## Create the Virtual Environment

Install all required packages:

```bash
uv sync --extra dev --extra docs
```

## Working Files

You'll work primarily with:

- **data/** - input and output files
- **docs/** - project documentation
- **Notebook/** - Jupyter notebook analysis
- **src/streaming/** - producer, consumer, and support modules
- **pyproject.toml**
- **zensical.toml**

## Quick Start

1. Start Kafka.
2. Create the topic.
3. Run the producer.
4. Run the consumer.
5. Open the notebook.
6. Run all notebook cells.

## Instructions

Follow the
[⭐ **Workflow: Apply Example**]
(<https://denisecase.github.io/pro-analytics-02/>
workflow-b-apply-example-project/)
to complete:

1. Phase 1. **Start & Run**
2. Phase 2. **Change Authorship**
3. Phase 3. **Read & Understand**
4. Phase 4. **Modify**
5. Phase 5. **Apply**

## Success

Use four named terminals:

1. **kafka**
2. **topics**
3. **producer**
4. **consumer**

After the producer and consumer run successfully, you should see:

```text
========================
Consumer executed successfully!
========================
```

A new `project.log` file will appear in the root project folder and
processed data will appear in `data/output/`.

## Command Reference

### Terminal 1: Start Kafka (`kafka`)

Open a new VS Code terminal and rename it `kafka`.

If running Windows, specify the terminal type as **wsl** or type:

```bash
wsl
```

#### Step 1. Verify Java and PATH

```bash
echo "$JAVA_HOME"

"$JAVA_HOME/bin/java" --version
```

#### Step 2. Rebuild Cluster ID (as Needed)

```bash
cd ~/kafka

rm -rf /tmp/kraft-combined-logs

KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"

echo "Cluster ID: $KAFKA_CLUSTER_ID"

bin/kafka-storage.sh format \
  --standalone \
  -t "$KAFKA_CLUSTER_ID" \
  -c config/server.properties
```

#### Step 3. Start Kafka Server

```bash
cd ~/kafka

bin/kafka-server-start.sh config/server.properties
```

Keep this terminal running.

### Terminal 2: Create Topic (`topics`)

Open another VS Code terminal and rename it `topics`.

If running Windows, specify the terminal type as **wsl** or type:

```bash
wsl
```

Run:

```bash
cd ~/kafka

bin/kafka-topics.sh --create \
  --bootstrap-server localhost:9092 \
  --partitions 1 \
  --replication-factor 1 \
  --topic streaming-06-scenarios-sowers
```

Verify the topic:

```bash
bin/kafka-topics.sh --list \
  --bootstrap-server localhost:9092
```

Expected output:

```text
streaming-06-scenarios-sowers
```

### Terminal 3: Run the Producer (`producer`)

Open another VS Code terminal and rename it `producer`.

If running Windows, use **PowerShell**.

Run:

```bash
uv run python -m streaming.kafka_producer_sowers
```

Messages should begin streaming to Kafka.

### Terminal 4: Run the Consumer (`consumer`)

Open another VS Code terminal and rename it `consumer`.

If running Windows, use **PowerShell**.

Run:

```bash
uv run python -m streaming.kafka_consumer_sowers
```

Successful execution ends with:

```text
========================
Consumer executed successfully!
========================
```

## Verify Outputs

After the consumer finishes, verify that the following files
have been created.

### Log File

```text
project.log
```

### Processed CSV

```text
data/output/consumed_sales_sowers.csv
```

### Visualization

```text
data/output/sales_chart_sowers.png
```

### DuckDB Database

```text
data/output/*.duckdb
```

## Run the Notebook

Launch Jupyter:

```bash
uv run jupyter notebook
```

Open:

```text
Notebook/sowers_sales_analysis.ipynb
```

Run all cells.

## Troubleshooting

If your Kafka installation uses KRaft mode instead of the course
legacy setup, replace `config/server.properties` with:

```text
config/kraft/server.properties
```

in the Kafka format and start commands.

If Kafka fails to start, rebuild the cluster ID using the course
instructions.

If you accidentally enter Python interactive mode and see:

```text
>>>
```

or

```text
...
```

press `Ctrl+C` (or `Ctrl+Z` then Enter on Windows).

## Make a Technical Modification

For my technical modification, I enhanced the Kafka consumer
by adding a new derived field named `sowers_sales_level`.

The consumer classifies transactions based on the total sale amount.

Sales with totals greater than or equal to 100 are labeled `High`,
while all remaining sales are labeled `Standard`.

```python
enriched["sowers_sales_level"] = (
    "High" if enriched["total"] >= 100 else "Standard"
)
```

I also updated the output field list so the new column would be
written to:

```text
data/output/consumed_sales_sowers.csv
```

### Results

After running the producer and consumer, the output file successfully
included the new `sowers_sales_level` column.

Transactions with totals greater than or equal to 100 were categorized
as `High`, while all other transactions were categorized as
`Standard`.

This modification demonstrates how streaming data can be enriched with
additional business information before being stored and analyzed.

## Apply the Skills to a New Problem

I applied the streaming analytics workflow to a new problem by creating
a Jupyter notebook that transforms the consumed sales data into
business insights.

Using pandas and matplotlib, I summarized and visualized total revenue
by region.

This additional analysis extended the original streaming example beyond
message production and consumption and demonstrated how streaming data
can support decision-making.

### Notebook Analysis

I used pandas and matplotlib to summarize the data stored in:

```text
data/output/consumed_sales_sowers.csv
```

The notebook generates visualizations that compare revenue across
regions.

### Business Insight

The notebook revealed that the US-TX region generated the highest total
revenue in the sample dataset.

Visualization:

The chart below summarizes total revenue by region using the consumed
Kafka sales data.

![Total Revenue by Region]
(data/output/sales_chart_sowers.png)

*Figure 1. Total revenue by region based on consumed sales data.*

- Explore the notebook:

```text
Notebook/sowers_sales_analysis.ipynb
```

for additional analysis and insights.

## Shut Down the Project

Press `Ctrl+C` in the:

- producer terminal
- consumer terminal
- kafka terminal

to stop all running processes.
