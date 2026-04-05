# 📚 Pre-Class: SQL Data Manipulation Language (DML)

**Estimated Time:** 30 minutes
**Prerequisites:** Lesson 1.3 — SQL DDL

> In Lesson 1.3 you built the structure of a database — creating tables, defining columns, and setting constraints. This lesson is about working with the *contents* of those tables: querying, filtering, summarising, and transforming data using DML.

---

## Step 1 — Watch the Video (8 min)

🎬 [Intro to SQL DML](https://youtu.be/baODMatLuqE)

An overview of what DML is, how it differs from DDL, and the key commands you'll use in class.

---

## Step 2 — Read the Core Concepts (15 min)

### DDL vs DML — What's the Difference?

In Lesson 1.3 you used **DDL** (Data *Definition* Language) to build containers — tables, schemas, columns. **DML** (Data *Manipulation* Language) is about working with the data *inside* those containers.

| DDL (Lesson 1.3) | DML (This Lesson) |
|---|---|
| `CREATE TABLE` | `SELECT` — retrieve data |
| `ALTER TABLE` | `WHERE` — filter rows |
| `DROP TABLE` | `GROUP BY` — summarise groups |

### The Key DML Commands

**Retrieve data:**
```sql
SELECT column1, column2 FROM table_name;
```

**Filter results:**
```sql
SELECT * FROM table_name WHERE condition;
-- Example: WHERE resale_price > 450000
```

**Summarise with aggregates:**
```sql
SELECT town, AVG(resale_price) AS avg_price
FROM resale_flat_prices_2017
GROUP BY town;
```
Common aggregate functions: `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`

**Transform data on the fly:**
```sql
-- CASE: conditional logic (like IF/ELSE)
SELECT
    CASE
        WHEN resale_price < 400000 THEN 'Budget'
        WHEN resale_price <= 700000 THEN 'Mid-Range'
        ELSE 'Premium'
    END AS price_category
FROM resale_flat_prices_2017;

-- CAST: convert data type
SELECT CAST(floor_area_sqm AS INTEGER) FROM resale_flat_prices_2017;
```

### The Dataset: Singapore HDB Resale Prices 2017

In class we'll work with real Singapore public housing resale transaction data. Each row represents one flat sold, with columns including `town`, `flat_type`, `floor_area_sqm`, `resale_price`, and `month`.

---

## Step 3 — Tool Setup (5 min)

1. Ensure **DbGate** is installed (from Lesson 1.3)
2. Download `unit-1-4.db` from the course repository
3. Open DbGate → New DuckDB connection → point to `unit-1-4.db`
4. Verify your setup by running:

```sql
SELECT * FROM resale_flat_prices_2017 LIMIT 5;
```

You should see 5 rows of HDB resale data. If you get an error, post in **#questions** on Discord.

---

## Step 4 — Check Your Understanding

Try to answer these before class, then check below.

1. What is the difference between `WHERE` and `HAVING`?
2. If you want to count how many flats were sold in each town, what SQL structure would you use?
3. What does `SELECT *` return, and why is it sometimes avoided?

<details>
<summary>Suggested answers</summary>

**Q1:** `WHERE` filters *individual rows* before any grouping happens. `HAVING` filters *groups* after a `GROUP BY` — you use it to filter on aggregated values (e.g., `HAVING AVG(resale_price) > 450000`). You can't use `WHERE` to filter on the result of an aggregate function.

**Q2:** `SELECT town, COUNT(*) FROM resale_flat_prices_2017 GROUP BY town;` — the `GROUP BY town` splits the data into one group per town, then `COUNT(*)` counts the rows in each group.

**Q3:** `SELECT *` returns every column in the table. It's convenient for exploration, but avoided in production queries because it retrieves more data than needed (slower), and if someone adds or removes columns from the table, your query results change unexpectedly.

</details>
