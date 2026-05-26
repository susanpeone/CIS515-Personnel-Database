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

erDiagram
    EMPLOYEE ||--o{ EMPLOYEE_HISTORY : "accumulates"
    EMPLOYEE ||--o{ SALARY_LOG : "receives"
    EMPLOYEE ||--o{ TRAINING_RECORD : "completes"

    EMPLOYEE {
        int Employee_ID PK
        varchar Emp_First_Name
        varchar Emp_Last_Name
        varchar Phone_Number
        varchar City
        char State
    }

    EMPLOYEE_HISTORY {
        int History_ID PK
        int Employee_ID FK
        int Position_Code FK
        date Effective_Date
        date Expiration_Date
        varchar Status_Flag
    }

    SALARY_LOG {
        int Salary_Log_ID PK
        int Employee_ID FK
        decimal Actual_Salary
        varchar Pay_Reason_Code
        date Effective_Date
        date Expiration_Date
    }

    TRAINING_RECORD {
        int Training_Log_ID PK
        int Employee_ID FK
        int Policy_ID FK
        date Completion_Date
        varchar Certification_Status
        date Recertification_Due_Date
    }

    graph TD
    %% Style adjustments for clarity
    classDef pk fill:#e1f5fe,stroke:#01579b,stroke-width:2px;
    classDef attr fill:#fff,stroke:#37474f,stroke-width:1px;

    subgraph Employee Entity
        E_PK(Employee_ID):::pk --> E_A1(Emp_First_Name):::attr
        E_PK --> E_A2(Emp_Last_Name):::attr
        E_PK --> E_A3(Phone_Number):::attr
        E_PK --> E_A4(City):::attr
        E_PK --> E_A5(State):::attr
    end

    subgraph Job Position Entity
        JP_PK(Position_Code):::pk --> JP_A1(Job_Title):::attr
        JP_PK --> JP_A2(Standard_Base_Salary):::attr
        JP_PK --> JP_A3(Pay_Grade):::attr
    end

    subgraph Regulatory Policy Entity
        RP_PK(Policy_ID):::pk --> RP_A1(Policy_Name):::attr
        RP_PK --> RP_A2(Policy_Authority):::attr
        RP_PK --> RP_A3(Revision_Date):::attr
    end

    subgraph Employee History Junction
        EH_PK(History_ID):::pk --> EH_FK1(Employee_ID FK):::attr
        EH_PK --> EH_FK2(Position_Code FK):::attr
        EH_PK --> EH_A1(Effective_Date):::attr
        EH_PK --> EH_A2(Expiration_Date):::attr
        EH_PK --> EH_A3(Status_Flag):::attr
    end

    subgraph Training Record Junction
        TR_PK(Training_Log_ID):::pk --> TR_FK1(Employee_ID FK):::attr
        TR_PK --> TR_FK2(Policy_ID FK):::attr
        TR_PK --> TR_A1(Completion_Date):::attr
        TR_PK --> TR_A2(Certification_Status):::attr
        TR_PK --> TR_A3(Recertification_Due_Date):::attr
    end
