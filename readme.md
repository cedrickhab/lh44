# 🎓 PL/SQL FINAL EXAM

## 👤 Identification
- **Name:** Habimana Cedrick  
- **Student ID:** 27443  
- **Project Title:** Small Retail Inventory System  
- **Course:** INSY 8311 - Database Development with PL/SQL  
- **Academic Year:** 2024-2025  
- **Lecturer:** Eric Maniraguha (eric.maniraguha@auca.ac.rw)  

---

## 🚀 Phase I: Problem Statement & Presentation

### 📌 Objective
To identify a real-world issue that requires a **PL/SQL-based Oracle database solution**. The system must involve multiple entities and complex logic suitable for database development and procedural programming.

---

## 💡 Project Summary: Small Retail Inventory System

### 📖 Problem Definition
Small retail stores often struggle with manual inventory tracking. This leads to:
- Overstocking or stockouts
- Inefficiency in restocking
- Inaccurate sales reporting

### 🌍 Context
The system will be deployed in:
- Small grocery stores
- Corner shops
- Local retail stores

It will help automate the management of product stock, sales tracking, and supplier coordination.

### 🎯 Target Users
- Small retail store owners
- Cashiers and shop managers

### 🏆 Project Goals
- 🛒 Automate inventory tracking  
- 📊 Monitor real-time stock levels and sales  
- 🔔 Alert users for low stock  
- 📦 Track suppliers and streamline restocking  
- 📈 Improve overall business operation accuracy  

---

## 🧩 Key Database Entities

| Entity             | Attributes                                                                 |
|--------------------|----------------------------------------------------------------------------|
| **Products**        | `Product_ID`, `Name`, `Price`, `Quantity`                              |
| **Sales**           | `Sale_ID`, `Product_ID`, `Date`, `Quantity_Sold`                           |
| **Suppliers**       | `Supplier_ID`, `Name`, `Contact`, `Created_At`                       |
| **Supplier_Product**| `Supplier_ID`, `Product_ID`, `Supply_Date`                                 |

### 🔗 Relationships
- A **Supplier** can supply **many Products** (M:N)  
- A **Product** can be involved in **many Sales** (1:N)  

---

## 💎 System Benefits
✅ Reduces human errors in stock counting  
✅ Sends low stock alerts automatically  
✅ Tracks supplier-product links for easy replenishment  
✅ Enhances record accuracy with real-time sales logging  
✅ Improves decision-making with reliable inventory data  

---

```mermaid
flowchart TD
  A[🧍 Inventory Manager] -->|Adds Supplier Info| B[📦 Suppliers]
  A -->|Adds New Products| C[🛒 Products]
  B -->|Supplies Stock| C
  D[🧾 Sales Clerk] -->|Records Sale| E[🧾 Sales]
  E -->|Triggers Update| C
  C -->|Check Inventory| F[(🔍 Stock Threshold Monitor)]
  F -->|Low Stock?| G{Is Stock < 10?}
  G -- Yes --> H[⚠️ Notify Manager]
  G -- No --> I[✅ Normal Flow]

  classDef actor fill:#e3f2fd,stroke:#2196f3,stroke-width:2px;
  classDef data fill:#fff3e0,stroke:#fb8c00,stroke-width:2px;
  classDef system fill:#fce4ec,stroke:#d81b60,stroke-width:2px;

  class A,D,H actor
  class B,C,E data
  class F,G,I system
```

---
![phase I](./screenshots/phase%20I.png)

---
---

## 📘 Phase II: Business Process Modeling (MIS)

### 🔍 Scope & Purpose
This phase models the **inventory workflow** from stock monitoring to sales and low-stock alerting. It demonstrates how an **MIS supports decision-making** through automation and accurate data flow.

### 👥 Key Actors

| Role               | Responsibility                             |
|--------------------|---------------------------------------------|
| Inventory Manager  | Monitors stock and updates inventory        |
| Supplier           | Delivers products                           |
| Sales Clerk        | Records sales                               |
| Inventory System   | Maintains stock data, evaluates thresholds  |
| Alert System       | Notifies when stock is low                  |

---

### 🖼️ Process Diagram

✅ **Tools Used:**  
- **Mermaid** (Lightweight Markdown-based modeling)  
- **Draw.io** (Standard BPMN format)  

#### 🔗 Mermaid Diagram  
![Mermaid Diagram](./screenshots/phaseII.png)

<br>

#### 🧩 Draw.io BPMN Diagram  
![Draw.io Diagram](./screenshots/PhaseII.drawio.png)

---

### 🧠 MIS Value & Flow Summary
The diagram starts with the **Inventory Manager** monitoring stock. When a product is low, they receive deliveries from the **Supplier**. The system is then updated. When a **Sales Clerk** records a sale, the inventory is updated again. After each update, the **Inventory System** evaluates whether the stock has fallen below the set threshold. If so, the **Alert System** automatically notifies the manager for restocking.  

This workflow supports MIS by:
- Enabling **real-time decision-making**  
- Reducing **manual effort** through automation  
- **Improving efficiency** in retail operations  

---

### 💻 Mermaid Code Reference

```mermaid
flowchart TD
  start([● Process Start]) --> A1["🧑 Inventory Manager\nMonitor Stock Levels"]
  A1 --> A2["📦 Receive Products"]
  A2 --> B1["🚚 Supplier Delivery"]
  B1 --> C1["🧾 Record Sale"]
  C1 --> A3["📊 Update Inventory"]
  A2 --> A3
  A3 --> D1{{"🔍 Stock < Threshold?"}}
  D1 -- Yes --> E1["❗ Send Alert"]
  D1 -- No --> finish([✅ Process End])
  E1 --> finish

  classDef manager fill:#f9f,stroke:#333;
  classDef supplier fill:#bbf,stroke:#333;
  classDef clerk fill:#9f9,stroke:#333;
  classDef system fill:#f96,stroke:#333;

  class A1,A2,A3 manager
  class B1 supplier
  class C1 clerk
  class D1,E1 system
```

---

## 🧩 Phase III: Logical Model Design

### 🎯 Objective

This project addresses the inventory challenges of small retail stores, including overstocking, stockouts, and poor supplier coordination. The logical model developed in this phase is based on the real-world needs outlined in Phase I and the process workflow modeled in Phase II.

To design a normalized, well-constrained, and relational data model that accurately represents the inventory, sales, and supplier interactions of a small retail business.

---

### 🗃️ Entities & Attributes

#### 📦 Products

| Attribute    | Type         | Constraint                      |
|--------------|--------------|----------------------------------|
| Product_ID   | NUMBER       | Primary Key (Auto-generated)     |
| Name         | VARCHAR(100) | NOT NULL                         |
| Price        | NUMBER(10,2) | NOT NULL, CHECK (Price > 0)      |
| Quantity     | NUMBER       | DEFAULT 0, CHECK (Quantity ≥ 0)  |
| Created_At   | DATE         | DEFAULT SYSDATE                  |

#### 🧾 Sales

| Attribute      | Type       | Constraint                          |
|----------------|------------|-------------------------------------|
| Sale_ID        | NUMBER     | Primary Key (Auto-generated)        |
| Product_ID     | NUMBER     | Foreign Key → Products              |
| Quantity_Sold  | NUMBER     | NOT NULL, CHECK (Quantity_Sold > 0) |
| Sale_Date      | DATE       | DEFAULT SYSDATE                     |

#### 🚚 Suppliers

| Attribute     | Type         | Constraint                       |
|---------------|--------------|----------------------------------|
| Supplier_ID   | NUMBER       | Primary Key (Auto-generated)     |
| Name          | VARCHAR(100) | NOT NULL                         |
| Contact       | VARCHAR(100) | UNIQUE, NOT NULL                 |
| Created_At    | DATE         | DEFAULT SYSDATE                  |

#### 🔗 Supplier_Product (Junction Table)

| Attribute     | Type    | Constraint                             |
|---------------|---------|----------------------------------------|
| Supplier_ID   | NUMBER  | Foreign Key → Suppliers, part of PK     |
| Product_ID    | NUMBER  | Foreign Key → Products, part of PK      |
| Supply_Date   | DATE    | DEFAULT SYSDATE                         |

---

### 🔄 Relationships & Constraints

- 🧩 **Suppliers ↔ Products** — *Many-to-Many* via `Supplier_Product`  
- 📈 **Products → Sales** — *One-to-Many* 
- ✅ Foreign keys ensure data integrity  
- ✅ CHECK constraints enforce data validity  
- ✅ DEFAULT and UNIQUE improve usability and data quality  

---

### 📐 Normalization (3NF Verified)

- ✅ **1NF** – Atomic values only  
- ✅ **2NF** – All attributes fully dependent on PKs  
- ✅ **3NF** – No transitive dependencies  

---

### 🧪 Real-World Scenario Coverage

| Scenario                                         | Supported |
|--------------------------------------------------|-----------|
| Track products with current stock levels         | ✅        |
| Record every sale tied to a product              | ✅        |
| Handle suppliers offering multiple products      | ✅        |
| Prevent invalid sales (e.g., nonexistent product)| ✅        |
| Ensure valid pricing and quantities              | ✅        |
| Avoid duplicate supplier contact info            | ✅        |

---

### 🖼️ ERD Diagram

> 🟩 **Visual Placeholder: Logical Model ERD**  
> 👉 *This image illustrates the tables, keys, and relationships defined above via Mermaid chart and also SQL developer.*

![ERD - Logical Model](./screenshots/ERD%201.png)

<br>

![ERD - Logical Model](./screenshots/ERD%202.png)

---

### 💻 SQL Script Location

> 📁 [`/sql/create_logical_model.sql`](./sql/create_logical_model.sql)  
> Contains complete code for all table creation, constraints, and relationships.

---
---

## 🏗️ Phase IV: Database Creation and Access Setup (via SQL Developer)

### 🎯 Objective

To create a dedicated Oracle PL/SQL database environment using SQL Developer as an **alternative to Oracle Enterprise Manager (OEM)**. This setup provides full access control and prepares the development environment for Phase V.

---

### 🔐 Task 1: PDB and User Creation (SQL Developer)

The user and development environment were set up inside an Oracle **Pluggable Database (PDB)** using **SQL Developer**, which allows graphical interaction and full administrative capabilities.

---

### 🧰 Configuration Summary

| Component             | Value                                  |
|------------------------|----------------------------------------|
| **Tool Used**         | SQL Developer (OEM Alternative)         |
| **PDB Name**          | `wed_27443_cedrick_Retail_db`           |
| **User Created**      | `cedrick27443`                          |
| **Password**          | `cedrick`                               |
| **Privileges Granted**| Full DBA privileges                     |

---

### 📸 Screenshot: PDB Creation in SQL Developer

![PDB Creation](./screenshots/pdb.png)

---
<br>

### 📸 Screenshot: User Created & Privileges Granted

![Privileges](./screenshots/privileges.png)

```sql
-- SQL run inside SQL plus

alter session set container=WED_27443_Cedrick_Retail_DB;
CREATE USER cedrick27443 IDENTIFIED BY cedrick;
GRANT all privileges to cedrick27443;
GRANT SYSDBA to cedrick27443;
```
---
---
<!-- OEM Screenshot -->
![OEM](./screenshots/OEM.png)
---

---
<!-- OEM 1 Screenshot -->
![OEM 1](./screenshots/OEM%2027443.png)
---

---
<!-- OEM 2 Screenshot -->
![OEM 2](./screenshots/OEM%20act.png)
---


---

---

### 📸 Screenshot: User Successfully Connected to PDB in SQL Developer

![PDB User Connection](./screenshots/conn%20db.png)

---

### ✅ Summary

| Step                             | Completed |
|----------------------------------|-----------|
| PDB created                      | ✅        |
| Project user created             | ✅        |
| Password set to first name       | ✅        |
| DBA privileges granted           | ✅        |
| SQL Developer used as OEM alt    | ✅        |
| Screenshots taken and stored     | ✅        |

---
---

## 🧱 Phase V: Table Implementation and Data Insertion

### 🎯 Objective

To implement the physical database structure based on the logical model and insert meaningful, testable data. This phase ensures structural integrity, accurate constraints, and realistic data to support business operations and future PL/SQL programming.

---

### 🔨 Step 1: Table Creation

✅ The following tables were created in the schema `WED_27443_Cedrick_Retail_DB` using SQL Developer:

---

#### 🧱 Table: Suppliers

![Suppliers Table Created](./screenshots/supplier%20created.png)

---

#### 🧱 Table: Products

![Products Table Created](./screenshots/prod%20created.png)

---

#### 🧱 Table: Sales

![Sales Table Created](./screenshots/sales%20created.png)

---

#### 🧱 Table: Supplier_Product

![Supplier_Product Table Created](./screenshots/sup_pro%20created.png)

---

### 📥 Step 2: Data Insertion

Realistic data entries were inserted for each table to reflect meaningful retail operations.

---

#### 🗃️ Insertion: Suppliers

![Suppliers Data Inserted](./screenshots/supp%20data.png)

---

#### 🗃️ Insertion: Products

![Products Data Inserted](./screenshots/pro%20data.png)

---

#### 🗃️ Insertion: Sales

![Sales Data Inserted](./screenshots/sales%20data.png)

---

#### 🗃️ Insertion: Supplier_Product

![Supplier_Product Data Inserted](./screenshots/sup_pro%20data.png) 

---

### 🔍 Step 3: Data Integrity Validation

A join query was executed to validate relationships and ensure referential integrity.

> ✅ Result confirmed that:
- All foreign keys work as expected
- Many-to-many and one-to-many relationships are intact
- Data is consistent and logically connected

![Query Output](./screenshots/data%20integrity.png)

---

### 🛡️ Step 4: Constraints and Integrity

| Constraint       | Applied To             | Type                  |
|------------------|------------------------|------------------------|
| `PRIMARY KEY`    | All 4 tables           | Uniquely identifies rows |
| `FOREIGN KEY`    | Sales, Supplier_Product| Enforces referential integrity |
| `NOT NULL`       | Most attributes        | Prevents null violations |
| `UNIQUE`         | Suppliers.Contact      | Prevents duplicate entries |
| `CHECK`          | Product price/quantity| Validates acceptable values |
| `DEFAULT`        | Created_At, Supply_Date| Auto-fills dates       |

---

### ✅ Summary

| Deliverable              | Status |
|---------------------------|--------|
| Physical table creation   | ✅     |
| Data inserted             | ✅     |
| Data integrity validated  | ✅     |
| Constraints applied       | ✅     |
| Screenshots added         | ✅     |

---

---

## 🔧 Phase VI: PL/SQL Programming (Procedures, Functions, Triggers, Packages)

### 🎯 Objective

To implement business logic directly within the Oracle database using PL/SQL. This includes **automating operations**, **analyzing data**, and **ensuring reliability** through procedures, functions, triggers, and packages.

---

## 🧱 Database Operations

### 🔁 DML Operations
- `INSERT`, `UPDATE`, `DELETE` queries were used to manipulate table data.
- Example: Updating product quantities, deleting old supplier entries, and inserting new sales.
---
![DML](./screenshots/DML.png)

---

### 🧩 DDL Operations
- `CREATE`, `ALTER`, and `DROP` commands were executed to structure the database.
- Example: Added new columns like `Last_Updated` and modified constraints during testing.
---
![DDL](./screenshots/DDL.png)

---
## 💡 Simple Analytics Problem Statement

> “Analyze how many times each product has been sold to help determine restocking priorities.”

This was implemented using a **Window Function** on the `Sales` table to track total sales per product.

```sql
SELECT 
    p.Product_Name,
    s.Product_ID,
    SUM(s.Quantity_Sold) OVER (PARTITION BY s.Product_ID) AS Total_Sold
FROM Sales s
JOIN Products p ON s.Product_ID = p.Product_ID;
```
---
![Problem statement](./screenshots/problem%20statement.png)

---

## 🛠️ PL/SQL Components

### ✅ Procedure: `register_sale`

- Records a new sale
- Automatically decreases stock quantity
- Warns if stock is low
- Retrieved data using cursors


---
![Procedure](./screenshots/procedures.png)

---
---
![Procedure 1](./screenshots/procedures%201.png)

---
---
#####  !!Cursors:
-data is now retrieved because of cursors

![cursors procedure](./screenshots/procedures.png)

---





## 🧪 Testing:
All PL/SQL components were **individually tested** using anonymous blocks and verified with realistic test data.

---
### ✅ Function-Quantity-Testing: `get_stock_level`

- Returns the current quantity of a specified product
- Useful for on-demand stock checks

![Function](./screenshots/TEST%20.png)


---
### ✅ Trigger-Products-Testing:  `trigger_stock_alert`

- Fires automatically after a product’s quantity is updated
- Alerts if quantity falls below threshold

![Trigger](./screenshots/TEST%201%20ON%20products.png)

---
## 📦 PL/SQL Package: `inventory_pkg`

This package encapsulates reusable logic:
- `register_sale` procedure
- `get_stock_level` function

**Benefits:**
- Modular code structure
- Reusability and better organization
---
![Package spec](./screenshots/pack%20spec.png)

---
---
![Package body 1](./screenshots/package%20bod1.png)

---
---
![Package body 2](./screenshots/package%20bod%202.png)

---



---

### ✅ Package-Testing

![Package completed](./screenshots/pack%20spec,bod%20used%20completed.png)

---

## ✅ Summary of Deliverables

| Task                           | Completed |
|--------------------------------|-----------|
| DDL / DML Commands             | ✅        |
| Simple Analytics Query         | ✅        |
| Procedure                      | ✅        |
| Function                       | ✅        |
| Cursor Use (in Procedure)      | ✅        |
| Exception Handling             | ✅        |
| Trigger                        | ✅        |
| Package with Reusable Logic    | ✅        |
| Testing + Screenshots          | ✅        |

---


