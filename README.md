# 💊Pharmacy Management System

A full-featured **Windows Forms** desktop application built with **C# (.NET)** and **SQL Server** for managing a pharmacy with three user roles: Customer, Pharmacist, and Admin.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Database Schema](#database-schema)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Default Login Credentials](#default-login-credentials)
- [Contributors](#contributors)

---

## 🏥 Overview

MediCare Pharmacy Management System is a desktop application that allows customers to browse and order medicines, pharmacists to manage inventory and process orders, and admins to oversee the entire system including users, medicines, reports, and settings.

---

## ✨ Features

### 👤 Customer
- Register and login with role-based access
- Browse and search medicines with real-time stock display
- Add medicines to cart with quantity validation
- Remove items from cart (stock automatically restocked)
- Place orders and track order status
- Upload and view prescriptions
- Edit profile and change password
- Forgot password / reset via email

### 💊 Pharmacist
- View and search medicine inventory
- Restock medicines with quantity popup
- Process and update order status (Pending → Processing → Delivered)
- Filter orders by status
- Approve or reject customer prescriptions
- Edit profile and change password

### 🛡️ Admin
- Dashboard with real-time stats (users, medicines, orders, revenue)
- Add, edit, and delete medicines
- View and update all orders with status management
- Approve or reject prescriptions
- Manage all users (view, delete)
- Reports and analytics — top selling medicines, revenue stats
- Settings — change admin password

---

## 🛠️ Tech Stack

| Technology | Usage |
|---|---|
| C# (.NET Framework 4.7.2) | Application logic |
| Windows Forms | UI / Frontend |
| SQL Server (LocalDB) | Database |
| ADO.NET | Database connectivity |
| System.Drawing | Custom UI rendering |

---

## 🗄️ Database Schema

### Users
| Column | Type | Description |
|---|---|---|
| UserID | INT (PK) | Auto-increment primary key |
| FirstName | NVARCHAR(50) | User's first name |
| LastName | NVARCHAR(50) | User's last name |
| Username | NVARCHAR(50) | Unique username |
| Email | NVARCHAR(100) | Email address |
| Phone | NVARCHAR(20) | Phone number |
| Password | NVARCHAR(255) | User password |
| Role | NVARCHAR(20) | Customer / Pharmacist / Admin |

### Medicines
| Column | Type | Description |
|---|---|---|
| MedicineID | INT (PK) | Auto-increment primary key |
| Name | NVARCHAR(100) | Medicine name |
| Category | NVARCHAR(50) | Medicine category |
| Price | DECIMAL(10,2) | Price per unit |
| Stock | INT | Available stock quantity |

### Cart
| Column | Type | Description |
|---|---|---|
| CartID | INT (PK) | Auto-increment primary key |
| UserID | INT (FK) | References Users |
| MedicineID | INT (FK) | References Medicines |
| Quantity | INT | Quantity added to cart |

### Orders
| Column | Type | Description |
|---|---|---|
| OrderID | INT (PK) | Auto-increment primary key |
| UserID | INT (FK) | References Users |
| MedicineName | NVARCHAR(100) | Medicine ordered |
| Quantity | INT | Quantity ordered |
| TotalPrice | DECIMAL(10,2) | Total price |
| OrderDate | DATETIME | Date of order |
| Status | NVARCHAR(20) | Pending / Processing / Delivered |

### Prescriptions
| Column | Type | Description |
|---|---|---|
| PrescriptionID | INT (PK) | Auto-increment primary key |
| UserID | INT (FK) | References Users |
| FileName | NVARCHAR(255) | Uploaded file name |
| FilePath | NVARCHAR(500) | File path on disk |
| UploadDate | DATETIME | Date uploaded |
| Status | NVARCHAR(20) | Pending / Approved / Rejected |

---

## 📁 Project Structure

```
PharmacyUser/
├── PharmacyUser/
│   ├── Program.cs
│   ├── DBHelper.cs
│   ├── LoginForm.cs
│   ├── RegisterForm.cs
│   ├── ForgotPasswordForm.cs
│   ├── CustomerDashboard.cs
│   ├── PharmacistDashboard.cs
│   ├── AdminDashboard.cs
│   └── App.config
└── PharmacyUser.sln
```

---

## 🚀 Getting Started

### Prerequisites

- [Visual Studio 2019 or later](https://visualstudio.microsoft.com/)
- [.NET Framework 4.7.2](https://dotnet.microsoft.com/download/dotnet-framework)
- [SQL Server LocalDB](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/sql-server-express-localdb)

### Installation

**1. Clone the repository**
```bash
git clone https://github.com/yourusername/medicare-pharmacy.git
cd medicare-pharmacy
```

**2. Set up the database**

Open SSMS, connect to `(localdb)\MSSQLLocalDB` and run:

```sql
CREATE DATABASE PharmacyDB;
USE PharmacyDB;

CREATE TABLE Users (
    UserID    INT IDENTITY(1,1) PRIMARY KEY,
    FirstName NVARCHAR(50)  NOT NULL,
    LastName  NVARCHAR(50)  NOT NULL,
    Username  NVARCHAR(50)  NOT NULL UNIQUE,
    Email     NVARCHAR(100) NOT NULL,
    Phone     NVARCHAR(20),
    Password  NVARCHAR(255) NOT NULL,
    Role      NVARCHAR(20)  NOT NULL DEFAULT 'Customer'
);

CREATE TABLE Medicines (
    MedicineID INT IDENTITY(1,1) PRIMARY KEY,
    Name       NVARCHAR(100)  NOT NULL,
    Category   NVARCHAR(50),
    Price      DECIMAL(10,2)  NOT NULL,
    Stock      INT            NOT NULL DEFAULT 0
);

CREATE TABLE Cart (
    CartID     INT IDENTITY(1,1) PRIMARY KEY,
    UserID     INT NOT NULL,
    MedicineID INT NOT NULL,
    Quantity   INT NOT NULL DEFAULT 1,
    FOREIGN KEY (UserID)     REFERENCES Users(UserID),
    FOREIGN KEY (MedicineID) REFERENCES Medicines(MedicineID)
);

CREATE TABLE Orders (
    OrderID      INT IDENTITY(1,1) PRIMARY KEY,
    UserID       INT            NOT NULL,
    MedicineName NVARCHAR(100)  NOT NULL,
    Quantity     INT            NOT NULL,
    TotalPrice   DECIMAL(10,2)  NOT NULL,
    OrderDate    DATETIME       NOT NULL DEFAULT GETDATE(),
    Status       NVARCHAR(20)   NOT NULL DEFAULT 'Pending',
    FOREIGN KEY (UserID) REFERENCES Users(UserID)
);

CREATE TABLE Prescriptions (
    PrescriptionID INT IDENTITY(1,1) PRIMARY KEY,
    UserID         INT            NOT NULL,
    FileName       NVARCHAR(255)  NOT NULL,
    FilePath       NVARCHAR(500),
    UploadDate     DATETIME       NOT NULL DEFAULT GETDATE(),
    Status         NVARCHAR(20)   NOT NULL DEFAULT 'Pending',
    FOREIGN KEY (UserID) REFERENCES Users(UserID)
);

INSERT INTO Medicines (Name, Category, Price, Stock) VALUES
('Paracetamol',  'Pain Relief',  60.00,  100),
('Vitamin D3',   'Supplements',  150.00,  80),
('Zinc Tablets', 'Supplements',   95.00,  60),
('Omeprazole',   'Gastric',       75.00,  90),
('Amoxicillin',  'Antibiotic',   120.00,  50),
('Metformin',    'Diabetes',      85.00,  70),
('Aspirin',      'Pain Relief',   45.00, 110),
('Cetirizine',   'Allergy',       55.00,  65);

INSERT INTO Users (FirstName, LastName, Username, Email, Phone, Password, Role)
VALUES ('Admin', 'User', 'arnob', 'admin@medicare.com', '01700000000', '1', 'Admin');
```

**3. Open the project**

Open `PharmacyUser.sln` in Visual Studio.

**4. Check the connection string**

Make sure `App.config` has:

```xml
<connectionStrings>
  <add name="PharmacyDB"
       connectionString="Server=(localdb)\MSSQLLocalDB;Database=PharmacyDB;Trusted_Connection=True;"
       providerName="System.Data.SqlClient" />
</connectionStrings>
```

**5. Build and run**

Press `Ctrl + Shift + B` to build, then `F5` to run.

---

## 🔑 Default Login Credentials

| Role | Username | Password |
|---|---|---|
| Admin | `arnob` | `1` |

> New Customer and Pharmacist accounts can be created via the Register screen.

---

## 👥 Contributors

| Name | Role |
|---|---|
| [Your Name] | Customer Dashboard, Register/Login, Database |
| [Friend's Name] | Admin Dashboard, Pharmacist Dashboard |

---

## 📄 License

This project is for educational purposes.
