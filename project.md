# Automobile Dealership Management - SQL Queries & Operations

This document contains comprehensive SQL queries and PL/SQL operations for the Automobile Dealership Management System.

## 📋 Table of Contents
1. [Basic Queries (1-15)](#basic-queries)
2. [Intermediate Queries (16-30)](#intermediate-queries)
3. [Advanced Queries (31-50)](#advanced-queries)
4. [PL/SQL Operations (51-60)](#plsql-operations)

---

## Basic Queries

### 1. Display All Cars
**Description**: Retrieve all cars in the inventory
```sql
SELECT * FROM Car;
```
**Sample Output**:
```
CAR_ID  D_ID  STOCK_ID  YEAR  VIN           COLOR   ENGINETYPE  MILEAGE  PRICE    STATUS
------  ----  --------  ----  -----------   -----   ----------  -------  -------  --------
C001    D01   S001      2023  1A2B3C4D5E6   Red     V6          25       45000    Available
C002    D01   S002      2022  2B3C4D5E6F7   Blue    V8          30       52000    Sold
```

### 2. List All Customers
**Description**: Show customer information
```sql
SELECT Cust_id, F_name, L_name, Email, Phno FROM Customer;
```

### 3. Find Cars by Year
**Description**: Get all cars manufactured in a specific year
```sql
SELECT * FROM Car WHERE year = 2023;
```

### 4. Customers by Gender
**Description**: Count customers by gender
```sql
SELECT Gender, COUNT(*) as Total_Customers 
FROM Customer 
GROUP BY Gender;
```
**Sample Output**:
```
GENDER  TOTAL_CUSTOMERS
------  ---------------
Male    45
Female  38
```

### 5. Available Cars Only
**Description**: Show only available cars for sale
```sql
SELECT Car_id, year, color, price 
FROM Car 
WHERE status = 'Available';
```

### 6. Expensive Cars
**Description**: List cars priced above $50,000
```sql
SELECT Car_id, year, color, price 
FROM Car 
WHERE price > 50000 
ORDER BY price DESC;
```

### 7. Recent Sales
**Description**: Show sales from the last 30 days
```sql
SELECT * FROM Sales 
WHERE sale_date >= SYSDATE - 30;
```

### 8. Employee Contact Info
**Description**: Display employee names and contact information
```sql
SELECT emp_id, First_name, Last_name, phone_number, email 
FROM Employee;
```

### 9. Service Costs Above Average
**Description**: Find services with costs above average
```sql
SELECT Service_id, Service_type, Cost 
FROM Service 
WHERE Cost > (SELECT AVG(Cost) FROM Service);
```

### 10. Cars by Engine Type
**Description**: Group cars by engine type
```sql
SELECT enginetype, COUNT(*) as Car_Count 
FROM Car 
GROUP BY enginetype;
```

### 11. Customer Age Calculation
**Description**: Calculate customer ages
```sql
SELECT Cust_id, F_name, L_name, 
       FLOOR(MONTHS_BETWEEN(SYSDATE, DOB)/12) as Age 
FROM Customer;
```

### 12. Top 5 Expensive Cars
**Description**: Show the 5 most expensive cars
```sql
SELECT * FROM (
    SELECT * FROM Car 
    ORDER BY price DESC
) WHERE ROWNUM <= 5;
```

### 13. Suppliers by Location
**Description**: List suppliers with their addresses
```sql
SELECT S_name, Company_id, Address 
FROM Supplier 
ORDER BY S_name;
```

### 14. Sales Payment Methods
**Description**: Count sales by payment mode
```sql
SELECT pay_mode, COUNT(*) as Transaction_Count 
FROM Sales 
GROUP BY pay_mode;
```

### 15. Low Mileage Cars
**Description**: Find cars with excellent fuel efficiency
```sql
SELECT Car_id, year, color, mileage, price 
FROM Car 
WHERE mileage > 30 
ORDER BY mileage DESC;
```

---

## Intermediate Queries

### 16. Customer Purchase History
**Description**: Join customers with their purchase history
```sql
SELECT c.F_name, c.L_name, s.sale_date, s.price 
FROM Customer c 
JOIN Sales s ON c.Cust_id = s.cust_id;
```

### 17. Dealership Sales Summary
**Description**: Total sales amount by dealership
```sql
SELECT d.D_name, COUNT(s.Sale_id) as Total_Sales, 
       SUM(s.price) as Revenue 
FROM Dealership d 
LEFT JOIN Sales s ON d.D_id = s.d_id 
GROUP BY d.D_name;
```

### 18. Employee Sales Performance
**Description**: Track employee performance through dealership sales
```sql
SELECT e.First_name, e.Last_name, d.D_name, 
       COUNT(s.Sale_id) as Sales_Made 
FROM Employee e 
JOIN Dealership d ON e.emp_id = d.Emp_id 
LEFT JOIN Sales s ON d.D_id = s.d_id 
GROUP BY e.First_name, e.Last_name, d.D_name;
```

### 19. Cars Needing Service
**Description**: Find cars that haven't been serviced recently
```sql
SELECT DISTINCT c.Car_id, c.year, c.color 
FROM Car c 
LEFT JOIN Service s ON c.Car_id = s.Car_id 
WHERE s.S_checkin IS NULL OR s.S_checkin < SYSDATE - 365;
```

### 20. Monthly Sales Report
**Description**: Sales summary by month
```sql
SELECT TO_CHAR(sale_date, 'YYYY-MM') as Month, 
       COUNT(*) as Sales_Count, 
       SUM(price) as Total_Revenue 
FROM Sales 
GROUP BY TO_CHAR(sale_date, 'YYYY-MM') 
ORDER BY Month;
```

### 21. Customer Service History
**Description**: Complete service history for customers
```sql
SELECT c.F_name, c.L_name, s.Service_type, s.Cost, s.S_checkin 
FROM Customer c 
JOIN Service s ON c.Cust_id = s.Cust_id 
ORDER BY c.Cust_id, s.S_checkin;
```

### 22. Stock Availability Report
**Description**: Current stock levels with supplier information
```sql
SELECT st.Name, st.Quantity, su.S_name, su.Company_id 
FROM Stock st 
JOIN Supplier su ON st.S_id = su.S_id 
WHERE st.Quantity > 0;
```

### 23. Test Drive Success Rate
**Description**: Analyze test drives leading to sales
```sql
SELECT t.car_id, COUNT(t.TD_ID) as Test_Drives, 
       COUNT(s.Sale_id) as Sales_Made 
FROM Testdrive t 
LEFT JOIN Sales s ON t.car_id = s.car_id AND t.cust_id = s.cust_id 
GROUP BY t.car_id;
```

### 24. Average Service Cost by Type
**Description**: Average cost for different service types
```sql
SELECT Service_type, AVG(Cost) as Avg_Cost, COUNT(*) as Service_Count 
FROM Service 
GROUP BY Service_type 
ORDER BY Avg_Cost DESC;
```

### 25. Dealership Employee Count
**Description**: Number of employees per dealership
```sql
SELECT d.D_name, COUNT(e.emp_id) as Employee_Count 
FROM Dealership d 
LEFT JOIN Employee e ON d.Emp_id = e.emp_id 
GROUP BY d.D_name;
```

### 26. Cars Sold This Year
**Description**: Cars sold in the current year with customer details
```sql
SELECT c.year, c.color, cu.F_name, cu.L_name, s.sale_date 
FROM Car c 
JOIN Sales s ON c.Car_id = s.car_id 
JOIN Customer cu ON s.cust_id = cu.Cust_id 
WHERE EXTRACT(YEAR FROM s.sale_date) = EXTRACT(YEAR FROM SYSDATE);
```

### 27. Service Duration Analysis
**Description**: Calculate service duration
```sql
SELECT Service_id, Service_type, 
       (S_checkout - S_checkin) as Service_Days 
FROM Service 
WHERE S_checkout IS NOT NULL;
```

### 28. Customer Spending Analysis
**Description**: Total spending per customer
```sql
SELECT c.Cust_id, c.F_name, c.L_name, 
       SUM(s.price) as Total_Spent 
FROM Customer c 
JOIN Sales s ON c.Cust_id = s.cust_id 
GROUP BY c.Cust_id, c.F_name, c.L_name 
ORDER BY Total_Spent DESC;
```

### 29. Warranty Expiration Report
**Description**: Cars with warranties expiring soon
```sql
SELECT s.Sale_id, c.Car_id, s.sale_date, s.warranty, 
       (s.sale_date + s.warranty) as Warranty_Expiry 
FROM Sales s 
JOIN Car c ON s.car_id = c.Car_id 
WHERE (s.sale_date + s.warranty) <= SYSDATE + 30;
```

### 30. Supplier Performance
**Description**: Evaluate supplier performance by stock quantity
```sql
SELECT su.S_name, su.Company_id, 
       SUM(st.Quantity) as Total_Stock, 
       AVG(st.Price) as Avg_Price 
FROM Supplier su 
JOIN Stock st ON su.S_id = st.S_id 
GROUP BY su.S_name, su.Company_id;
```

---

## Advanced Queries

### 31. Top Performing Salespeople (Subquery)
**Description**: Find employees whose dealerships have above-average sales
```sql
SELECT e.First_name, e.Last_name, e.Salary 
FROM Employee e 
WHERE e.emp_id IN (
    SELECT d.Emp_id 
    FROM Dealership d 
    JOIN Sales s ON d.D_id = s.d_id 
    GROUP BY d.Emp_id 
    HAVING COUNT(s.Sale_id) > (
        SELECT AVG(sales_count) 
        FROM (
            SELECT COUNT(s2.Sale_id) as sales_count 
            FROM Sales s2 
            GROUP BY s2.d_id
        )
    )
);
```

### 32. Customer Lifetime Value (Window Function)
**Description**: Calculate customer rankings by total purchases
```sql
SELECT c.Cust_id, c.F_name, c.L_name, 
       SUM(s.price) as Total_Spent,
       ROW_NUMBER() OVER (ORDER BY SUM(s.price) DESC) as Customer_Rank 
FROM Customer c 
JOIN Sales s ON c.Cust_id = s.cust_id 
GROUP BY c.Cust_id, c.F_name, c.L_name;
```

### 33. Running Sales Total (Window Function)
**Description**: Calculate cumulative sales by date
```sql
SELECT sale_date, price, 
       SUM(price) OVER (ORDER BY sale_date) as Running_Total 
FROM Sales 
ORDER BY sale_date;
```

### 34. Car Inventory Analysis (CTE)
**Description**: Complex inventory analysis with multiple metrics
```sql
WITH CarMetrics AS (
    SELECT enginetype, 
           COUNT(*) as Total_Cars,
           AVG(price) as Avg_Price,
           AVG(mileage) as Avg_Mileage 
    FROM Car 
    GROUP BY enginetype
)
SELECT enginetype, Total_Cars, 
       ROUND(Avg_Price, 2) as Avg_Price,
       ROUND(Avg_Mileage, 2) as Avg_Mileage,
       CASE 
           WHEN Avg_Price > 50000 THEN 'Luxury'
           WHEN Avg_Price > 30000 THEN 'Mid-Range'
           ELSE 'Economy'
       END as Price_Category 
FROM CarMetrics;
```

### 35. Service Revenue by Quarter
**Description**: Quarterly service revenue analysis
```sql
SELECT EXTRACT(YEAR FROM S_checkin) as Year,
       EXTRACT(QUARTER FROM S_checkin) as Quarter,
       SUM(Cost) as Total_Revenue,
       COUNT(*) as Service_Count 
FROM Service 
WHERE S_checkin IS NOT NULL 
GROUP BY EXTRACT(YEAR FROM S_checkin), EXTRACT(QUARTER FROM S_checkin) 
ORDER BY Year, Quarter;
```

### 36. Customer Segmentation
**Description**: Segment customers based on purchase behavior
```sql
WITH CustomerStats AS (
    SELECT c.Cust_id, c.F_name, c.L_name,
           COUNT(s.Sale_id) as Purchase_Count,
           SUM(s.price) as Total_Spent,
           AVG(s.price) as Avg_Purchase 
    FROM Customer c 
    LEFT JOIN Sales s ON c.Cust_id = s.cust_id 
    GROUP BY c.Cust_id, c.F_name, c.L_name
)
SELECT Cust_id, F_name, L_name, Purchase_Count, Total_Spent,
       CASE 
           WHEN Total_Spent > 100000 THEN 'VIP'
           WHEN Total_Spent > 50000 THEN 'Premium'
           WHEN Purchase_Count > 0 THEN 'Regular'
           ELSE 'Prospect'
       END as Customer_Segment 
FROM CustomerStats;
```

### 37. Dealership Performance Comparison
**Description**: Compare dealership performance metrics
```sql
SELECT d.D_name,
       COUNT(s.Sale_id) as Total_Sales,
       SUM(s.price) as Revenue,
       AVG(s.price) as Avg_Sale_Price,
       RANK() OVER (ORDER BY SUM(s.price) DESC) as Revenue_Rank 
FROM Dealership d 
LEFT JOIN Sales s ON d.D_id = s.d_id 
GROUP BY d.D_name;
```

### 38. Most Popular Car Features
**Description**: Analyze popular car features and their impact on sales
```sql
SELECT c.enginetype, c.color,
       COUNT(s.Sale_id) as Sales_Count,
       AVG(c.price) as Avg_Price 
FROM Car c 
LEFT JOIN Sales s ON c.Car_id = s.car_id 
GROUP BY c.enginetype, c.color 
HAVING COUNT(s.Sale_id) > 0 
ORDER BY Sales_Count DESC;
```

### 39. Service Efficiency Analysis
**Description**: Analyze service department efficiency
```sql
WITH ServiceMetrics AS (
    SELECT emp_id,
           COUNT(*) as Services_Completed,
           AVG(S_checkout - S_checkin) as Avg_Service_Days,
           SUM(Cost) as Total_Revenue 
    FROM Service 
    WHERE S_checkout IS NOT NULL 
    GROUP BY emp_id
)
SELECT e.First_name, e.Last_name, e.job_title,
       sm.Services_Completed,
       ROUND(sm.Avg_Service_Days, 2) as Avg_Service_Days,
       sm.Total_Revenue 
FROM ServiceMetrics sm 
JOIN Employee e ON sm.emp_id = e.emp_id;
```

### 40. Seasonal Sales Trends
**Description**: Identify seasonal patterns in car sales
```sql
SELECT EXTRACT(MONTH FROM sale_date) as Month,
       TO_CHAR(sale_date, 'Month') as Month_Name,
       COUNT(*) as Sales_Count,
       SUM(price) as Revenue,
       AVG(price) as Avg_Sale_Price 
FROM Sales 
GROUP BY EXTRACT(MONTH FROM sale_date), TO_CHAR(sale_date, 'Month') 
ORDER BY Month;
```

### 41. Test Drive Conversion Analysis
**Description**: Analyze test drive to sale conversion rates
```sql
WITH TestDriveConversion AS (
    SELECT t.car_id,
           COUNT(t.TD_ID) as Test_Drives,
           COUNT(s.Sale_id) as Sales,
           CASE 
               WHEN COUNT(t.TD_ID) > 0 
               THEN ROUND((COUNT(s.Sale_id) * 100.0 / COUNT(t.TD_ID)), 2)
               ELSE 0 
           END as Conversion_Rate 
    FROM Testdrive t 
    LEFT JOIN Sales s ON t.car_id = s.car_id 
    GROUP BY t.car_id
)
SELECT c.Car_id, c.year, c.color, c.enginetype,
       tc.Test_Drives, tc.Sales, tc.Conversion_Rate 
FROM TestDriveConversion tc 
JOIN Car c ON tc.car_id = c.Car_id 
WHERE tc.Test_Drives > 0 
ORDER BY tc.Conversion_Rate DESC;
```

### 42. Employee Quota Performance
**Description**: Compare employee performance against sales quotas
```sql
WITH EmployeePerformance AS (
    SELECT e.emp_id, e.First_name, e.Last_name, e.sale_quota,
           COUNT(s.Sale_id) as Actual_Sales,
           SUM(s.price) as Revenue_Generated 
    FROM Employee e 
    LEFT JOIN Dealership d ON e.emp_id = d.Emp_id 
    LEFT JOIN Sales s ON d.D_id = s.d_id 
    WHERE e.sale_quota IS NOT NULL 
    GROUP BY e.emp_id, e.First_name, e.Last_name, e.sale_quota
)
SELECT First_name, Last_name, sale_quota, Actual_Sales,
       Revenue_Generated,
       ROUND((Actual_Sales * 100.0 / sale_quota), 2) as Quota_Achievement,
       CASE 
           WHEN Actual_Sales >= sale_quota THEN 'Target Met'
           WHEN Actual_Sales >= (sale_quota * 0.8) THEN 'Close to Target'
           ELSE 'Below Target'
       END as Performance_Status 
FROM EmployeePerformance;
```

### 43. Customer Retention Analysis
**Description**: Analyze customer retention and repeat purchases
```sql
WITH CustomerPurchases AS (
    SELECT cust_id,
           COUNT(*) as Purchase_Count,
           MIN(sale_date) as First_Purchase,
           MAX(sale_date) as Last_Purchase,
           MONTHS_BETWEEN(MAX(sale_date), MIN(sale_date)) as Customer_Lifespan 
    FROM Sales 
    GROUP BY cust_id
)
SELECT c.F_name, c.L_name, cp.Purchase_Count, cp.Customer_Lifespan,
       CASE 
           WHEN cp.Purchase_Count = 1 THEN 'One-time Customer'
           WHEN cp.Purchase_Count BETWEEN 2 AND 3 THEN 'Repeat Customer'
           ELSE 'Loyal Customer'
       END as Customer_Type 
FROM CustomerPurchases cp 
JOIN Customer c ON cp.cust_id = c.Cust_id;
```

### 44. Price Optimization Analysis
**Description**: Analyze pricing strategies and market positioning
```sql
WITH PriceAnalysis AS (
    SELECT enginetype,
           COUNT(*) as Total_Cars,
           AVG(price) as Avg_Price,
           PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY price) as Median_Price,
           MIN(price) as Min_Price,
           MAX(price) as Max_Price 
    FROM Car 
    GROUP BY enginetype
)
SELECT enginetype, Total_Cars,
       ROUND(Avg_Price, 2) as Avg_Price,
       ROUND(Median_Price, 2) as Median_Price,
       Min_Price, Max_Price,
       ROUND(Max_Price - Min_Price, 2) as Price_Range 
FROM PriceAnalysis 
ORDER BY Avg_Price DESC;
```

### 45. Service Department Workload
**Description**: Analyze service department capacity and workload
```sql
SELECT TO_CHAR(S_checkin, 'YYYY-MM') as Month,
       COUNT(*) as Total_Services,
       COUNT(DISTINCT emp_id) as Technicians_Involved,
       SUM(Cost) as Revenue,
       AVG(S_checkout - S_checkin) as Avg_Service_Duration 
FROM Service 
WHERE S_checkin IS NOT NULL 
GROUP BY TO_CHAR(S_checkin, 'YYYY-MM') 
ORDER BY Month;
```

### 46. Cross-Selling Opportunities
**Description**: Identify customers for service offerings
```sql
SELECT c.Cust_id, c.F_name, c.L_name, c.Email,
       COUNT(s.Sale_id) as Cars_Purchased,
       COUNT(sv.Service_id) as Services_Used,
       CASE 
           WHEN COUNT(sv.Service_id) = 0 THEN 'Service Prospect'
           WHEN COUNT(sv.Service_id) < COUNT(s.Sale_id) THEN 'Partial Service User'
           ELSE 'Full Service User'
       END as Service_Status 
FROM Customer c 
JOIN Sales s ON c.Cust_id = s.cust_id 
LEFT JOIN Service sv ON c.Cust_id = sv.Cust_id 
GROUP BY c.Cust_id, c.F_name, c.L_name, c.Email;
```

### 47. Geographic Sales Distribution
**Description**: Analyze sales distribution by dealership location
```sql
SELECT d.Location,
       COUNT(s.Sale_id) as Sales_Count,
       SUM(s.price) as Total_Revenue,
       AVG(s.price) as Avg_Sale_Price,
       RANK() OVER (ORDER BY COUNT(s.Sale_id) DESC) as Location_Rank 
FROM Dealership d 
LEFT JOIN Sales s ON d.D_id = s.d_id 
GROUP BY d.Location 
ORDER BY Sales_Count DESC;
```

### 48. Inventory Turnover Analysis
**Description**: Calculate inventory turnover rates
```sql
WITH InventoryMetrics AS (
    SELECT st.Stock_id, st.Name, st.Quantity,
           COUNT(c.Car_id) as Cars_Using_Stock,
           COUNT(s.Sale_id) as Sales_Count 
    FROM Stock st 
    LEFT JOIN Car c ON st.Stock_id = c.Stock_id 
    LEFT JOIN Sales s ON c.Car_id = s.car_id 
    GROUP BY st.Stock_id, st.Name, st.Quantity
)
SELECT Name, Quantity, Cars_Using_Stock, Sales_Count,
       CASE 
           WHEN Quantity > 0 
           THEN ROUND((Sales_Count * 100.0 / Quantity), 2)
           ELSE 0 
       END as Turnover_Rate 
FROM InventoryMetrics 
ORDER BY Turnover_Rate DESC;
```

### 49. Customer Demographics Analysis
**Description**: Comprehensive customer demographic analysis
```sql
WITH DemographicAnalysis AS (
    SELECT 
        CASE 
            WHEN MONTHS_BETWEEN(SYSDATE, DOB)/12 < 25 THEN 'Under 25'
            WHEN MONTHS_BETWEEN(SYSDATE, DOB)/12 < 35 THEN '25-34'
            WHEN MONTHS_BETWEEN(SYSDATE, DOB)/12 < 45 THEN '35-44'
            WHEN MONTHS_BETWEEN(SYSDATE, DOB)/12 < 55 THEN '45-54'
            ELSE '55+'
        END as Age_Group,
        Gender,
        COUNT(*) as Customer_Count,
        AVG(s.price) as Avg_Purchase 
    FROM Customer c 
    LEFT JOIN Sales s ON c.Cust_id = s.cust_id 
    GROUP BY 
        CASE 
            WHEN MONTHS_BETWEEN(SYSDATE, DOB)/12 < 25 THEN 'Under 25'
            WHEN MONTHS_BETWEEN(SYSDATE, DOB)/12 < 35 THEN '25-34'
            WHEN MONTHS_BETWEEN(SYSDATE, DOB)/12 < 45 THEN '35-44'
            WHEN MONTHS_BETWEEN(SYSDATE, DOB)/12 < 55 THEN '45-54'
            ELSE '55+'
        END, Gender
)
SELECT Age_Group, Gender, Customer_Count,
       ROUND(Avg_Purchase, 2) as Avg_Purchase,
       ROUND((Customer_Count * 100.0 / SUM(Customer_Count) OVER()), 2) as Percentage 
FROM DemographicAnalysis 
ORDER BY Age_Group, Gender;
```

### 50. Comprehensive Business Intelligence Dashboard
**Description**: Executive summary with key business metrics
```sql
WITH BusinessMetrics AS (
    -- Sales Metrics
    SELECT 'Sales' as Metric_Type, 
           COUNT(*) as Count_Value,
           SUM(price) as Sum_Value,
           AVG(price) as Avg_Value 
    FROM Sales 
    UNION ALL
    -- Service Metrics
    SELECT 'Services' as Metric_Type,
           COUNT(*) as Count_Value,
           SUM(Cost) as Sum_Value,
           AVG(Cost) as Avg_Value 
    FROM Service 
    UNION ALL
    -- Customer Metrics
    SELECT 'Customers' as Metric_Type,
           COUNT(*) as Count_Value,
           0 as Sum_Value,
           0 as Avg_Value 
    FROM Customer 
    UNION ALL
    -- Inventory Metrics
    SELECT 'Available Cars' as Metric_Type,
           COUNT(*) as Count_Value,
           SUM(price) as Sum_Value,
           AVG(price) as Avg_Value 
    FROM Car 
    WHERE status = 'Available'
)
SELECT Metric_Type,
       Count_Value,
       ROUND(Sum_Value, 2) as Total_Value,
       ROUND(Avg_Value, 2) as Average_Value 
FROM BusinessMetrics;
```

---

## PL/SQL Operations

### 51. Calculate EMI Procedure
**Description**: Calculate EMI for car financing
```sql
CREATE OR REPLACE PROCEDURE calculate_emi(
    p_principal IN NUMBER,
    p_rate IN NUMBER,
    p_tenure IN NUMBER,
    p_emi OUT NUMBER
) AS
    v_monthly_rate NUMBER;
    v_months NUMBER;
BEGIN
    v_monthly_rate := p_rate / (12 * 100);
    v_months := p_tenure * 12;
    
    IF v_monthly_rate = 0 THEN
        p_emi := p_principal / v_months;
    ELSE
        p_emi := p_principal * v_monthly_rate * 
                POWER(1 + v_monthly_rate, v_months) / 
                (POWER(1 + v_monthly_rate, v_months) - 1);
    END IF;
    
    p_emi := ROUND(p_emi, 2);
END;
/
```

### 52. Customer Purchase Summary Function
**Description**: Get customer's total purchases and savings
```sql
CREATE OR REPLACE FUNCTION get_customer_summary(
    p_customer_id IN VARCHAR2
) RETURN VARCHAR2 AS
    v_total_purchases NUMBER := 0;
    v_total_amount NUMBER := 0;
    v_result VARCHAR2(200);
BEGIN
    SELECT COUNT(*), NVL(SUM(price), 0)
    INTO v_total_purchases, v_total_amount
    FROM Sales
    WHERE cust_id = p_customer_id;
    
    v_result := 'Customer ID: ' || p_customer_id || 
                ', Purchases: ' || v_total_purchases || 
                ', Total Amount: $' || v_total_amount;
    
    RETURN v_result;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        RETURN 'Customer not found';
    WHEN OTHERS THEN
        RETURN 'Error: ' || SQLERRM;
END;
/
```

### 53. Auto-Update Car Status Trigger
**Description**: Automatically update car status when sold
```sql
CREATE OR REPLACE TRIGGER update_car_status
    AFTER INSERT ON Sales
    FOR EACH ROW
BEGIN
    UPDATE Car 
    SET status = 'Sold' 
    WHERE Car_id = :NEW.car_id;
    
    DBMS_OUTPUT.PUT_LINE('Car ' || :NEW.car_id || ' status updated to Sold');
END;
/
```

### 54. Service Cost Validation Trigger
**Description**: Validate service costs before insertion
```sql
CREATE OR REPLACE TRIGGER validate_service_cost
    BEFORE INSERT OR UPDATE ON Service
    FOR EACH ROW
DECLARE
    v_max_cost NUMBER := 10000;
BEGIN
    IF :NEW.Cost > v_max_cost THEN
        RAISE_APPLICATION_ERROR(-20001, 
            'Service cost cannot exceed $' || v_max_cost);
    END IF;
    
    IF :NEW.Cost <= 0 THEN
        RAISE_APPLICATION_ERROR(-20002, 
            'Service cost must be positive');
    END IF;
END;
/
```

### 55. Monthly Sales Report Procedure
**Description**: Generate comprehensive monthly sales report
```sql
CREATE OR REPLACE PROCEDURE monthly_sales_report(
    p_month IN NUMBER,
    p_year IN NUMBER
) AS
    v_total_sales NUMBER;
    v_total_revenue NUMBER;
    v_avg_sale NUMBER;
    
    CURSOR c_daily_sales IS
        SELECT TO_CHAR(sale_date, 'DD') as Day,
               COUNT(*) as Sales_Count,
               SUM(price) as Revenue
        FROM Sales
        WHERE EXTRACT(MONTH FROM sale_date) = p_month
        AND EXTRACT(YEAR FROM sale_date) = p_year
        GROUP BY TO_CHAR(sale_date, 'DD')
        ORDER BY Day;
BEGIN
    -- Get summary statistics
    SELECT COUNT(*), NVL(SUM(price), 0), NVL(AVG(price), 0)
    INTO v_total_sales, v_total_revenue, v_avg_sale
    FROM Sales
    WHERE EXTRACT(MONTH FROM sale_date) = p_month
    AND EXTRACT(YEAR FROM sale_date) = p_year;
    
    DBMS_OUTPUT.PUT_LINE('=== MONTHLY SALES REPORT ===');
    DBMS_OUTPUT.PUT_LINE('Month: ' || p_month || '/' || p_year);
    DBMS_OUTPUT.PUT_LINE('Total Sales: ' || v_total_sales);
    DBMS_OUTPUT.PUT_LINE('Total Revenue: $' || ROUND(v_total_revenue, 2));
    DBMS_OUTPUT.PUT_LINE('Average Sale: $' || ROUND(v_avg_sale, 2));
    DBMS_OUTPUT.PUT_LINE('');
    DBMS_OUTPUT.PUT_LINE('Daily Breakdown:');
    
    FOR rec IN c_daily_sales LOOP
        DBMS_OUTPUT.PUT_LINE('Day ' || rec.Day || ': ' || 
                           rec.Sales_Count || ' sales, $' || 
                           ROUND(rec.Revenue, 2) || ' revenue');
    END LOOP;
END;
/
```

### 56. Customer Loyalty Points Function
**Description**: Calculate loyalty points based on purchases
```sql
CREATE OR REPLACE FUNCTION calculate_loyalty_points(
    p_customer_id IN VARCHAR2
) RETURN NUMBER AS
    v_total_spent NUMBER := 0;
    v_points NUMBER := 0;
    v_purchase_count NUMBER := 0;
BEGIN
    SELECT COUNT(*), NVL(SUM(price), 0)
    INTO v_purchase_count, v_total_spent
    FROM Sales
    WHERE cust_id = p_customer_id;
    
    -- Base points: 1 point per dollar spent
    v_points := v_total_spent;
    
    -- Bonus points for multiple purchases
    IF v_purchase_count >= 3 THEN
        v_points := v_points * 1.5; -- 50% bonus
    ELSIF v_purchase_count >= 2 THEN
        v_points := v_points * 1.2; -- 20% bonus
    END IF;
    
    -- VIP bonus for high spenders
    IF v_total_spent > 100000 THEN
        v_points := v_points + 10000; -- VIP bonus
    END IF;
    
    RETURN ROUND(v_points);
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        RETURN 0;
END;
/
```

### 57. Inventory Management Procedure
**Description**: Update inventory levels and reorder alerts
```sql
CREATE OR REPLACE PROCEDURE manage_inventory AS
    CURSOR c_low_stock IS
        SELECT Stock_id, Name, Quantity
        FROM Stock
        WHERE Quantity < 5; -- Low stock threshold
        
    v_reorder_count NUMBER := 0;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== INVENTORY MANAGEMENT REPORT ===');
    DBMS_OUTPUT.PUT_LINE('Low Stock Items:');
    
    FOR rec IN c_low_stock LOOP
        DBMS_OUTPUT.PUT_LINE('Stock ID: ' || rec.Stock_id || 
                           ', Item: ' || rec.Name || 
                           ', Quantity: ' || rec.Quantity);
        v_reorder_count := v_reorder_count + 1;
    END LOOP;
    
    IF v_reorder_count = 0 THEN
        DBMS_OUTPUT.PUT_LINE('All items are adequately stocked.');
    ELSE
        DBMS_OUTPUT.PUT_LINE('');
        DBMS_OUTPUT.PUT_LINE('Total items needing reorder: ' || v_reorder_count);
    END IF;
END;
/
```

### 58. Service Reminder Procedure
**Description**: Generate service reminders for customers
```sql
CREATE OR REPLACE PROCEDURE generate_service_reminders AS
    CURSOR c_service_due IS
        SELECT DISTINCT c.Cust_id, cu.F_name, cu.L_name, cu.Email,
               ca.Car_id, ca.year, ca.color
        FROM Customer cu
        JOIN Sales s ON cu.Cust_id = s.cust_id
        JOIN Car ca ON s.car_id = ca.Car_id
        WHERE ca.Car_id NOT IN (
            SELECT Car_id FROM Service 
            WHERE S_checkin >= SYSDATE - 180 -- Last 6 months
        );
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== SERVICE REMINDERS ===');
    
    FOR rec IN c_service_due LOOP
        DBMS_OUTPUT.PUT_LINE('Customer: ' || rec.F_name || ' ' || rec.L_name);
        DBMS_OUTPUT.PUT_LINE('Email: ' || rec.Email);
        DBMS_OUTPUT.PUT_LINE('Car: ' || rec.year || ' ' || rec.color || 
                           ' (ID: ' || rec.Car_id || ')');
        DBMS_OUTPUT.PUT_LINE('Reminder: Your car is due for service!');
        DBMS_OUTPUT.PUT_LINE('---');
    END LOOP;
END;
/
```

### 59. Sales Performance Package
**Description**: Package containing sales analysis procedures
```sql
CREATE OR REPLACE PACKAGE sales_analytics AS
    PROCEDURE top_performers(p_month IN NUMBER DEFAULT NULL);
    FUNCTION sales_growth(p_current_month IN NUMBER, p_current_year IN NUMBER) RETURN NUMBER;
    PROCEDURE forecast_sales(p_months_ahead IN NUMBER);
END sales_analytics;
/

CREATE OR REPLACE PACKAGE BODY sales_analytics AS
    
    PROCEDURE top_performers(p_month IN NUMBER DEFAULT NULL) AS
        CURSOR c_performers IS
            SELECT d.D_name, e.First_name, e.Last_name,
                   COUNT(s.Sale_id) as Sales_Count,
                   SUM(s.price) as Revenue
            FROM Employee e
            JOIN Dealership d ON e.emp_id = d.Emp_id
            JOIN Sales s ON d.D_id = s.d_id
            WHERE (p_month IS NULL OR EXTRACT(MONTH FROM s.sale_date) = p_month)
            GROUP BY d.D_name, e.First_name, e.Last_name
            ORDER BY Revenue DESC;
    BEGIN
        DBMS_OUTPUT.PUT_LINE('=== TOP PERFORMERS ===');
        FOR rec IN c_performers LOOP
            DBMS_OUTPUT.PUT_LINE(rec.First_name || ' ' || rec.Last_name || 
                               ' (' || rec.D_name || '): $' || 
                               ROUND(rec.Revenue, 2) || ' revenue, ' ||
                               rec.Sales_Count || ' sales');
        END LOOP;
    END top_performers;
    
    FUNCTION sales_growth(p_current_month IN NUMBER, p_current_year IN NUMBER) 
    RETURN NUMBER AS
        v_current_sales NUMBER;
        v_previous_sales NUMBER;
        v_growth_rate NUMBER;
    BEGIN
        -- Current month sales
        SELECT NVL(SUM(price), 0)
        INTO v_current_sales
        FROM Sales
        WHERE EXTRACT(MONTH FROM sale_date) = p_current_month
        AND EXTRACT(YEAR FROM sale_date) = p_current_year;
        
        -- Previous month sales
        SELECT NVL(SUM(price), 0)
        INTO v_previous_sales
        FROM Sales
        WHERE EXTRACT(MONTH FROM sale_date) = p_current_month - 1
        AND EXTRACT(YEAR FROM sale_date) = p_current_year;
        
        IF v_previous_sales > 0 THEN
            v_growth_rate := ((v_current_sales - v_previous_sales) / v_previous_sales) * 100;
        ELSE
            v_growth_rate := 0;
        END IF;
        
        RETURN ROUND(v_growth_rate, 2);
    END sales_growth;
    
    PROCEDURE forecast_sales(p_months_ahead IN NUMBER) AS
        v_avg_monthly_sales NUMBER;
        v_forecast NUMBER;
    BEGIN
        SELECT AVG(monthly_sales)
        INTO v_avg_monthly_sales
        FROM (
            SELECT SUM(price) as monthly_sales
            FROM Sales
            WHERE sale_date >= SYSDATE - 365
            GROUP BY TO_CHAR(sale_date, 'YYYY-MM')
        );
        
        v_forecast := v_avg_monthly_sales * p_months_ahead;
        
        DBMS_OUTPUT.PUT_LINE('=== SALES FORECAST ===');
        DBMS_OUTPUT.PUT_LINE('Average Monthly Sales: $' || ROUND(v_avg_monthly_sales, 2));
        DBMS_OUTPUT.PUT_LINE('Forecast for next ' || p_months_ahead || 
                           ' months: $' || ROUND(v_forecast, 2));
    END forecast_sales;
    
END sales_analytics;
/
```

### 60. Database Maintenance Procedure
**Description**: Comprehensive database maintenance and cleanup
```sql
CREATE OR REPLACE PROCEDURE database_maintenance AS
    v_count NUMBER;
    v_total_records NUMBER := 0;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== DATABASE MAINTENANCE REPORT ===');
    DBMS_OUTPUT.PUT_LINE('Generated on: ' || TO_CHAR(SYSDATE, 'DD-MON-YYYY HH24:MI:SS'));
    DBMS_OUTPUT.PUT_LINE('');
    
    -- Table record counts
    SELECT COUNT(*) INTO v_count FROM Car;
    DBMS_OUTPUT.PUT_LINE('Cars: ' || v_count || ' records');
    v_total_records := v_total_records + v_count;
    
    SELECT COUNT(*) INTO v_count FROM Customer;
    DBMS_OUTPUT.PUT_LINE('Customers: ' || v_count || ' records');
    v_total_records := v_total_records + v_count;
    
    SELECT COUNT(*) INTO v_count FROM Sales;
    DBMS_OUTPUT.PUT_LINE('Sales: ' || v_count || ' records');
    v_total_records := v_total_records + v_count;
    
    SELECT COUNT(*) INTO v_count FROM Service;
    DBMS_OUTPUT.PUT_LINE('Services: ' || v_count || ' records');
    v_total_records := v_total_records + v_count;
    
    SELECT COUNT(*) INTO v_count FROM Employee;
    DBMS_OUTPUT.PUT_LINE('Employees: ' || v_count || ' records');
    v_total_records := v_total_records + v_count;
    
    DBMS_OUTPUT.PUT_LINE('');
    DBMS_OUTPUT.PUT_LINE('Total Records: ' || v_total_records);
    
    -- Data integrity checks
    DBMS_OUTPUT.PUT_LINE('');
    DBMS_OUTPUT.PUT_LINE('=== DATA INTEGRITY CHECKS ===');
    
    -- Check for orphaned records
    SELECT COUNT(*) INTO v_count
    FROM Sales s
    WHERE NOT EXISTS (SELECT 1 FROM Car c WHERE c.Car_id = s.car_id);
    
    IF v_count > 0 THEN
        DBMS_OUTPUT.PUT_LINE('WARNING: ' || v_count || ' sales records with invalid car references');
    ELSE
        DBMS_OUTPUT.PUT_LINE('✓ All sales records have valid car references');
    END IF;
    
    -- Check for incomplete service records
    SELECT COUNT(*) INTO v_count
    FROM Service
    WHERE S_checkin IS NOT NULL AND S_checkout IS NULL
    AND S_checkin < SYSDATE - 30; -- Services older than 30 days without checkout
    
    IF v_count > 0 THEN
        DBMS_OUTPUT.PUT_LINE('WARNING: ' || v_count || ' incomplete service records');
    ELSE
        DBMS_OUTPUT.PUT_LINE('✓ All service records are complete');
    END IF;
    
    DBMS_OUTPUT.PUT_LINE('');
    DBMS_OUTPUT.PUT_LINE('Maintenance completed successfully!');
    
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('ERROR during maintenance: ' || SQLERRM);
END;
/
```

---

## Sample Execution Commands

To execute these queries and procedures:

```sql
-- Execute basic queries
SELECT * FROM Car WHERE year = 2023;

-- Call procedures
EXEC monthly_sales_report(12, 2023);
EXEC database_maintenance;

-- Use functions
SELECT get_customer_summary('C001') FROM DUAL;
SELECT calculate_loyalty_points('C001') FROM DUAL;

-- Package procedures
EXEC sales_analytics.top_performers(12);
SELECT sales_analytics.sales_growth(12, 2023) FROM DUAL;
```

This comprehensive collection demonstrates various SQL concepts from basic operations to advanced analytics and PL/SQL programming, providing a complete toolkit for automobile dealership management.
