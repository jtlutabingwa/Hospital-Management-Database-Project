# 🏥 Hospital Management Database System Project

### **By:** Jonathan Lutabingwa, Austin Hayes, Kyle Lunsford  
**Course:** Database Design and Implementation - ITCS 3160  
**Instructor:** Azhar Ahmed  
**Date:** May 2, 2025  

---

## 📘 Project Overview

### **Description**
This project presents a **Relational Database System for Hospital Management**, designed to efficiently store and manage data related to patients, doctors, billing, procedures, and insurance.  

The system utilizes **SQL** to build, maintain, and query structured data. Relationships between entities ensure data integrity, normalization, and accessibility for hospital operations.

---

### **Objectives**
- Develop a **normalized relational database** for a hospital system.  
- Insert, update, and query data efficiently using **SQL**.  
- Implement relational connections between entities for **real-world functionality**.

---

### **Scope**
The database defines:
- **Patients**
- **Doctors**
- **Departments**
- **Procedures**
- **Billing Information**
- **Insurance Providers**

Each entity is interconnected through **foreign key relationships**, creating a fully relational model.  
The design includes **5 definition tables** and **1 transactional table**.

---

## 📋 Requirements Gathering

### **Data Requirements**
Main entities include:
- **Patient**
- **Doctor**
- **Procedures**
- **Department**
- **Billing Information**
- **Insurance Providers**

---

### **Functional Requirements**
To ensure referential integrity:
- Departments must exist before assigning doctors.  
- Insurance providers must exist before assigning patients or billing records.  
- Billing records require valid patients, doctors, and procedures.  

Each **Patient ID** links to **Billing Information**, which connects to the **Doctor**, **Procedure**, and **Insurance Provider**.

---

### **Design Summary**
#### **Entities**
- **Patients:** Stores patient details and insurance information.  
- **Doctors:** Contains physician details, license number, and assigned department.  
- **Departments:** Defines departments, contact info, and bed capacities.  
- **Billing_Information:** Links patients, doctors, procedures, and insurance for billing.  
- **Procedures:** Stores medical procedure details (name, duration, cost).  
- **Insurance_Providers:** Includes provider contact info and plan types.

---

## 🔐 Security Measures

### 1. **Principle of Least Privilege**
Each user role is restricted to only what’s necessary:

| Role | Permissions |
|------|--------------|
| Admin | Full access |
| Doctor | Read/write assigned patients |
| Nurse | Read-only, limited write (logs) |
| Billing Staff | Billing & demographics only |
| Receptionist | Add patients, no billing/diagnosis access |

---

### 2. **Data Encryption**
- **Encryption at Rest:** Protects data stored on disk.  
- **Field Encryption:** Secures sensitive fields like phone numbers or billing info.

---

### 3. **Audit Logging**
- Tracks all access and modifications.  
- Detects unauthorized data operations.

---

### 4. **Backups & Recovery**
- **Regular backups:** Daily, weekly, and offsite/cloud storage.  
- **Disaster recovery:** Includes documented recovery procedures and testing drills.

---

## 🧩 Conceptual Design (ER Diagram)

*The ER diagram is included in the separate PDF submission.*

### **Entity Relationships Overview**
- Departments → Doctors (1:M)  
- Doctors → Billing_Information (1:M)  
- Patients → Billing_Information (1:M)  
- Procedures → Billing_Information (1:M)  
- Insurance_Providers → Billing_Information (1:M)  

---

## 🧠 Logical Design (Relational Model)

```sql
Patient (
  patient_id VARCHAR(11) PRIMARY KEY,
  first_name VARCHAR(50),
  last_name VARCHAR(50),
  age INT,
  birth_date DATE,
  insurance_id VARCHAR(11) FOREIGN KEY,
  insurance_group_id VARCHAR(11),
  address1 VARCHAR(50),
  address2 VARCHAR(50),
  city VARCHAR(50),
  state_of_residency VARCHAR(2),
  zip4 VARCHAR(10),
  phone_number VARCHAR(12) UNIQUE
);

Doctor (
  doctor_id VARCHAR(11) PRIMARY KEY,
  first_name VARCHAR(50),
  last_name VARCHAR(50),
  license_number VARCHAR(25) UNIQUE,
  department_id VARCHAR(11) FOREIGN KEY
);

Department (
  department_id VARCHAR(11) PRIMARY KEY,
  department_name VARCHAR(50),
  email_address VARCHAR(100),
  phone_number VARCHAR(12) UNIQUE,
  bed_capacity INT
);


Procedures (
  procedure_id VARCHAR(11) PRIMARY KEY,
  procedure_name VARCHAR(100),
  description VARCHAR(2000),
  procedure_length_in_minutes INT,
  cost NUMERIC(10,2)
);

Insurance_Providers (
  insurance_id VARCHAR(11) PRIMARY KEY,
  insurance_provider VARCHAR(100),
  phone_number VARCHAR(12),
  fax_number VARCHAR(12),
  plan_type VARCHAR(50)
);


Billing_Information (
  billing_id VARCHAR(11) PRIMARY KEY,
  patient_id VARCHAR(11) FOREIGN KEY,
  doctor_id VARCHAR(11) FOREIGN KEY,
  procedure_id VARCHAR(11) FOREIGN KEY,
  billing_date DATE,
  insurance_id VARCHAR(11) FOREIGN KEY
);
```

## 💾 Physical Design (Implementation)

The Physical Design translates the logical model into executable SQL code that can be implemented in a relational database management system (RDBMS).
It includes table creation, data types, constraints, and foreign key relationships to ensure referential integrity.

```sql
Insurance Providers
CREATE TABLE Insurance_Providers (
  insurance_id VARCHAR(11) PRIMARY KEY,
  insurance_provider VARCHAR(50),
  phone_number VARCHAR(12) UNIQUE,
  fax_number VARCHAR(12) UNIQUE,
  plan_type VARCHAR(50)
);

Patient
CREATE TABLE Patient (
  patient_id VARCHAR(11) PRIMARY KEY, 
  first_name VARCHAR(50),
  last_name VARCHAR(50),
  birth_date DATE,
  insurance_id VARCHAR(11),
  insurance_group_id VARCHAR(11),
  address1 VARCHAR(50),
  address2 VARCHAR(50),
  city VARCHAR(50),
  state_of_residency VARCHAR(2),
  zip4 VARCHAR(10),
  phone_number VARCHAR(12) UNIQUE,
  FOREIGN KEY (insurance_id) REFERENCES Insurance_Providers(insurance_id)
);

Department
CREATE TABLE Department (
  department_id VARCHAR(11) PRIMARY KEY,
  department_name VARCHAR(50),
  phone_number VARCHAR(12) UNIQUE,
  email_address VARCHAR(100),
  bed_capacity INT
);

Doctor
CREATE TABLE Doctor (
  doctor_id VARCHAR(11) PRIMARY KEY,
  first_Name VARCHAR(50),
  last_Name VARCHAR(50),
  licence_number VARCHAR(25) UNIQUE,
  department_id VARCHAR(11),
  FOREIGN KEY (department_id) REFERENCES Department(department_id)
);

Procedures
CREATE TABLE Procedures (
  procedure_id VARCHAR(11) PRIMARY KEY,
  procedure_name VARCHAR(100),
  description VARCHAR(2000),
  procedure_length_in_minutes INT,
  cost NUMERIC(10,2)
);

Billing Information
CREATE TABLE Billing_Information (
  billing_id VARCHAR(11) PRIMARY KEY,
  patient_id VARCHAR(11),
  doctor_id VARCHAR(11),
  procedure_id VARCHAR(11),
  billing_date DATE,
  insurance_id VARCHAR(11),
  FOREIGN KEY (patient_id) REFERENCES Patient(patient_id),
  FOREIGN KEY (doctor_id) REFERENCES Doctor(doctor_id),
  FOREIGN KEY (procedure_id) REFERENCES Procedures(procedure_id),
  FOREIGN KEY (insurance_id) REFERENCES Insurance_Providers(insurance_id)
);
```

## Data Population

Each table is populated with 30 entries representing real-world hospital data, including:

30 Patients

30 Doctors

30 Billing Records

30 Procedures

30 Insurance Providers

These entries were inserted using INSERT INTO statements to simulate realistic hospital operations and relationships.

🧮 Example Queries
Join Example #1 – Patient Billing & Insurance
```sql
SELECT 
  p.first_name AS patient_first_name,
  p.last_name AS patient_last_name,
  bi.billing_date,
  ip.insurance_provider,
  ip.plan_type,
  bi.billing_id
FROM Billing_Information bi
JOIN Patient p ON bi.patient_id = p.patient_id
JOIN Insurance_Providers ip ON bi.insurance_id = ip.insurance_id;
```

✅ Output: 30 rows — verified integrity across all relationships.
```sql
Join Example #2 – Doctors and Departments
SELECT 
  d.doctor_id,
  d.first_name AS doctor_first_name,
  d.last_name AS doctor_last_name,
  d.licence_number,
  dept.department_name
FROM Doctor d
LEFT JOIN Department dept 
ON d.department_id = dept.department_id
ORDER BY d.doctor_id ASC;
```

✅ Output: All doctors successfully mapped to their departments.

```sql
Join Example #3 – Comprehensive Multi-Table Join
SELECT 
  p.patient_id,
  p.first_name AS patient_first_name,
  p.last_name AS patient_last_name,
  pr.procedure_name,
  pr.cost AS procedure_cost,
  d.first_name AS doctor_first_name,
  d.last_name AS doctor_last_name,
  ip.insurance_provider,
  bi.billing_date
FROM Billing_Information bi
INNER JOIN Patient p ON bi.patient_id = p.patient_id
INNER JOIN Procedures pr ON bi.procedure_id = pr.procedure_id
INNER JOIN Doctor d ON bi.doctor_id = d.doctor_id
LEFT JOIN Insurance_Providers ip ON bi.insurance_id = ip.insurance_id
ORDER BY bi.billing_date DESC;
```

✅ Output: 30 rows — complete and consistent results.

## 🧾 Summary

The Hospital Management Database System is a comprehensive and secure solution for managing hospital operations through an efficient relational database.

Key Achievements

Established data normalization and referential integrity across all entities.

Implemented 30+ entries per table to simulate real-world hospital activity.

Demonstrated multi-table joins for insights into patients, procedures, doctors, and insurance data.

Integrated security principles such as least privilege and encryption.

Created backup and recovery strategies to ensure reliability and data protection.

## Scalability

This database can be expanded with additional entities like:

Staff Scheduling

Pharmacy Management

Patient Diagnostics

Inventory Tracking

The system’s structure allows for easy modification, additional constraints, and adaptation for larger-scale hospital environments.
