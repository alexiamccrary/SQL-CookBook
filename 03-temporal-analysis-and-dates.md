# Chapter 3: Temporal Analysis & Date/Time Manipulation

This section details methods for handling date, time, and timestamp dimensions within relational databases. Topics include date extraction functions, interval calculations, Unix/Epoch epoch conversions, custom timestamp string formatting, and joining tables on temporal intersections.

---

## 1. Foundational Building Blocks (Isolated Concepts)

### Extracting Date Components
Isolates a single unit of time (such as year, month, or day) from a full date or timestamp field.
*   **Definition:** Allows you to filter or group data chronologically without needing exact match matches down to the second.

```sql
SELECT 
    YEAR(date_column) AS transaction_year,
    MONTH(date_column) AS transaction_month,
    DAY(date_column) AS transaction_day
FROM table_name;
```

### Date Differences (Intervals)
Calculates the numerical span of time between two specific calendar dates.
*   **Definition:** The `DATEDIFF()` function evaluates the elapsed interval between a starting date and an ending date, returning the distance as an integer.

```sql
SELECT DATEDIFF(latest_date_column, earliest_date_column) AS days_elapsed
FROM table_name;
```

### Formatting Timestamps into Text Strings
Converts standard internal database date formats into custom, highly readable text layouts.
*   **Definition:** Uses formatting masks (like `%d/%m/%Y`) to build professional, customer-ready text summaries directly out of raw timestamps.

```sql
-- Example using standard MySQL DATE_FORMAT mask syntax
SELECT DATE_FORMAT(timestamp_column, '%d/%m/%Y %r') AS formatted_timestamp
FROM table_name;
```

---

## 2. Temporal Aggregations & Metrics

### Date-Extract Grouping with Multi-Level Counts
Groups database fields dynamically using an extracted chronological attribute while running counts on primary keys.

```sql
SELECT 
    category_dimension,
    YEAR(date_column) AS tracking_year,
    COUNT(primary_key_id) AS total_occurrences
FROM table_name
GROUP BY 
    category_dimension,
    YEAR(date_column);
```

### Relational Intersections with Named Weekday Extraction
Joins primary entities with mapping tables to find the minimum chronological starting point, displaying the day of the week by name (e.g., 'Monday').
*   **Key Function:** `DAYNAME()` extracts the literal calendar name matching a date entry.

```sql
SELECT 
    t1.string_dimension,
    t2.text_title,
    MIN(f.date_column) AS earliest_record_date,
    DAYNAME(MIN(f.date_column)) AS day_of_the_week
FROM main_fact_table f
INNER JOIN dimension_table_1 t1 ON f.dim_1_id = t1.id
INNER JOIN dimension_table_2 t2 ON f.dim_2_id = t2.id
GROUP BY 
    t1.string_dimension,
    t2.text_title
ORDER BY earliest_record_date ASC;
```

### Global Statistical Grouping by Temporal Trends
Combines category indicators with extracted time fields to analyze data quality averages across distinct operational years.

```sql
SELECT 
    category_dimension,
    YEAR(date_column) AS operational_year,
    AVG(numeric_score_column) AS average_performance
FROM table_name
GROUP BY 
    category_dimension,
    YEAR(date_column);
```

---

## 3. Epoch Filters & Advanced Interval Permutations

### Filtering via Unix/Epoch Timestamps
Screens database fields using standard integer Epoch tracking numbers rather than standard calendar strings.
*   **Key Concept:** `FROM_UNIXTIME()` converts universal raw Unix numbers (seconds passed since Jan 1, 1970) into accessible database timestamps.

```sql
SELECT COUNT(*) AS high_boundary_rows
FROM table_name
WHERE date_column > FROM_UNIXTIME(733708800);
```

### Preserving Baseline Rows for Absolute Lifespan Analysis
Uses outer relational joins to find the absolute positive difference between the earliest and latest known entries for specific dimensions, ensuring a positive distance output.
*   **Key Function:** `ABS()` forces a mathematical absolute value configuration, turning potential negative value errors into positive intervals.

```sql
SELECT 
    t1.unique_identifier,
    ABS(DATEDIFF(MAX(f.date_column), MIN(f.date_column))) AS absolute_lifespan_days
FROM dimension_table_1 t1
LEFT JOIN main_fact_table f ON t1.id = f.dim_1_id
GROUP BY t1.unique_identifier
ORDER BY absolute_lifespan_days DESC;
```

---

## 4. Comprehensive Calendar Matrices

### Deep Calendar Property Breakdown
Extracts extensive calendar traits—including corporate quarters, months, numerical weeks, and numerical year-days—while filtering for specific segments.

```sql
SELECT 
    category_dimension,
    text_title,
    QUARTER(date_column) AS chronological_quarter,
    MONTH(date_column) AS chronological_month,
    WEEK(date_column) AS week_of_year,
    DAYOFYEAR(date_column) AS day_of_year
FROM table_name
WHERE QUARTER(date_column) = 1; -- Screens strictly for the first business quarter
```

### Multi-Table Cross-Analysis by Numerical Weekday Indexing
Joins factual data layers on dimensions to evaluate operational load balances (like word tracking or cost metrics) isolated by structural weekday order numbers.
*   **Key Function:** `DAYOFWEEK()` translates regular date values into raw sorted sequence indices (e.g., 1 for Sunday through 7 for Saturday).

```sql
SELECT 
    DAYOFWEEK(f.date_column) AS weekday_index_number,
    SUM(f.tracked_metric_count) AS aggregate_volume_total
FROM main_fact_table f
INNER JOIN dimension_table_1 t1 ON f.dim_1_id = t1.id
GROUP BY DAYOFWEEK(f.date_column);
```
