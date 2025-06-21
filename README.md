# Automobile Dealership Management System

A comprehensive database project simulating real-time operations of an automobile dealership using Oracle SQL.

## 🚗 Project Overview

This project demonstrates a complete automobile dealership management system with structured database design, complex query implementations, and real-world business logic scenarios.

## ✨ Features

- *Sales Management*: Complete sales tracking with customer information and payment modes
- *Inventory Control*: Stock management with supplier relationships
- *Service Operations*: Vehicle service tracking and maintenance records
- *Test Drive Management*: Customer test drive scheduling and feedback
- *Employee Management*: Staff information and sales quota tracking
- *EMI Financing*: Financial analysis and payment tracking
- *Analytics*: Comprehensive reporting and insights

## 🗄 Database Schema

### Tables Structure

sql
-- Car Information
Car(Car_id, d_id, Stock_id, year, VIN, color, enginetype, mileage, price, status)

-- Customer Management
Customer(Cust_id, F_name, L_name, Phno, Email, address, DOB, Gender)

-- Supplier Network
Supplier(S_id, Stock_id, Company_id, S_name, Phno, Email, Address)

-- Inventory Stock
Stock(Stock_id, S_id, Name, Description, Quantity, Price)

-- Sales Transactions
Sales(Sale_id, d_id, car_id, cust_id, sale_date, pay_mode, price, warranty)

-- Staff Management
Employee(emp_id, First_name, Last_name, phone_number, email, hire_date, Salary, job_title, dept, sale_quota)

-- Service Records
Service(Service_id, Car_id, Cust_id, Emp_id, Service_type, Cost, S_checkout, S_checkin)

-- Test Drive Management
Testdrive(TD_ID, cust_id, car_id, emp_id, date, feedback)

-- Dealership Information
Dealership(D_id, D_name, Emp_id, Phno, Location)


### Entity Relationship

- *Car* belongs to *Dealership* and references *Stock*
- *Sales* connects *Customer, **Car, and **Dealership*
- *Service* links *Car, **Customer, and **Employee*
- *Testdrive* associates *Customer, **Car, and **Employee*
- *Stock* is managed by *Supplier*
- *Employee* works at *Dealership*

## 🔧 Technical Implementation

### Technologies Used
- *Database*: Oracle SQL
- *Query Types*: 
  - Basic SELECT operations
  - Complex JOINs (INNER, LEFT, RIGHT, FULL OUTER)
  - Subqueries and Correlated subqueries
  - Aggregate functions and GROUP BY
  - Window functions
  - CTEs (Common Table Expressions)
  - PL/SQL procedures and functions

### Key Features Implemented
- ✅ 50+ SQL queries from basic to advanced
- ✅ 10+ PL/SQL procedures and functions
- ✅ Sales analytics and reporting
- ✅ Customer insights and segmentation
- ✅ Service analytics and tracking
- ✅ EMI financing calculations
- ✅ Inventory management queries
- ✅ Performance optimization

## 📊 Query Categories

1. *Basic Queries* - Simple SELECT, WHERE, ORDER BY
2. *Intermediate Queries* - JOINs, GROUP BY, HAVING
3. *Advanced Queries* - Subqueries, Window functions, CTEs
4. *Analytics Queries* - Business intelligence and reporting
5. *PL/SQL Operations* - Stored procedures, functions, triggers

## 🚀 Getting Started

### Prerequisites
- Oracle Database (11g or higher)
- SQL Developer or similar database client

### Installation
1. Clone the repository
2. Execute the schema creation scripts
3. Load sample data
4. Run the query examples

## 📈 Business Use Cases

- *Sales Performance Analysis*: Track sales trends, top-performing employees
- *Customer Management*: Customer segmentation, purchase history
- *Inventory Optimization*: Stock levels, supplier performance
- *Service Operations*: Maintenance scheduling, service revenue
- *Financial Analysis*: EMI calculations, revenue reporting



## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues and enhancement requests.


## 👨‍💻 Author

*RISAPATIL*
- GitHub: [@your-RISAPATIL](https://github.com/RISAPATIL)
- Email: risa2005patil@email.com

---
This project demonstrates advanced SQL concepts and real-world database design for automobile dealership management.
