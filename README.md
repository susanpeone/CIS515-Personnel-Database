# CIS515-Personnel-Database
Strategic 3NF personnel database schema and deployment tracking.

# Enterprise Personnel Database System (CIS515)

## Project Overview
This repository contains the physical relational schema and logical modeling plan designed for a government human resources agency personnel system. The data model is fully optimized to Third Normal Form (3NF) to eliminate structural redundancies and operational data anomalies while facilitating fast, ad hoc departmental reporting.

## Core Entities (3NF Normalized)
1. **Employee:** Houses core biographical identifiers (Primary Key: `Employee_ID`).
2. **Job_Position:** Defines structural roles, pay grades, and base salaries independently of personnel.
3. **Employee_History:** An associative junction table tracking career progression, promotions, and lateral movements using time-variant temporal boundaries (`Effective_Date`, `Expiration_Date`).
4. **Regulatory_Policy:** Stores operational rules, legal mandates, and compliance baselines.
5. **Training_Record:** An associative junction tracking individual employee course completions and certification expiration tracking.

## Time-Variant Data Management
To fulfill strict public-sector auditing requirements, all historical tracking tables utilize explicit composite temporal components (`Effective_Date` and `Expiration_Date`). Instead of overwriting records during an update (such as a salary increase or training recertification), active records are archived with a historical timestamp, and a new row is appended. This establishes a non-repudiable, defensible audit trail.
