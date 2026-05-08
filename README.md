<p align="center">
  <h1 align="center">💊 MediCare Pharmacy Management System</h1>
  <p align="center">
    <b>A full-featured desktop pharmacy solution — built with love, C#, and a lot of SQL.</b><br/>
    <i>Manage medicines, orders, prescriptions, and users — all in one place.</i>
  </p>
  <p align="center">
    <img src="https://img.shields.io/badge/Platform-Windows-blue?style=for-the-badge&logo=windows" />
    <img src="https://img.shields.io/badge/.NET_Framework-4.7.2-purple?style=for-the-badge&logo=dotnet" />
    <img src="https://img.shields.io/badge/Database-SQL_Server-red?style=for-the-badge&logo=microsoftsqlserver" />
    <img src="https://img.shields.io/badge/Language-C%23-green?style=for-the-badge&logo=csharp" />
    <img src="https://img.shields.io/badge/License-Educational-orange?style=for-the-badge" />
  </p>
</p>

📋 Table of Contents

Overview
Features
Tech Stack
Database Schema
Project Structure
Getting Started
Default Login Credentials
Contributors


🏥 Overview
MediCare is a role-based pharmacy desktop application designed to streamline the complete pharmacy workflow — from customer medicine orders to pharmacist inventory management and admin-level oversight.

🎯 Three roles. One system. Zero chaos.

👤 Customer💊 Pharmacist🛡️ AdminBrowse & order medicinesManage inventoryFull system controlUpload prescriptionsProcess ordersAnalytics & reportsTrack order statusApprove prescriptionsUser management

✨ Features
👤 Customer

✅ Register and login with role-based access
🔍 Browse and search medicines with real-time stock display
🛒 Add medicines to cart with quantity validation
❌ Remove items from cart (stock automatically restocked)
📦 Place orders and track order status
📄 Upload and view prescriptions
✏️ Edit profile and change password
🔐 Forgot password / reset via email

💊 Pharmacist

📋 View and search medicine inventory
📦 Restock medicines with quantity popup
🔄 Process and update order status (Pending → Processing → Delivered)
🔎 Filter orders by status
✅ Approve or reject customer prescriptions
✏️ Edit profile and change password

🛡️ Admin

📊 Dashboard with real-time stats (users, medicines, orders, revenue)
➕ Add, edit, and delete medicines
📋 View and update all orders with status management
📄 Approve or reject prescriptions
👥 Manage all users (view, delete)
📈 Reports & analytics — top selling medicines, revenue stats
⚙️ Settings — change admin password


🛠️ Tech Stack
TechnologyUsage⚙️ C# (.NET Framework 4.7.2)Application logic🖥️ Windows FormsUI / Frontend🗄️ SQL Server (LocalDB)Database🔌 ADO.NETDatabase connectivity🎨 System.DrawingCustom UI rendering

🗄️ Database Schema
👥 Users
ColumnTypeDescriptionUserIDINT (PK)Auto-increment primary keyFirstNameNVARCHAR(50)User's first nameLastNameNVARCHAR(50)User's last nameUsernameNVARCHAR(50)Unique usernameEmailNVARCHAR(100)Email addressPhoneNVARCHAR(20)Phone numberPasswordNVARCHAR(255)User passwordRoleNVARCHAR(20)Customer / Pharmacist / Admin
💊 Medicines
ColumnTypeDescriptionMedicineIDINT (PK)Auto-increment primary keyNameNVARCHAR(100)Medicine nameCategoryNVARCHAR(50)Medicine categoryPriceDECIMAL(10,2)Price per unitStockINTAvailable stock quantity
🛒 Cart
ColumnTypeDescriptionCartIDINT (PK)Auto-increment primary keyUserIDINT (FK)References UsersMedicineIDINT (FK)References MedicinesQuantityINTQuantity added to cart
📦 Orders
ColumnTypeDescriptionOrderIDINT (PK)Auto-increment primary keyUserIDINT (FK)References UsersMedicineNameNVARCHAR(100)Medicine orderedQuantityINTQuantity orderedTotalPriceDECIMAL(10,2)Total priceOrderDateDATETIMEDate of orderStatusNVARCHAR(20)Pending / Processing / Delivered
📄 Prescriptions
ColumnTypeDescriptionPrescriptionIDINT (PK)Auto-increment primary keyUserIDINT (FK)References UsersFileNameNVARCHAR(255)Uploaded file nameFilePathNVARCHAR(500)File path on diskUploadDateDATETIMEDate uploadedStatusNVARCHAR(20)Pending / Approved / Rejected

📁 Project Structure
PharmacyUser/
├── PharmacyUser/
│   ├── 🚀 Program.cs
│   ├── 🔌 DBHelper.cs
│   ├── 🔐 LoginForm.cs
│   ├── 📝 RegisterForm.cs
│   ├── 🔑 ForgotPasswordForm.cs
│   ├── 👤 CustomerDashboard.cs
│   ├── 💊 PharmacistDashboard.cs
│   ├── 🛡️ AdminDashboard.cs
│   └── ⚙️ App.config
└── PharmacyUser.sln

🚀 Getting Started
Prerequisites
Before you begin, make sure you have the following installed:

🖥️ Visual Studio 2019 or later
⚙️ .NET Framework 4.7.2
🗄️ SQL Server LocalDB

⚡ Installation
Step 1 — Clone the repository
bashgit clone https://github.com/yourusername/medicare-pharmacy.git
cd medicare-pharmacy
Step 2 — Set up the database
Open SSMS, connect to (localdb)\MSSQLLocalDB and run:
sqlCREATE DATABASE PharmacyDB;
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

-- 🌱 Seed: Sample Medicines
INSERT INTO Medicines (Name, Category, Price, Stock) VALUES
('Paracetamol',  'Pain Relief',  60.00,  100),
('Vitamin D3',   'Supplements',  150.00,  80),
('Zinc Tablets', 'Supplements',   95.00,  60),
('Omeprazole',   'Gastric',       75.00,  90),
('Amoxicillin',  'Antibiotic',   120.00,  50),
('Metformin',    'Diabetes',      85.00,  70),
('Aspirin',      'Pain Relief',   45.00, 110),
('Cetirizine',   'Allergy',       55.00,  65);

-- 🌱 Seed: Default Admin
INSERT INTO Users (FirstName, LastName, Username, Email, Phone, Password, Role)
VALUES ('Admin', 'User', 'arnob', 'admin@medicare.com', '01700000000', '1', 'Admin');
Step 3 — Open the project
Open PharmacyUser.sln in Visual Studio.
Step 4 — Check the connection string
Make sure App.config has:
xml<connectionStrings>
  <add name="PharmacyDB"
       connectionString="Server=(localdb)\MSSQLLocalDB;Database=PharmacyDB;Trusted_Connection=True;"
       providerName="System.Data.SqlClient" />
</connectionStrings>
Step 5 — Build and run
Press Ctrl + Shift + B to build, then F5 to run. 🎉

🔑 Default Login Credentials
RoleUsernamePassword🛡️ Adminarnob1

💡 New Customer and Pharmacist accounts can be created via the Register screen.


👥 Contributors

🙌 Built with teamwork, late nights, and plenty of coffee.

#NameContribution🛡️Arnob DasAdmin Dashboard👤Istiauque Hossain AkibCustomer Dashboard, Forgot Password💊Adreta MallickPharmacist Dashboard🗄️Snahasish Paul TonmoyDatabase Connection, Register, Login, README

📄 License

🎓 This project is developed for educational purposes as part of an academic assignment.
Feel free to learn from it — but please don't forget to credit the team!


<p align="center">
  Made with ❤️ by the MediCare Team
</p>
```
