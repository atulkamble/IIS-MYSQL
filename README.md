# 🚀 IIS + HTML + MySQL (Complete Project Guide)

## 🧱 Architecture

```text
Browser → IIS (Web Server) → PHP (Backend) → MySQL (Database)
```

👉 Key fact: **HTML alone cannot connect to DB → PHP is required**

---

# 📦 Prerequisites

Install:

* IIS (Windows Web Server)
* MySQL Community Server
* PHP

---

# ⚙️ STEP 1: Install IIS

### ▶️ PowerShell

```powershell
Install-WindowsFeature Web-Server -IncludeManagementTools
Install-WindowsFeature Web-CGI
```

👉 Verify:

```powershell
iisreset
```

---

# 🐬 STEP 2: Install MySQL

### Setup

1. Install MySQL
2. Set:

   * Root user
   * Password
   * Port: `3306`

### Start MySQL

```bash
net start MySQL
```

---

# 🐘 STEP 3: Install PHP

### Steps:

1. Download PHP (ZIP)
2. Extract:

```bash
C:\PHP
```

3. Add to PATH

---

### Configure PHP

Edit:

```bash
C:\PHP\php.ini
```

Enable:

```ini
extension=mysqli
extension=pdo_mysql
```

---

# 🔗 STEP 4: Configure IIS for PHP

### Add Handler Mapping

IIS Manager → Handler Mappings → Add:

| Field        | Value              |
| ------------ | ------------------ |
| Request Path | *.php              |
| Module       | FastCgiModule      |
| Executable   | C:\PHP\php-cgi.exe |

---

# 📂 STEP 5: Project Structure

```bash
C:\inetpub\wwwroot\
    index.html
    insert.php
    view.php
```

---

# 📝 STEP 6: HTML Frontend

### index.html

```html
<!DOCTYPE html>
<html>
<head>
    <title>User Registration</title>
</head>
<body>

<h2>Register User</h2>

<form action="insert.php" method="post">
    Name: <input type="text" name="name"><br><br>
    Email: <input type="email" name="email"><br><br>
    <input type="submit" value="Submit">
</form>

<a href="view.php">View Users</a>

</body>
</html>
```

---

# 🗄️ STEP 7: MySQL Database Setup

```sql
CREATE DATABASE testdb;

USE testdb;

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100)
);
```

---

# 🧠 STEP 8: PHP – Insert Data

### insert.php

```php
<?php
$conn = new mysqli("localhost", "root", "password", "testdb");

if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

$name = $_POST['name'];
$email = $_POST['email'];

$sql = "INSERT INTO users (name, email) VALUES ('$name', '$email')";

if ($conn->query($sql) === TRUE) {
    echo "Data inserted successfully! <br>";
    echo "<a href='index.html'>Go Back</a>";
} else {
    echo "Error: " . $conn->error;
}

$conn->close();
?>
```

---

# 📊 STEP 9: PHP – View Data

### view.php

```php
<?php
$conn = new mysqli("localhost", "root", "password", "testdb");

$result = $conn->query("SELECT * FROM users");

echo "<h2>User List</h2>";

while($row = $result->fetch_assoc()) {
    echo "ID: " . $row["id"] . " | Name: " . $row["name"] . " | Email: " . $row["email"] . "<br>";
}

$conn->close();
?>
```

---

# ▶️ STEP 10: Run Project

Open browser:

```bash
http://localhost
```

✔ Submit form
✔ View data

---

# 🔥 STEP 11: Open Firewall Ports

| Service | Port |
| ------- | ---- |
| HTTP    | 80   |
| HTTPS   | 443  |
| MySQL   | 3306 |

---

# ⚠️ Troubleshooting

### ❌ PHP not working

* Check FastCGI mapping

### ❌ 500 Error

* Enable detailed errors in IIS

### ❌ MySQL connection error

* Check credentials
* Check MySQL service

---

# 🔐 Security Improvements (IMPORTANT)

❌ Current code is vulnerable (SQL Injection)

👉 Use Prepared Statements:

```php
$stmt = $conn->prepare("INSERT INTO users (name, email) VALUES (?, ?)");
$stmt->bind_param("ss", $name, $email);
$stmt->execute();
```

---

# 🚀 Production-Level Enhancements

● Use SSL (HTTPS)
● Use environment variables for DB password
● Deploy on Azure App Service
● Use CI/CD (GitHub Actions / Azure DevOps)
● Replace PHP with:

* ASP.NET Core (best for IIS)
* Node.js (modern stack)

---

# 🧠 Points to Remember (Interview + Training)

● HTML = Frontend
● PHP = Backend logic
● MySQL = Database
● IIS = Web server
● FastCGI = IIS ↔ PHP bridge

---

# 🎯 Real Use Cases

● Internal tools (HR, CRM)
● Student registration systems
● Training lab demos

---
