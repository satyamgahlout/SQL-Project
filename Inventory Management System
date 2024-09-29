-- Create a database 
CREATE DATABASE InventoryManagementSystem;

-- Use the InventoryManagement database
USE InventoryManagementSystem;

-- Create Suppliers table to store supplier information
CREATE TABLE Suppliers
(
SupplierID INT PRIMARY KEY IDENTITY(1,1),
SupplierName VARCHAR(150),
Phone NUMERIC(30),
SupplierAddress VARCHAR(100),
Email VARCHAR(100), 
CREATEDDATE DATETIME DEFAULT GETDATE(),
MODIFYDATE DATETIME DEFAULT GETDATE()  
);

-- Create Categories table to store product categories
CREATE TABLE Categories
(
CategoryID INT PRIMARY KEY IDENTITY(1,1),
CategoryName VARCHAR(100) NOT NULL,
CREATEDDATE DATETIME DEFAULT GETDATE(),
MODIFYDATE DATETIME DEFAULT GETDATE()
);

-- Create Products table to store product details
CREATE TABLE Products
(
ProductID INT PRIMARY KEY IDENTITY(1,1), 
ProductName VARCHAR(200) NOT NULL,
Stock INT NOT NULL, 
Price DECIMAL(10,2) NOT NULL, 
CategoryID INT,
SupplierID INT, 
CREATEDDATE DATETIME DEFAULT GETDATE(), 
MODIFYDATE DATETIME DEFAULT GETDATE(),
CONSTRAINT FK_Products_Categories FOREIGN KEY (CategoryID) REFERENCES Categories(CategoryID),
CONSTRAINT FK_Products_Suppliers FOREIGN KEY (SupplierID) REFERENCES Suppliers(SupplierID)
);

-- Create Employees table to store employee information
CREATE TABLE Employees
(
EmployeeID INT PRIMARY KEY IDENTITY(1,1), 
FirstName VARCHAR(100), 
LastName VARCHAR(100), 
Position VARCHAR(100), 
Department VARCHAR(100),
HireDate DATE, 
CREATEDDATE DATETIME DEFAULT GETDATE(), 
MODIFYDATE DATETIME DEFAULT GETDATE() 
);
-- Query the view to get product details with a price filter
SELECT * FROM ProductDetails WHERE Price > 500;

-- Display all records from Suppliers table
SELECT * FROM Suppliers;

-- Display all records from Categories table
SELECT * FROM Categories;

-- Display all records from Products table
SELECT * FROM Products;

-- Alter Employees table to add a new column E_Address
ALTER TABLE Employees ADD E_Address VARCHAR(100);

-- Drop the E_Address column from Employees table
ALTER TABLE Employees DROP COLUMN E_Address;

-- Correcting a mistake in the table name Employees instead of Employee
ALTER TABLE Employees ALTER COLUMN Department VARCHAR(200);

-- Rename the column ProductName to Name in the Products table
EXEC sp_rename 'Products.ProductName', 'Name', 'COLUMN';

-- Insert sample data into Suppliers table
INSERT INTO Suppliers (SupplierName, Phone, SupplierAddress, Email) VALUES ('Mukesh', 12345, 'Noida Sector 62', 'mukesh@123');
INSERT INTO Suppliers (SupplierName, Phone, SupplierAddress, Email) VALUES ('Rajesh', 12346, 'Noida Sector 61', 'rajesh@123');
INSERT INTO Suppliers (SupplierName, Phone, SupplierAddress, Email) VALUES ('Prakash', 12343, 'Noida Sector 63', 'prakash@123');

-- Insert sample data into Categories table
INSERT INTO Categories(CategoryName) VALUES ('Milk');
INSERT INTO Categories(CategoryName) VALUES ('Milk');
INSERT INTO Categories(CategoryName) VALUES ('Milk');

-- Insert sample data into Products table
INSERT INTO Products(ProductName, Stock, Price, CategoryID, SupplierID) VALUES ('Buffalo milk 1 kg', 85, 2720, 1, 1);
INSERT INTO Products(ProductName, Stock, Price, CategoryID, SupplierID) VALUES ('ton milk 1 kg', 80, 2670, 2, 2);
INSERT INTO Products(ProductName, Stock, Price, CategoryID, SupplierID) VALUES ('full cream milk 1 kg', 87, 2800, 3, 3);

-- Insert sample data into Employees table
INSERT INTO Employees(FirstName, LastName, Position, Department, HireDate) VALUES ('Vikash', 'Kumar', 'Salesman', 'Marketing', '2023-09-12');
INSERT INTO Employees(FirstName, LastName, Position, Department, HireDate) VALUES ('Dinesh', 'Sharma', 'Salesman', 'Marketing', '2023-09-12');
INSERT INTO Employees(FirstName, LastName, Position, Department, HireDate) VALUES ('Kartik', 'Sisodiya', 'Salesman', 'Marketing', '2023-09-12');

-- Use LIKE operator to find employees whose first name starts with 'D'
SELECT * FROM Employees WHERE FirstName LIKE 'D%';

-- Use IN and NOT IN operators to filter Products based on CategoryID and SupplierID
SELECT * FROM Products WHERE CategoryID IN (1, 2);
SELECT * FROM Products WHERE SupplierID NOT IN (1, 3);

-- Use UPPER and LOWER functions to change the case of SupplierName
SELECT UPPER(SupplierName) AS SuppliersNameUpper FROM Suppliers;
SELECT LOWER(SupplierName) AS SuppliersNameLower FROM Suppliers;

-- Use SUBSTRING function to extract part of ProductName
SELECT SUBSTRING(ProductName, 1, 2) AS ProductName FROM Products;

-- Use arithmetic operators to filter products based on Price
SELECT * FROM Products WHERE Price > 2700;
SELECT * FROM Products WHERE Price < 2700;

-- Use aggregate functions to get count, average price, and maximum price of products
SELECT COUNT(*) AS TotalProducts FROM Products;
SELECT AVG(Price) AS AveragePrice FROM Products;
SELECT MAX(Price) AS MaxPrice FROM Products;

-- Use BETWEEN operator to filter products based on Price range
SELECT * FROM Products WHERE Price BETWEEN 500 AND 1500;

-- Create a scalar function to get the product count by category
CREATE FUNCTION GetProductCountByCategory3 (@CategoryID INT)
RETURNS INT
AS
BEGIN
    DECLARE @ProductCount INT;
    SELECT @ProductCount = COUNT(*) FROM Products WHERE CategoryID = @CategoryID;
    RETURN @ProductCount;
END;

-- Use the created scalar function
SELECT dbo.GetProductCountByCategory3(1) AS ProductCountForCategory3;

-- Inner join between Products, Categories, and Suppliers to retrieve related information
 

-- Left join between Products and Categories to retrieve all products with their category names
SELECT p.ProductName, c.CategoryName
FROM Products p
LEFT JOIN Categories c ON p.CategoryID = c.CategoryID;

-- Right join between Products and Categories to retrieve all categories with their products
SELECT p.ProductName, c.CategoryName
FROM Products p
RIGHT JOIN Categories c ON p.CategoryID = c.CategoryID;

-- Full outer join between Products and Categories to retrieve all products and categories
SELECT p.ProductName, c.CategoryName
FROM Products p
FULL OUTER JOIN Categories c ON p.CategoryID = c.CategoryID;

-- Create a stored procedure to insert a new supplier
CREATE PROCEDURE spInsertSupplier2
    @SupplierName VARCHAR(100),
    @Email VARCHAR(100),
    @Phone NUMERIC(15),
    @SupplierAddress VARCHAR(255)
AS
BEGIN
    INSERT INTO Suppliers(SupplierName, Email, Phone, SupplierAddress)
    VALUES (@SupplierName, @Email, @Phone, @SupplierAddress);
END;

-- Execute the stored procedure to insert a new supplier
EXEC spInsertSupplier2 'New Supplier', 'contact@newsupplier.com', 51234, '123 Supplier St.';

-- Create a trigger to audit product updates
CREATE TRIGGER trgProductUpdateAudit
ON Products
AFTER UPDATE
AS
BEGIN
    INSERT INTO ProductAudit(ProductID, OldPrice, NewPrice, ModifiedDate)
    SELECT
        d.ProductID,
        d.Price AS OldPrice,
        i.Price AS NewPrice,
        GETDATE() AS ModifiedDate
    FROM
        deleted d
    INNER JOIN
        inserted i ON d.ProductID = i.ProductID
    WHERE
        d.Price <> i.Price;
END;

-- Create indexes to improve query performance
CREATE INDEX idx_ProductName ON Products(ProductID);
CREATE INDEX idx_CategoryID ON Products(CategoryID);

-- Create a view to display detailed information about products
CREATE VIEW vwProductDetails AS
SELECT p.ProductID, p.ProductName, c.CategoryName, s.SupplierName, p.Price, p.Stock
FROM Products p
JOIN Categories c ON p.CategoryID = c.CategoryID
JOIN Suppliers s ON p.SupplierID = s.SupplierID;

-- Query the view to get product details with a price filter
SELECT * FROM vwProductDetails WHERE Price > 500;

-- Display all records from Suppliers table
SELECT * FROM Suppliers;

-- Display all records from Categories table
SELECT * FROM Categories;

-- Display all records from Products table
SELECT * FROM Products;

-- Display all records from Employees table
SELECT * FROM Employees;

