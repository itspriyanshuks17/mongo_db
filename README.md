# 🚀 MongoDB Local Development Setup

This repository uses **MongoDB** for local development.  
This guide explains how to install and run MongoDB on:

- 🐧 Ubuntu (via WSL)
- 🪟 Windows (native)
- 🍎 macOS (native)
- 🐳 Docker (recommended for teams)
- 🧩 Node.js connection example

---

## 📌 Prerequisites

- Git
- Terminal / Command Line access
- Admin / sudo permissions
- Node.js (for backend integration example)

---

# 🐳 Option A: MongoDB with Docker (Recommended)

> Best for teams and consistent environments.

### Install Docker
- Windows/macOS: https://www.docker.com/products/docker-desktop  
- Linux/WSL: https://docs.docker.com/engine/install/

Verify:
```bash
docker --version
docker compose version
````

### Docker Compose Setup

Create `docker-compose.yml`:

```yaml
version: "3.9"

services:
  mongodb:
    image: mongo:7
    container_name: mongodb
    restart: unless-stopped
    ports:
      - "27017:27017"
    volumes:
      - mongodb_data:/data/db
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: adminpassword
      MONGO_INITDB_DATABASE: appdb

volumes:
  mongodb_data:
```

Start MongoDB:

```bash
docker compose up -d
```

Connect:

```bash
mongosh "mongodb://admin:adminpassword@localhost:27017/appdb?authSource=admin"
```

Stop:

```bash
docker compose down
```

---

# 🐧 Option B: MongoDB on Ubuntu (WSL)

### 1️⃣ Check Ubuntu Version

```bash
lsb_release -a
```

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

### 4️⃣ Add MongoDB Repo (Ubuntu 22.04)

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

### 6️⃣ Start MongoDB (WSL)

```bash
sudo mongod --dbpath /var/lib/mongodb --logpath /var/log/mongodb/mongod.log --fork
```

### 7️⃣ Connect

```bash
mongosh
```

---

# 🪟 Option C: MongoDB on Windows (Native)

1. Download MongoDB Community Server:
   [https://www.mongodb.com/try/download/community](https://www.mongodb.com/try/download/community)
2. Install with:

   * ✔ Install as a Service
   * ✔ Install MongoDB Compass (optional GUI)

Start MongoDB (if not auto-started):

```powershell
net start MongoDB
```

Connect:

```powershell
mongosh
```

---

# 🍎 Option D: MongoDB on macOS

Install Homebrew (if needed):

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Install MongoDB:

```bash
brew tap mongodb/brew
brew install mongodb-community@7.0
```

Start MongoDB:

```bash
brew services start mongodb-community@7.0
```

Connect:

```bash
mongosh
```

---

# 🧩 Node.js + MongoDB Example

Install dependency:

```bash
npm install mongodb
```

Example `db.js`:

```js
import { MongoClient } from "mongodb";

const uri = process.env.MONGODB_URI || "mongodb://localhost:27017/appdb";

const client = new MongoClient(uri);

export async function connectDB() {
  await client.connect();
  console.log("✅ MongoDB connected");
  return client.db();
}
```

Example usage:

```js
import { connectDB } from "./db.js";

const db = await connectDB();
const users = db.collection("users");

await users.insertOne({ name: "Dev", role: "Engineer" });
console.log(await users.find().toArray());
```

Example `.env`:

```env
MONGODB_URI=mongodb://admin:adminpassword@localhost:27017/appdb?authSource=admin
```

---

# 🧪 Verify MongoDB

```bash
mongosh
```

```js
use appdb
db.health.insertOne({ status: "ok" })
db.health.find()
```

---

# 🛠 Troubleshooting

### Port already in use (27017)

```bash
lsof -i :27017
```

### WSL data directory error

```bash
sudo mkdir -p /var/lib/mongodb /var/log/mongodb
sudo chown -R mongodb:mongodb /var/lib/mongodb /var/log/mongodb
```

### Reset Docker MongoDB (⚠ Deletes data)

```bash
docker compose down -v
```

---

# 📚 Tools

* MongoDB Compass (GUI): [https://www.mongodb.com/products/tools/compass](https://www.mongodb.com/products/tools/compass)
* MongoDB Docs: [https://www.mongodb.com/docs/](https://www.mongodb.com/docs/)

---

# 🧠 Recommendation

For team projects and CI/CD:
👉 Use **Docker-based MongoDB**
For quick experiments:
👉 Native install is fine

---

Happy hacking! 😄
If you run into issues, feel free to open an issue or PR to improve this setup guide.

