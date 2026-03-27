# Chatify - Real-Time Chat Application with AI Integration

Chatify adalah aplikasi chat modern yang dilengkapi dengan integrasi AI (Google Generative AI) untuk memberikan pengalaman komunikasi yang lebih baik. Aplikasi ini dibangun menggunakan teknologi full-stack modern dengan real-time messaging capabilities.

## 📋 Daftar Isi

- [Overview](#overview)
- [Fitur](#fitur)
- [Tech Stack](#tech-stack)
- [Struktur Project](#struktur-project)
- [Instalasi](#instalasi)
- [Konfigurasi](#konfigurasi)
- [Menjalankan Project](#menjalankan-project)
- [API Documentation](#api-documentation)
- [Database Schema](#database-schema)
- [Contributing](#contributing)

## 🎯 Overview

Chatify adalah aplikasi chat real-time yang memungkinkan pengguna untuk:

- Berkomunikasi secara langsung dengan pengguna lain
- Menambah dan mengelola daftar teman
- Berinteraksi dengan AI assistant untuk bantuan
- Mengalami komunikasi real-time dengan fitur Socket.io
- Autentikasi aman menggunakan Firebase

**Struktur Folder:**

```
Group_Project/
├── chatify-be/          # Backend (Node.js + Express)
├── chatify-fe/          # Frontend (React + Vite)
└── README.md            # Dokumentasi ini
```

## ✨ Fitur

### Backend

- ✅ Real-time messaging dengan Socket.io
- ✅ Sistem autentikasi Firebase
- ✅ Manajemen pertemanan (friend system)
- ✅ Manajemen percakapan
- ✅ Integrasi Google Generative AI
- ✅ Database PostgreSQL dengan Sequelize ORM
- ✅ Error handling dan validation dengan Joi
- ✅ JWT token untuk API authentication

### Frontend

- ✅ Interface chat modern dengan Bootstrap
- ✅ Real-time message updates
- ✅ User authentication dengan Firebase
- ✅ State management dengan Zustand
- ✅ Data fetching dengan React Query
- ✅ Form handling dengan React Hook Form
- ✅ Validasi dengan Zod
- ✅ Icon system dengan Font Awesome

## 🛠️ Tech Stack

### Backend

- **Runtime:** Node.js
- **Framework:** Express.js 5.1.0
- **Database:** PostgreSQL
- **ORM:** Sequelize
- **Real-time:** Socket.io
- **Authentication:** Firebase Admin SDK, JWT
- **Validation:** Joi
- **Security:** bcryptjs
- **AI:** @google/genai
- **Dev Tools:** Nodemon, Jest, Supertest

### Frontend

- **Framework:** React 19
- **Build Tool:** Vite
- **Language:** JavaScript (ES Modules)
- **State Management:** Zustand
- **Data Fetching:** React Query, Axios
- **UI Framework:** Bootstrap 5
- **Form Management:** React Hook Form
- **Validation:** Zod
- **Real-time:** Socket.io Client
- **Authentication:** Firebase SDK
- **Icons:** Font Awesome
- **Linting:** ESLint

## 📁 Struktur Project

### Backend Structure (chatify-be/)

```
chatify-be/
├── controllers/           # Business logic untuk routes
│   ├── ConversationController.js
│   └── FriendController.js
├── models/               # Database models
│   ├── conversation.js
│   ├── friend.js
│   └── index.js
├── routers/              # API routes
│   ├── ConversationRouter.js
│   └── FriendRouter.js
├── middlewares/          # Custom middleware
│   ├── AuthMiddleware.js
│   └── ErrorMiddleware.js
├── validation/           # Input validation
├── migrations/           # Database migrations
├── helpers/              # Helper functions
├── config/               # Configuration files
├── data/                 # Seed data
├── app.js                # Entry point
├── package.json
├── .env.example
└── Command.txt
```

### Frontend Structure (chatify-fe/)

```
chatify-fe/
├── src/
│   ├── components/       # Reusable React components
│   ├── Pages/            # Page components
│   ├── Contexts/         # React context
│   ├── hooks/            # Custom React hooks
│   ├── utils/            # Utility functions
│   ├── data/             # Static data
│   ├── server/           # API calls
│   ├── main.jsx          # Entry point
│   └── App.jsx
├── public/               # Static assets
├── vite.config.js
├── eslint.config.js
├── firebase.js           # Firebase configuration
├── package.json
├── .env.example
└── index.html
```

## 🚀 Instalasi

### Prerequisites

- Node.js (v14.0 atau lebih tinggi)
- npm atau yarn
- PostgreSQL database
- Firebase project
- Google Cloud project untuk Generative AI

### Step 1: Clone Project

```bash
cd Group_Project
```

### Step 2: Install Backend Dependencies

```bash
cd chatify-be
npm install
```

### Step 3: Install Frontend Dependencies

```bash
cd ../chatify-fe
npm install
```

## ⚙️ Konfigurasi

### Backend Configuration

#### 1. Setup Environment Variables

Buat file `.env` di folder `chatify-be/`:

```env
# Server
NODE_ENV=development
PORT=3000

# Database
DB_HOST=localhost
DB_PORT=5432
DB_NAME=chatify_db
DB_USER=your_postgres_user
DB_PASSWORD=your_postgres_password
DB_DIALECT=postgres

# Firebase
FIREBASE_PROJECT_ID=your_project_id
FIREBASE_PRIVATE_KEY=your_private_key
FIREBASE_CLIENT_EMAIL=your_client_email

# Google Generative AI
GOOGLE_AI_API_KEY=your_google_ai_key

# JWT
JWT_SECRET=your_jwt_secret_key
```

#### 2. Database Setup

Jalankan migrasi dan seeding:

```bash
npm run create           # Run migrations
npm run seed             # Run seeders
```

#### 3. Reset Database (jika diperlukan)

```bash
npm run reset            # Undo semua migrations, run migrations, dan seed
```

### Frontend Configuration

#### 1. Setup Environment Variables

Buat file `.env` di folder `chatify-fe/`:

```env
VITE_API_URL=http://localhost:3000
VITE_SOCKET_URL=http://localhost:3000

# Firebase
VITE_FIREBASE_API_KEY=your_api_key
VITE_FIREBASE_AUTH_DOMAIN=your_auth_domain
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_storage_bucket
VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
VITE_FIREBASE_APP_ID=your_app_id
```

## 🏃 Menjalankan Project

### Backend

```bash
cd chatify-be

# Development mode (dengan auto-reload)
npm run ex

# Test
npm test
```

Server akan berjalan di `http://localhost:3000`

### Frontend

```bash
cd chatify-fe

# Development mode
npm run dev

# Production build
npm run build

# Preview build
npm run preview

# Linting
npm run lint
```

Frontend akan berjalan di `http://localhost:5173` (default Vite)

## 📡 API Documentation

### Authentication Routes

```
POST   /api/auth/register        - Register user
POST   /api/auth/login           - Login user
POST   /api/auth/logout          - Logout user
```

### Friend Routes

```
GET    /api/friends              - Get all friends
POST   /api/friends              - Add friend
DELETE /api/friends/:id          - Remove friend
GET    /api/friends/requests     - Get friend requests
POST   /api/friends/requests     - Send friend request
PUT    /api/friends/requests/:id - Accept/Reject friend request
```

### Conversation Routes

```
GET    /api/conversations              - Get all conversations
POST   /api/conversations              - Create conversation
GET    /api/conversations/:id          - Get conversation by ID
POST   /api/conversations/:id/messages - Create message
GET    /api/conversations/:id/messages - Get conversation messages
DELETE /api/conversations/:id          - Delete conversation
```

### Socket Events

**Client → Server:**

- `join-room` - Join conversation room
- `send-message` - Send message to room
- `leave-room` - Leave conversation room

**Server → Client:**

- `receive-message` - Receive new message
- `error` - Error event
- `user-joined` - User joined room
- `user-left` - User left room

## 🗄️ Database Schema

### Users Table

```sql
- id (Primary Key)
- email (Unique)
- name
- avatar
- firebaseUid (Firebase UID)
- createdAt
- updatedAt
```

### Conversations Table

```sql
- id (Primary Key)
- name
- description
- createdBy (Foreign Key → Users)
- createdAt
- updatedAt
```

### ConversationUsers Table (Junction)

```sql
- conversationId (Foreign Key)
- userId (Foreign Key)
- createdAt
- updatedAt
```

### Messages Table

```sql
- id (Primary Key)
- conversationId (Foreign Key)
- userId (Foreign Key)
- content
- createdAt
- updatedAt
```

### Friends Table

```sql
- id (Primary Key)
- userId (Foreign Key)
- friendId (Foreign Key)
- status (pending/accepted/blocked)
- createdAt
- updatedAt
```

## 🧪 Testing

### Backend Testing

```bash
cd chatify-be
npm test
```

### Frontend Linting

```bash
cd chatify-fe
npm run lint
```

## 📚 Dokumentasi Tambahan

Lihat dokumentasi detail untuk setiap bagian:

- [Backend Documentation](./chatify-be/README.md) - Dokumentasi lengkap backend
- [Frontend Documentation](./chatify-fe/README.md) - Dokumentasi lengkap frontend

## 🤝 Contributing

Silakan berkontribusi dengan:

1. Fork repository
2. Buat branch fitur (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add some AmazingFeature'`)
4. Push ke branch (`git push origin feature/AmazingFeature`)
5. Buka Pull Request

## 📝 Lisensi

ISC License

## 👥 Tim Pengembang

Project ini adalah bagian dari Group Project di Hacktive8 Phase 2.

---

**Last Updated:** March 27, 2026

Untuk pertanyaan atau bantuan lebih lanjut, silakan buat issue atau hubungi tim development.
#   C h a t i f y _ C o m b i n e d  
 