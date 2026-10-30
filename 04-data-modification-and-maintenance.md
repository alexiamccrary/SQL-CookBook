# Chapter 4: Data Modification & Schema Maintenance (DML)

This section details methods for altering, maintaining, and updating relational database records. Topics include bulk transactions, nested subquery additions, conditional update loops, relational deletion configurations, and programmatic date modification arithmetic.

---

## 1. Foundational Building Blocks (Isolated Concepts)

### Bulk Multi-Row Insertion
Adds multiple new data entries into a targeted table using a single database operation.
*   **Definition:** Grouping multiple values inside parentheses minimizes transaction overhead and speeds up write times.

```sql
INSERT INTO table_name (column_name) 
VALUES 
    ('Value_A'), 
    ('Value_B'), 
    ('Value_C');
```

### Direct Record Deletion
Permanently drops targeted records out of a data table based on standard field evaluations.
*   **Best Practice:** Always verify your target filters before execution to avoid accidentally clearing out production data tables.

```sql
DELETE FROM table_name 
WHERE status_condition = 'Deprecated';
```

### Simple Field Updates
Alters values within specific columns for rows that meet a defined filtering rule.

```sql
UPDATE table_name 
SET column_name = 'New_Value' 
WHERE unique_id = 5;
```

---

## 2. Advanced Multi-Row & Subquery Insertions

### Nested Lookup Subquery Insertion
Inserts new rows while dynamically fetching matching relational structural parent IDs from an alternate lookup table.
*   **Key Concept:** Instead of hardcoding static relational structural key identifiers, inline scalar subqueries trace and match the target parent tables dynamically.

```sql
INSERT INTO child_table (child_name, parent_id_fk, version_label, is_active) 
VALUES 
    ('Item_1', (SELECT id FROM parent_table WHERE name = 'Brand_A'), 'V1.0', TRUE),
    ('Item_2', (SELECT id FROM parent_table WHERE name = 'Brand_A'), 'V1.0', FALSE),
    ('Item_3', (SELECT id FROM parent_table WHERE name = 'Brand_B'), 'V2.1', TRUE);
```

### Explicit Primary Key Sequence Insertion
Forces structural entries into datasets where unique primary sequencing counts must match exact legacy values.

```sql
INSERT INTO security_user_table (user_id, username_string) 
VALUES 
    (1, 'Profile_Alpha'),
    (2, 'Profile_Beta'),
    (3, 'Profile_Gamma');
```

---

## 3. Relational Deletions & Orphan Mitigation

### Subquery Orphan Records Cleanup
Deletes master rows from a table if they lack active connected rows inside a secondary relational lookup tracking grid.
*   **Key Concept:** Clears out old configuration items by ensuring their IDs do not show up inside child association tables.

```sql
DELETE FROM master_asset_table 
WHERE asset_id NOT IN (
    SELECT DISTINCT assigned_asset_id 
    FROM user_assignment_bridge
    WHERE assigned_asset_id IS NOT NULL
);
```

### Relational Join Record Deletion
Deletes logging records or history rows out of database tables by executing exact cross-table inner joins on master account names.

```sql
-- Standard syntax template for join-based transactional deletions
DELETE logging_table
FROM logging_table
INNER JOIN identity_profile_table ON logging_table.account_id = identity_profile_table.id
WHERE identity_profile_table.profile_name = 'Target_User_Name';
```

---

## 4. Conditional Updates & Date Arithmetic Calculations

### Mass Multi-Target Value Updates
Alters data metrics globally across multiple targeted profiles at once using simple evaluation array matrices.
*   **Syntax:** The `IN` expression maps modifications seamlessly across distinct parameter text keys.

```sql
UPDATE customer_credential_table 
SET temporary_access_code = 'FallbackPass123' 
WHERE user_profile_name IN ('User_A', 'User_B', 'User_C');
```

### Dynamic Date Anomaly Resolution
Finds data logging mistakes (like corrupted historical dates) and fixes them automatically by replacing the value with the system's exact current date.

```sql
UPDATE system_audit_log 
SET recording_timestamp = CURRENT_DATE() 
WHERE recording_timestamp > '2099-12-31' OR recording_timestamp IS NULL;
```

### Programmatic Date Interval Addition
Updates temporal history metrics by dynamically calculating and adding an exact length of time (like a business year) to an existing column date.
*   **Key Function:** `DATE_ADD()` or `INTERVAL` arithmetic alters chronological cells safely without creating layout mathematical string errors.

```sql
UPDATE history_tracking_table 
SET tracking_date = DATE_ADD(tracking_date, INTERVAL 1 YEAR) 
WHERE target_employee_name = 'Target_Identifier';
```
