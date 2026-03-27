# Chatify Backend - Node.js/Express Server

Backend server untuk Chatify yang dibangun dengan Express.js dan Socket.io untuk mendukung real-time messaging.

## 📋 Daftar Isi

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Struktur Folder](#struktur-folder)
- [Setup & Installation](#setup--installation)
- [Environment Variables](#environment-variables)
- [Database](#database)
- [API Endpoints](#api-endpoints)
- [Socket.io Events](#socketio-events)
- [Authentication](#authentication)
- [Error Handling](#error-handling)
- [Scripts](#scripts)

## 🎯 Overview

Backend Chatify adalah RESTful API yang menyediakan:

- Manajemen user dan autentikasi
- Manajemen percakapan (conversations)
- Manajemen pertemanan (friends)
- Real-time messaging dengan Socket.io
- Integrasi Google Generative AI
- Input validation dan error handling yang robust

## 🛠️ Tech Stack

- **Framework:** Express.js 5.1.0
- **Server:** Node.js with HTTP
- **Real-time:** Socket.io 4.8.1
- **Database:** PostgreSQL
- **ORM:** Sequelize 6.37.7
- **Authentication:** Firebase Admin, JWT
- **Validation:** Joi 17.13.3
- **Security:** bcryptjs 3.0.2
- **AI:** @google/genai 0.13.0
- **Utils:**
  - CORS (2.8.5)
  - dotenv (16.5.0)
  - nanoid (3.x)
- **Dev:**
  - Nodemon (3.1.10)
  - Jest (29.7.0)
  - Supertest (7.1.0)
  - Sequelize CLI (6.6.3)

## 📁 Struktur Folder

```
chatify-be/
├── controllers/
│   ├── ConversationController.js    # Logika conversation
│   └── FriendController.js          # Logika friend requests
├── models/
│   ├── conversation.js              # Model conversation
│   ├── friend.js                    # Model friend
│   └── index.js                     # Model exports
├── routers/
│   ├── ConversationRouter.js        # Routes untuk conversations
│   ├── FriendRouter.js              # Routes untuk friends
│   └── index.js                     # Router exports
├── middlewares/
│   ├── AuthMiddleware.js            # Authentication middleware
│   └── ErrorMiddleware.js           # Global error handler
├── validation/
│   └── [validation files]           # Input validation schemas
├── migrations/
│   └── [migration files]            # Database migrations
├── helpers/
│   ├── ResponseError.js             # Custom error class
│   └── [helper functions]
├── config/
│   ├── config.json                  # Database config
│   └── [config files]
├── data/
│   └── [seeders]                    # Database seeders
├── app.js                           # Application entry point
├── package.json
├── package-lock.json
├── .env.example
├── .gitignore
├── Command.txt                      # Useful commands
└── README.md                        # Dokumentasi ini
```

### Penjelasan Folder

#### `controllers/`

Berisi business logic untuk handling requests. Setiap controller menangani logika spesifik untuk resource tertentu.

**Files:**

- `ConversationController.js` - Handle conversation-related operations
  - `createConversation()` - Buat conversation baru
  - `getConversations()` - Ambil semua conversations user
  - `getMessages()` - Ambil messages dari conversation
  - `createMessage()` - Tambah message ke conversation
- `FriendController.js` - Handle friend-related operations
  - `sendFriendRequest()` - Kirim friend request
  - `acceptFriendRequest()` - Terima friend request
  - `getFriends()` - Ambil daftar teman

#### `models/`

Sequelize models yang mendefinisikan structure database.

**Models:**

- `conversation.js` - Model untuk conversations
- `friend.js` - Model untuk friend relationships
- `index.js` - Associations dan model exports

#### `routers/`

Express route definitions yang menggunakan controllers.

**Routes:**

- `ConversationRouter.js` - `/api/conversations` endpoints
- `FriendRouter.js` - `/api/friends` endpoints

#### `middlewares/`

Custom middleware untuk request processing.

**Middleware:**

- `AuthMiddleware.js` - Verify Firebase token, setup user info
- `ErrorMiddleware.js` - Global error handling dan formatting

#### `validation/`

Joi schemas untuk input validation.

#### `helpers/`

Utility functions dan custom error classes.

- `ResponseError.js` - Custom error class untuk API responses

## 🚀 Setup & Installation

### Prerequisites

- Node.js v14.0+
- npm atau yarn
- PostgreSQL database
- Firebase project dengan service account
- Google Cloud API key untuk Generative AI

### Installation Steps

```bash
# 1. Navigate ke folder backend
cd chatify-be

# 2. Install dependencies
npm install

# 3. Setup database
npm run create   # Run migrations
npm run seed     # Run seeders

# 4. Start development server
npm run ex       # Dengan auto-reload (nodemon)
```

### Verification

Server harus berjalan di `http://localhost:3000`:

```
Server with Socket.IO listening on port 3000
```

## ⚙️ Environment Variables

Buat file `.env` di root folder `chatify-be/`:

```env
# === Server Configuration ===
NODE_ENV=development
PORT=3000

# === Database Configuration ===
DB_HOST=localhost
DB_PORT=5432
DB_NAME=chatify_db
DB_USER=postgres
DB_PASSWORD=your_password
DB_DIALECT=postgres

# === Firebase Configuration ===
FIREBASE_PROJECT_ID=your-project-id
FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----"
FIREBASE_CLIENT_EMAIL=firebase-adminsdk@your-project.iam.gserviceaccount.com
FIREBASE_DATABASE_URL=https://your-project.firebaseio.com

# === Google Generative AI ===
GOOGLE_AI_API_KEY=your_google_ai_api_key

# === JWT Configuration ===
JWT_SECRET=your_very_secret_jwt_key_here
JWT_EXPIRES_IN=7d

# === API Keys ===
# (Add any other API keys needed)
```

### Mendapatkan Credentials

**Firebase Service Account:**

1. Buka Firebase Console
2. Project Settings → Service Accounts
3. Generate private key (JSON format)
4. Copy credentials ke `.env`

**Google AI API Key:**

1. Buka Google Cloud Console
2. Enable Generative AI API
3. Create API key dari Credentials page

## 🗄️ Database

### Database Setup

```bash
# Create database
createdb chatify_db

# Run migrations
npm run create

# Run seeders
npm run seed

# Reset database (undo all + create + seed)
npm run reset

# Undo migrations
npm run delete
```

### Database Schema

Lihat [Database Schema Documentation](../README.md#database-schema) untuk detail lengkap.

### Sequelize CLI Commands

```bash
# Generate model
npx sequelize-cli model:generate --name User --attributes name:string,email:string

# Generate migration
npx sequelize-cli migration:generate --name create-users-table

# Generate seeder
npx sequelize-cli seed:generate --name demo-users

# Run migrations
npx sequelize-cli db:migrate

# Undo last migration
npx sequelize-cli db:migrate:undo

# Run seeders
npx sequelize-cli db:seed:all
```

## 📡 API Endpoints

### Base URL

```
http://localhost:3000/api
```

### Authentication Required

Semua endpoint (kecuali auth) memerlukan Firebase JWT token di header:

```
Authorization: Bearer {firebase_id_token}
```

### Conversation Endpoints

```
GET /conversations
  - Get all conversations untuk current user
  - Query params: limit, offset
  - Response: [{ id, name, description, participants, lastMessage, ... }]

POST /conversations
  - Create new conversation
  - Body: { name, description, participantIds: [] }
  - Response: { id, name, description, ... }

GET /conversations/:id
  - Get conversation detail
  - Response: { id, name, description, participants, ... }

GET /conversations/:id/messages
  - Get all messages dalam conversation
  - Query params: limit, offset, sort
  - Response: { messages: [...], total, hasMore }

POST /conversations/:id/messages
  - Create message dalam conversation
  - Body: { content }
  - Response: { id, content, sender, timestamp, ... }

DELETE /conversations/:id
  - Delete conversation
  - Response: { message: "Conversation deleted" }

PUT /conversations/:id/leave
  - User leave conversation
  - Response: { message: "User left conversation" }
```

### Friend Endpoints

```
GET /friends
  - Get all friends user
  - Response: [{ id, user, friend, addedAt, ... }]

GET /friends/requests
  - Get pending friend requests
  - Response: { sent: [...], received: [...] }

POST /friends/requests
  - Send friend request
  - Body: { friendId }
  - Response: { id, status: "pending", ... }

PUT /friends/requests/:id/accept
  - Accept friend request
  - Response: { message: "Friend request accepted" }

PUT /friends/requests/:id/reject
  - Reject friend request
  - Response: { message: "Friend request rejected" }

DELETE /friends/:id
  - Remove friend
  - Response: { message: "Friend removed" }
```

## 🔌 Socket.io Events

### Authentication

```javascript
// Client connect dengan token
const socket = io(SERVER_URL, {
  auth: {
    token: firebaseIdToken,
  },
});
```

### Events

**Client → Server:**

```javascript
// Join room untuk conversation
socket.emit("join-room", conversationId);

// Send message ke room
socket.emit("send-message", {
  roomId: conversationId,
  message: {
    content: "Hello",
    // other message data
  },
});

// Leave room
socket.emit("leave-room", conversationId);
```

**Server → Client:**

```javascript
// Receive message
socket.on("receive-message", (messageData) => {
  console.log("New message:", messageData);
});

// User joined room
socket.on("user-joined", (userData) => {
  console.log("User joined:", userData);
});

// User left room
socket.on("user-left", (userId) => {
  console.log("User left:", userId);
});

// Error event
socket.on("error", (errorMessage) => {
  console.error("Socket error:", errorMessage);
});
```

## 🔐 Authentication

### Flow

1. **Frontend**: User login dengan Firebase
2. **Frontend**: Get Firebase ID token
3. **Frontend**: Send token ke backend API dalam header `Authorization: Bearer {token}`
4. **Backend**: Verify token menggunakan Firebase Admin SDK
5. **Backend**: Attach user info ke request object
6. **Backend**: Process request dengan user context

### Firebase Token Verification

```javascript
// Di AuthMiddleware.js
const payload = await admin.auth().verifyIdToken(token);
// payload contains: uid, email, email_verified, name, etc.
```

### Socket.io Authentication

```javascript
// Socket authentication di app.js
io.use(async (socket, next) => {
  const token = socket.handshake.auth.token;
  const payload = await admin.auth().verifyIdToken(token);
  socket.user = payload;
  next();
});
```

## ❌ Error Handling

### Custom Error Class

```javascript
// ResponseError.js
class ResponseError extends Error {
  constructor(message, status) {
    super(message);
    this.status = status;
  }
}

// Usage
throw new ResponseError("User not found", 404);
throw new ResponseError("Unauthorized access", 401);
```

### Error Middleware

```javascript
// Catches all errors dan format response
app.use(ErrorMiddleware);

// Response format:
{
  status: 400,
  message: 'Error message',
  details: { ... } // optional
}
```

## 📦 Scripts

```bash
# Development
npm run ex              # Start dengan nodemon (hot reload)

# Database
npm run create          # Run migrations
npm run seed            # Run seeders
npm run delete          # Undo all migrations
npm run reset           # Reset database (delete + create + seed)

# Testing
npm test                # Run Jest tests

# Other
npm start               # Start production server (plain node)
```

## 🧪 Testing

```bash
# Run all tests
npm test

# Run specific test file
npm test -- ConversationController.test.js

# Watch mode
npm test -- --watch
```

## 🔧 Development Tips

### Debugging

```javascript
// Add console logging
console.log("Variable:", variable);

// Use debugger
debugger;
// Then run: node --inspect app.js
```

### Common Issues

1. **Database Connection Error**
   - Check `.env` database credentials
   - Ensure PostgreSQL is running
   - Check database exists

2. **Firebase Token Invalid**
   - Verify service account credentials in `.env`
   - Check token not expired
   - Ensure correct project ID

3. **Socket.io Connection Failed**
   - Check CORS settings
   - Verify frontend URL in CORS origin
   - Check firewall/network

## 📚 Useful Resources

- [Express.js Documentation](https://expressjs.com/)
- [Sequelize Documentation](https://sequelize.org/)
- [Socket.io Documentation](https://socket.io/docs/)
- [Firebase Admin SDK](https://firebase.google.com/docs/database/admin/start)
- [Google Generative AI](https://ai.google.dev/)

---

**Last Updated:** March 27, 2026

Untuk bantuan lebih, lihat [Main README](./README.md)
