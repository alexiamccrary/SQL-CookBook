# Advanced SQL Syntax & Database Design Cookbook

A professional, production-ready reference guide charting my progression from fundamental data manipulation to advanced schema orchestration, temporal analysis, and automated database engineering. 

This repository functions as a comprehensive SQL cookbook, demonstrating core database competencies using standardized engineering patterns, clear architectural definitions, and optimized query strategies.

---

## 📁 Repository Roadmap

### [Chapter 1: Foundational Queries & Data Filtering](./01-basic-queries.md)
*   **Core Concepts:** Data projections using the asterisk (`*`) wildcard, column isolation, and result set limitations.
*   **Filtering & Logic:** Duplicate row removal via `DISTINCT`, partial sub-string evaluation utilizing `LIKE` wildcards (`%`), logical evaluation boundaries (`BETWEEN`), and multi-column sorting constraints (`ORDER BY ASC/DESC`).

### [Chapter 2: Relational Joins, Aggregations, and Subqueries](./02-advanced-queries-and-joins.md)
*   **Table Joins:** Baseline `INNER JOIN` execution, data preservation matrices using `LEFT OUTER JOIN`, and comprehensive star-schema master connections.
*   **Aggregations & Grouping:** Global scalar functions (`AVG`, `SUM`, `MIN`, `MAX`, `COUNT`), multi-level bucket grouping (`GROUP BY`), and post-aggregation filtering utilizing the `HAVING` clause.
*   **String & Subquery Logic:** Dynamic text compilation using `CONCAT()` and execution flows for independent nested scalar subqueries.

### [Chapter 3: Temporal Analysis & Date/Time Manipulation](./03-temporal-analysis-and-dates.md)
*   **Date Operations:** Individual time element extractions (`YEAR`, `MONTH`, `DAY`, `QUARTER`, `WEEK`), day-name string rendering via `DAYNAME()`, and sorting by sequential weekday indexing.
*   **Intervals & Timestamps:** Calculating absolute calendar spans using `DATEDIFF()`, Unix/Epoch time conversions via `FROM_UNIXTIME()`, and custom output layout masks with `DATE_FORMAT()`.

### [Chapter 4: Data Modification & Schema Maintenance (DML)](./04-data-modification-and-maintenance.md)
*   **DML Operations:** Optimized bulk multi-row `INSERT` blocks, conditional multi-target `UPDATE` sweeps, and verified `DELETE` statements.
*   **Advanced Maintenance:** Nested parent ID lookups within insert arrays, subquery-driven orphan record cleanups, join-based log clearing, date anomaly corrections, and programmatic `DATE_ADD()` interval arithmetic.

### [Chapter 5: Schema Alterations, Constraints, and Automated Triggers](./05-schema-alterations-and-triggers.md)
*   **Schema Evolution:** Table structural modifications using `ALTER TABLE`, field initializations, and generating compliance shadow repositories.
*   **Referential Integrity:** Enforcing operational data rules through strict foreign key constraints, balancing `ON UPDATE CASCADE` / `ON DELETE CASCADE` actions against `RESTRICT` rules.
*   **Database Automation:** Building server-side procedural event triggers (`BEFORE UPDATE` and `BEFORE DELETE`) using `NEW` and `OLD` records to automate audit logging trails and transactional data archiving pipelines.

### ⏳ Future Modules (In Progress)
*   **Chapter 6:** *[Topic Coming Soon]*
*   **Chapter 7:** *[Topic Coming Soon]*
*   **Chapter 8:** *[Topic Coming Soon]*

---

## 🛠️ Tech Stack & Standards Demonstrated
*   **SQL Dialects:** ANSI SQL / MySQL Compliant Syntax
*   **Concepts:** Database Normalization (1NF - 5NF), Referential Integrity, Star-Schema Architecture, DML/DDL Execution, Server-Side Automation (Triggers).
*   **Documentation:** GitHub Markdown Optimization & System Design Mapping.

---

*This cookbook is actively maintained as I progress through advanced database architecture patterns. Feel free to explore the chapter logs to review structural code scripts and execution patterns.*
