# Chapter 5: Schema Alterations, Constraints, and Automated Triggers

This section details advanced schema management and automation techniques. Topics include structural foreign key constraint configurations, table mutations (`ALTER TABLE`), automated auditing via event triggers (`BEFORE UPDATE` / `BEFORE DELETE`), mass calculation mutations, and database archiving workflows.

---

## 1. Foundational Building Blocks (Isolated Concepts)

### Modifying Schema Structures (ALTER TABLE)
Appends a brand new column and data type to a pre-existing table architecture.
*   **Definition:** Modifies the structural framework of a table without disrupting the existing dataset rows.

```sql
ALTER TABLE table_name 
ADD COLUMN tracking_column_name DATETIME;
```

### Direct Column Value Initialization
Overwrites a single specific structural tracking attribute uniformly across every row within an isolated table.

```sql
UPDATE table_name 
SET tracking_column_name = '2026-01-01 00:00:00';
```

### Basic Event Triggers
Creates a database automation script that intercepts operational statements (like changes or structural deletions) to adjust data before it is saved to the disk.
*   **Key Concept:** The `NEW` keyword represents the incoming dataset modifications before they are written to the database grid.

```sql
CREATE TRIGGER tr_example_before_update
BEFORE UPDATE ON table_name
FOR EACH ROW
BEGIN
    SET NEW.tracking_column_name = NOW();
END;
```

---

## 2. Structural Referential Integrity & Constraints

### Foreign Key Cascade Configurations
Alters an existing child table to add foreign key references, defining exact behavioral cascade triggers when a parent primary key changes or is destroyed.
*   **ON UPDATE CASCADE:** If a parent ID updates, the change flows down automatically to update the child table.
*   **ON DELETE CASCADE:** If a parent row is deleted, all matching child records are automatically destroyed to prevent orphan entries.
*   **RESTRICT / NO ACTION:** Blocks parent deletions or modifications if matching child records exist, protecting core master tables.

```sql
ALTER TABLE child_transaction_table
    ADD CONSTRAINT fk_relational_dimension_1 
        FOREIGN KEY (dimension_1_id) REFERENCES parent_dimension_1(id)
        ON UPDATE CASCADE 
        ON DELETE CASCADE,
    ADD CONSTRAINT fk_master_dimension_2 
        FOREIGN KEY (dimension_2_id) REFERENCES parent_dimension_2(id); 
        -- Defaults to RESTRICT to shield master tables from cascade deletions
```

---

## 3. Automated Auditing & Value Triggers

### Automated State Audit Tracker
Implements a `BEFORE UPDATE` trigger loop that automatically enforces exact temporal stamps and user identity tags whenever a row is modified.
*   **Key Concept:** Automatically manages administrative logging data behind the scenes, eliminating the need for manual inputs during regular application operations.

```sql
DELIMITER \[  CREATE TRIGGER tr_audit_trail_update BEFORE UPDATE ON transactional_fact_table FOR EACH ROW BEGIN SET NEW.last_modified_date = CURRENT_TIMESTAMP(); SET NEW.modifying_user_id = 'SYSTEM_USER_ID'; END\]

DELIMITER ;
```

### Mass Metric Scaling & Aggregate Reporting
Executes a mathematical scaling factor (such as a percentage update) across specific item properties matching a defined text array list.

```sql
-- Step 1: Execute the percentage calculation update across targeted categories
UPDATE transactional_fact_table
SET primary_numeric_price = primary_numeric_price * 1.025
WHERE category_lookup_id IN (
    SELECT id FROM category_dimension WHERE category_name IN ('Brand_A', 'Brand_B', 'Brand_C')
);

-- Step 2: Extract audit records confirming transaction volumes
SELECT 
    d.category_name,
    f.last_modified_date,
    f.modifying_user_id,
    COUNT(*) AS records_adjusted_count
FROM transactional_fact_table f
INNER JOIN category_dimension d ON f.category_lookup_id = d.id
GROUP BY d.category_name, f.last_modified_date, f.modifying_user_id;
```

---

## 4. Archival Frameworks & Deletion Pipelines

### Creating Archival Shadow Tables
Builds dedicated, structural shadow repositories designed to store old, deleted entries for compliance auditing.

```sql
CREATE TABLE historical_archive_table (
    archive_surrogate_id INT AUTO_INCREMENT PRIMARY KEY,
    original_entity_id INT NOT NULL,
    descriptive_label VARCHAR(100),
    deleted_timestamp DATETIME,
    archiving_operator_id VARCHAR(50)
);
```

### Data Archiving Trigger Pipeline
Deploys a `BEFORE DELETE` routine that intercepts row deletions, copying the record parameters into an archival shadow table before the data is removed from the live table.
*   **Key Concept:** The `OLD` indicator allows the trigger script to read original cell parameters right before they are permanently purged.

```sql
DELIMITER \[  CREATE TRIGGER tr_archive_historical_purge BEFORE DELETE ON active_production_table FOR EACH ROW BEGIN INSERT INTO historical_archive_table ( original_entity_id, descriptive_label, deleted_timestamp, archiving_operator_id ) VALUES ( OLD.id, OLD.entity_name, CURRENT_TIMESTAMP(), 'SYSTEM_USER_ID' ); END\]

DELIMITER ;
```

### Multi-Stage Purging Pipeline
Demonstrates how to manually purge target records in a specific sequential order across multiple tables to avoid foreign key errors.

```sql
-- Step 1: Remove dependent operational children first
DELETE FROM transactional_fact_table 
WHERE category_lookup_id = (SELECT id FROM category_dimension WHERE category_name = 'Deprecated_Brand');

-- Step 2: Remove the master parent definition safely now that child rows are clear
DELETE FROM category_dimension 
WHERE category_name = 'Deprecated_Brand';

-- Step 3: Run counts on archival shadow logs to verify background automation worked
SELECT COUNT(*) AS total_archived_records 
FROM historical_archive_table;
```
