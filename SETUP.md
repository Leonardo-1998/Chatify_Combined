# Setup & Quick Start Guide

Panduan cepat untuk setup project Chatify dari awal.

## 🚀 Quick Start (5 Menit)

### Prasyarat Cepat

```bash
# Pastikan Node.js terinstall
node --version  # v14.0+

# Pastikan npm terinstall
npm --version   # v6.0+
```

### 1. Backend Setup

```bash
cd chatify-be

# Install dependencies
npm install

# Copy environment file
cp .env.example .env

# Edit .env dengan database credentials Anda
# (gunakan text editor favorit)

# Setup database
npm run reset    # Initial setup: migrate + seed

# Start server
npm run ex       # Server berjalan di http://localhost:3000
```

### 2. Frontend Setup (terminal baru)

```bash
cd chatify-fe

# Install dependencies
npm install

# Copy environment file
cp .env.example .env

# Edit .env dengan Firebase credentials Anda

# Start dev server
npm run dev      # App berjalan di http://localhost:5173
```

### 3. Buka Browser

Buka `http://localhost:5173` dan aplikasi sudah siap digunakan!

---

## 📋 Detailed Setup

### Database Setup

#### PostgreSQL Installation

**Windows (Admin):**

```bash
# Menggunakan Windows Subsystem for Linux (WSL) - recommended
# Or download dari: https://www.postgresql.org/download/windows/

# Setelah install, buat database
createdb chatify_db

# Login ke psql
psql -U postgres
```

**macOS:**

```bash
brew install postgresql@15
brew services start postgresql@15
createdb chatify_db
```

**Linux (Ubuntu):**

```bash
sudo apt-get install postgresql postgresql-contrib
sudo -u postgres createdb chatify_db
```

#### Running Migrations

```bash
cd chatify-be

# Run all pending migrations
npm run create

# Verify migrations ran successfully
npx sequelize-cli db:migrate:status

# Run seeders
npm run seed
```

### Firebase Setup

#### Create Firebase Project

1. Buka https://console.firebase.google.com
2. Click "Create a new project"
3. Pilih "Create project"
4. Enable authentication methods yang diperlukan

#### Backend - Get Service Account Key

1. Go to Project Settings → Service Accounts
2. Click "Generate New Private Key"
3. Save JSON file
4. Copy credentials ke `.env` file

**Example:**

```env
FIREBASE_PROJECT_ID=chatify-prod
FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\nMIIEvQIBA..."
FIREBASE_CLIENT_EMAIL=firebase-adminsdk@chatify-prod.iam.gserviceaccount.com
```

#### Frontend - Get Web Config

1. Go to Project Settings → General
2. Scroll ke "Your apps"
3. Click web app icon
4. Copy config
5. Add ke `.env` file

**Example:**

```env
VITE_FIREBASE_API_KEY=AIzaSyB1234567890
VITE_FIREBASE_AUTH_DOMAIN=chatify-prod.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=chatify-prod
VITE_FIREBASE_STORAGE_BUCKET=chatify-prod.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=123456789
VITE_FIREBASE_APP_ID=1:123456789:web:abcdef1234567890
```

### Google Generative AI Setup

1. Buka https://console.cloud.google.com
2. Create atau select existing project
3. Enable "Generative Language API"
4. Create API key di Credentials page
5. Copy ke `.env`

```env
GOOGLE_AI_API_KEY=AIzaSyD1234567890...
```

---

## ✅ Verification Checklist

### Backend Checks

```bash
cd chatify-be

# 1. Check Node installation
node --version  # Should be v14+

# 2. Check dependencies
npm list        # Should show all packages

# 3. Check environment
cat .env        # Should show all required variables

# 4. Check database connection
npm run ex      # Should show "Server listening on port 3000"
```

### Frontend Checks

```bash
cd chatify-fe

# 1. Check Vite
npm run dev     # Should show local dev server

# 2. Check build
npm run build   # Should complete without errors

# 3. Check linting
npm run lint    # Should have no critical errors
```

---

## 🔧 Common Setup Issues

### PostgreSQL Connection Error

**Error:** `Error: connect ECONNREFUSED 127.0.0.1:5432`

**Solution:**

```bash
# macOS
brew services start postgresql@15

# Linux
sudo service postgresql start

# Windows (WSL)
sudo service postgresql start
# Or create database manually
```

### Firebase Authentication Failed

**Error:** `Error: FIREBASE_PRIVATE_KEY is missing`

**Solution:**

- Verify `.env` file exists
- Verify all FIREBASE\_\* variables are set
- Verify private key formatting (watch for newlines)
- Restart dev server

### Port Already in Use

**Error:** `Error: listen EADDRINUSE: address already in use :::3000`

**Solution:**

```bash
# Kill process on port 3000
# Windows
netstat -ano | findstr :3000  # Find PID
taskkill /PID <PID> /F         # Kill it

# macOS/Linux
lsof -i :3000                  # Find process
kill -9 <PID>                  # Kill it

# Or change port in .env
PORT=3001
```

### npm Dependencies Error

**Error:** `npm ERR! permission denied`

**Solution:**

```bash
# Clear npm cache
npm cache clean --force

# Reinstall dependencies
rm -rf node_modules package-lock.json
npm install

# Or use sudo (not recommended)
sudo npm install
```

---

## 📁 Project Structure After Setup

```
Group_Project/
├── chatify-be/
│   ├── node_modules/      ← Installed packages
│   ├── .env               ← Your environment variables
│   └── ...
├── chatify-fe/
│   ├── node_modules/      ← Installed packages
│   ├── .env               ← Your environment variables
│   └── ...
└── README.md
```

---

## 🚀 Development Workflow

### Daily Startup

```bash
# Terminal 1 - Backend
cd chatify-be
npm run ex           # Auto-reloads on file changes

# Terminal 2 - Frontend (CTRL+` untuk buka terminal baru)
cd chatify-fe
npm run dev          # Auto-reloads on file changes

# Terminal 3 (Optional - untuk git atau CLI lainnya)
cd Group_Project
git status
```

### Making Changes

1. Edit code di editor (VS Code auto-saves)
2. File akan auto-reload di browser/terminal
3. Check console untuk errors
4. Test di browser

### Committing Changes

```bash
# Check status
git status

# Add files
git add .

# Commit
git commit -m "feat: add new feature description"

# Push
git push origin branch-name
```

---

## 📝 Environment File Templates

### Backend (.env.example → .env)

Copy dari [chatify-be/.env.example](./chatify-be/.env.example) atau:

```env
NODE_ENV=development
PORT=3000

DB_HOST=localhost
DB_PORT=5432
DB_NAME=chatify_db
DB_USER=postgres
DB_PASSWORD=password

FIREBASE_PROJECT_ID=your-id
FIREBASE_PRIVATE_KEY=your-key
FIREBASE_CLIENT_EMAIL=your-email

GOOGLE_AI_API_KEY=your-key

JWT_SECRET=your-secret
JWT_EXPIRES_IN=7d
```

### Frontend (.env.example → .env)

Copy dari [chatify-fe/.env.example](./chatify-fe/.env.example) atau:

```env
VITE_API_URL=http://localhost:3000
VITE_SOCKET_URL=http://localhost:3000

VITE_FIREBASE_API_KEY=your-key
VITE_FIREBASE_AUTH_DOMAIN=your-domain
VITE_FIREBASE_PROJECT_ID=your-id
VITE_FIREBASE_STORAGE_BUCKET=your-bucket
VITE_FIREBASE_MESSAGING_SENDER_ID=your-id
VITE_FIREBASE_APP_ID=your-id
```

---

## 🆘 Need Help?

### Check Logs

```bash
# Terminal output di console
# Check untuk error messages

# Browser console
# Press F12 → Console tab
# Look untuk JavaScript errors
```

### Common Commands

```bash
# Backend database
npm run reset     # Reset database completely
npm run delete    # Undo all migrations
npm run create    # Run migrations
npm run seed      # Run seeders

# Frontend
npm run lint      # Check code quality
npm run build     # Create production build
npm run preview   # Preview production build
```

### Resources

- [Main Documentation](./README.md)
- [Backend Docs](./chatify-be/README.md)
- [Frontend Docs](./chatify-fe/README.md)
- [Firebase Setup Guide](https://firebase.google.com/docs)

---

**Last Updated:** March 27, 2026

Silakan buat issue atau tanya di discussion jika ada kesulitan!
