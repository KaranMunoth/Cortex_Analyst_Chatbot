# 💬 Cortex Analyst + Streamlit App on Snowflake

> Build a natural language interface to interact with your Snowflake data using Cortex Analyst, Cortex Search, and Streamlit.

---

## Step 1: Overview

**Cortex Analyst** is a fully managed Snowflake service that allows users to ask questions in natural language and receive SQL-driven answers over structured datasets. It is powered by LLMs and is designed to:

- Improve business accessibility to data.
- Eliminate the need for constant dashboard refreshes or SQL writing.
- Provide near real-time, conversational analytics.

In this quickstart, you'll learn how to:

- Create and configure a Semantic Model.
- Use the Cortex Analyst REST API via a Streamlit app.
- Integrate Cortex Search for improved query accuracy.
- Support star schema joins and multi-turn conversations.

---

## Step 2: Set Up Snowflake Environment

1. **Open `create_snowflake_objects.sql`** in Snowsight.
2. **Run commands** to create:
   - Role: `cortex_user_role`
   - Warehouse: `cortex_analyst_wh`
   - Database & Schema: `cortex_analyst_demo.revenue_timeseries`
   - Stage: `raw_data`
   - Tables:
     - `daily_revenue`
     - `product_dim`
     - `region_dim`

> 💡 Ensure you're using the correct roles (`SECURITYADMIN`, `SYSADMIN`, `cortex_user_role`) as instructed in the SQL script.

---

## Step 3: Ingest Data & Semantic Model

1. Upload the following files to Snowflake stage:
   - `daily_revenue.csv`
   - `product.csv`
   - `region.csv`
   - `revenue_timeseries.yaml`

2. Use Snowsight → **Data » Add Data** → **Load files into stage**:
   - Target Database: `CORTEX_ANALYST_DEMO`
   - Schema: `REVENUE_TIMESERIES`
   - Stage: `RAW_DATA`

3. Run `load_data.sql` to ingest all 3 CSVs into their respective tables.

---

## Step 4: Integrate Cortex Search

1. Open `cortex_search_create.sql` and run:
   - Creates a Cortex Search service over `product_line` values in `product_dim`.
   - This improves literal value matching for SQL generation.

---

## Step 5: Create a Streamlit Conversational App

1. Go to Snowsight → **+ Streamlit App**.
2. Open `cortex_analyst_sis_demo_app.py`.
3. Paste the code into the Streamlit editor.
4. Click **Run** and interact with your data using natural language.

> 🔍 The `get_analyst_response` function handles all API calls to the Cortex Analyst service.

---
## 🧠 Example Questions to Ask the Assistant

- Compare actual vs forecasted revenue by month for 2024.  
- Show total profit by region and product line in descending order.  
- Which product lines had revenue below $5,000 in March 2024

## Step 6: Define the Semantic Model

Your semantic model file (`revenue_timeseries.yaml`) defines:

- **Tables and Relationships** for joins
- **Logical Columns**:
  - `measures`: e.g. `daily_revenue`, `daily_profit`
  - `dimensions`: e.g. `product_line`
  - `time_dimensions`: e.g. `date`

Example measure:
```yaml
- name: daily_profit
  expr: revenue - cogs
  description: profit is the difference between revenue and expenses
  data_type: number



