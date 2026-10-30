# SQL Query Fundamentals: Reference Guide

A structured reference guide outlining core SQL syntax patterns, operation workflows, and standard data-retrieval techniques.

---

## 1. Data Retrieval Foundations

### Select All Columns
Retrieves every column and row present within a targeted table. 
*   **Syntax:** Use the asterisk (`*`) wildcard operator.
*   **Best Practice:** Minimize use in production environments to avoid unnecessary network and memory overhead.

```sql
SELECT * 
FROM table_name;
```

### Column Projection
Extracts specific, named fields rather than pulling the entire row structure.
*   **Syntax:** Separate target column names with commas.

```sql
SELECT 
    column_name_1, 
    column_name_2 
FROM table_name;
```

---

## 2. Row Filtering & Pattern Matching

### Distinct Value Isolation
Filters out duplicate row values from your results, returning only unique records or unique permutations.
*   **Syntax:** Place `DISTINCT` immediately following the `SELECT` keyword.

```sql
SELECT DISTINCT column_name 
FROM table_name;
```

### Text Pattern Matching (Wildcards)
Filters records based on partial text patterns rather than exact matches.
*   **Syntax:** Used with the `LIKE` operator and the percent (`%`) wildcard character. 
*   **Mechanics:** `%string%` matches any value containing the search term anywhere inside the field.

```sql
SELECT DISTINCT column_name 
FROM table_name 
WHERE column_name LIKE '%search_term%';
```

---

## 3. Advanced Filtering & Sorting Constraints

### Evaluative Row Comparisons
Filters data by comparing values across two different columns within the same row.
*   **Syntax:** Define comparison constraints (`>`, `<`, `=`, `!=`) inside the `WHERE` clause.

```sql
SELECT * 
FROM table_name 
WHERE column_name_1 > column_name_2;
```

### Range Evaluation
Filters datasets to isolate values falling within a specific upper and lower limit boundary.
*   **Syntax:** The `BETWEEN` operator evaluates ranges **inclusively** (including the boundary numbers themselves).

```sql
SELECT * 
FROM table_name 
WHERE column_name BETWEEN lower_bound AND upper_bound;
```

---

## 4. Result Sorting & Multi-Column Ordering

### Standard Result Ordering
Sorts the output rows based on a specific attribute column.
*   **Syntax:** Appended using `ORDER BY`. Use `ASC` for ascending order (default) or `DESC` for descending order.

```sql
SELECT * 
FROM table_name 
ORDER BY column_name DESC;
```

### Multi-Layered Column Sorting
Arranges data across multiple levels, sorting by a secondary column when the primary sorting column contains identical duplicate values.
*   **Syntax:** List sorting priorities sequentially, separated by commas. Each column can have its own independent directional rule.

```sql
SELECT DISTINCT 
    column_name_1, 
    column_name_2 
FROM table_name 
ORDER BY 
    column_name_1 ASC, 
    column_name_2 DESC;
```
