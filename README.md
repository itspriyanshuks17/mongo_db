# 🚀 MongoDB Local Setup Guide (Windows, macOS, WSL Ubuntu)

This guide helps developers install and run **MongoDB locally** on:

- 🐧 Ubuntu (via WSL on Windows)
- 🪟 Native Windows
- 🍎 macOS

Follow the section for your operating system.

---

## 📌 Prerequisites

- Basic terminal / command line knowledge  
- Admin or sudo access  
- Internet connection  

---

# 🐧 MongoDB on Ubuntu (WSL)

> Recommended: WSL2 with Ubuntu 20.04 / 22.04

### 1️⃣ Check Ubuntu Version

```bash
lsb_release -a
````

### 2️⃣ Install Dependencies

```bash
sudo apt update
sudo apt install -y gnupg curl
```

### 3️⃣ Add MongoDB GPG Key

```bash
curl -fsSL https://pgp.mongodb.com/server-7.0.asc | \
  sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor
```

### 4️⃣ Add MongoDB Repository (Ubuntu 22.04 Example)

```bash
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] \
https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | \
sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

```bash
sudo apt update
```

### 5️⃣ Install MongoDB

```bash
sudo apt install -y mongodb-org
```

### 6️⃣ Start MongoDB (WSL Note)

WSL does not support `systemctl` by default. Start MongoDB manually:

```bash
sudo mongod --dbpath /var/lib/mongodb --logpath /var/log/mongodb/mongod.log --fork
```

### 7️⃣ Connect to MongoDB

```bash
mongosh
```

---

# 🪟 MongoDB on Windows

### 1️⃣ Download MongoDB

Download the MSI installer from:
👉 [https://www.mongodb.com/try/download/community](https://www.mongodb.com/try/download/community)

Choose:

* Version: Latest Stable
* Platform: Windows
* Package: MSI

### 2️⃣ Install MongoDB

* Run the installer
* Choose **Complete Setup**
* Enable **Install MongoDB as a Service**
* Optionally install **MongoDB Compass (GUI)**

### 3️⃣ Start MongoDB Service

MongoDB usually starts automatically.
If not:

```powershell
net start MongoDB
```

### 4️⃣ Connect to MongoDB

Open Command Prompt or PowerShell:

```powershell
mongosh
```

---

# 🍎 MongoDB on macOS

### 1️⃣ Install Homebrew (if not installed)

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 2️⃣ Tap MongoDB Formula

```bash
brew tap mongodb/brew
```

### 3️⃣ Install MongoDB

```bash
brew install mongodb-community@7.0
```

### 4️⃣ Start MongoDB

```bash
brew services start mongodb-community@7.0
```

Or run manually:

```bash
mongod --config /opt/homebrew/etc/mongod.conf
```

### 5️⃣ Connect to MongoDB

```bash
mongosh
```

---

# ✅ Verify Installation (All Platforms)

```bash
mongosh
```

```js
use testdb
db.users.insertOne({ name: "Developer", status: "MongoDB works!" })
db.users.find()
```

If you see your inserted document, MongoDB is working correctly 🎉

---

# 🛠 Common Issues

### ❌ Port Already in Use (27017)

```bash
lsof -i :27017
```

### ❌ MongoDB Fails to Start on WSL

```bash
sudo mkdir -p /var/lib/mongodb /var/log/mongodb
sudo chown -R mongodb:mongodb /var/lib/mongodb /var/log/mongodb
```

---

# 📚 Useful Links

* MongoDB Docs: [https://www.mongodb.com/docs/](https://www.mongodb.com/docs/)
* MongoDB Compass (GUI): [https://www.mongodb.com/products/tools/compass](https://www.mongodb.com/products/tools/compass)

---

# 🧑‍💻 Recommended Usage

This setup is ideal for:

* Local development
* Node.js / Express apps
* Python / FastAPI backends
* Testing APIs with MongoDB

---

Happy hacking! 😄
If you hit any issues, open an issue or PR to improve this guide.
