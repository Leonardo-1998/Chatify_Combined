# Chatify Combined Documentation

Dokumentasi ini adalah dokumentasi gabungan di folder `Group_Project`.
README di dalam `chatify-be` dan `chatify-fe` tetap dipakai sebagai dokumentasi khusus masing-masing service.

## Ringkasan

Chatify adalah aplikasi chat real-time yang terdiri dari:

- Backend `Node.js + Express + Socket.IO + Sequelize`
- Frontend `React + Vite + Firebase Auth + React Query`
- Fitur bantuan balasan chat menggunakan Google Gemini AI

## Struktur Folder

```text
Group_Project/
├── chatify-be/
│   ├── app.js
│   ├── controllers/
│   ├── middlewares/
│   ├── models/
│   ├── migrations/
│   └── routers/
├── chatify-fe/
│   ├── src/
│   ├── firebase.js
│   └── vite.config.js
└── README.md
```

## Arsitektur Singkat

1. Frontend login via Firebase Auth.
2. Frontend mengambil Firebase ID token dan mengirimnya ke backend di header `Authorization: Bearer <token>`.
3. Backend validasi token dengan Firebase Admin di `chatify-be/middlewares/AuthMiddleware.js`.
4. Untuk chat real-time, frontend connect ke Socket.IO dan mengirim token lewat `socket.auth.token`.
5. Backend memproses event room/message dan broadcast event `receive-message`.

## Endpoint Backend (Sesuai Kode Saat Ini)

Base URL: `http://localhost:3000`

Semua endpoint di bawah ini berada di belakang `AuthMiddleware`.

### Friend

- `GET /api/friends`
  - Ambil semua friend room milik user login.
- `POST /api/friends/request`
  - Body: `{ "email": "target@mail.com" }`
  - Membuat relasi friend dan `roomId` baru.
- `DELETE /api/friends/delete/:roomId`
  - Hapus friend relation berdasarkan `roomId`.
- `GET /api/friends/:email`
  - Cari data Firebase user berdasarkan email.

### Conversation

- `GET /api/conversations/:roomId`
  - Ambil semua message dalam room (ascending by createdAt).
- `POST /api/conversations/analyze-chat`
  - Body: `{ "roomId": "...", "message": "..." }`
  - Generate saran balasan via Gemini dari histori percakapan room.

## Socket.IO Events (Sesuai Kode Saat Ini)

Client ke Server:

- `join-room` dengan payload `roomId`
- `send-message` dengan payload `{ roomId, message }`
- `leave-room` dengan payload `roomId`

Server ke Client:

- `receive-message` dengan data message baru
- `error` untuk error umum

## Data Model (PostgreSQL via Sequelize)

### Table `Friends`

- `id` (integer, auto increment)
- `roomId` (string, unique)
- `uid` (string)
- `friendUId` (string)
- `createdAt`, `updatedAt`

### Table `Conversations`

- `id` (integer, auto increment)
- `roomId` (string, FK ke `Friends.roomId`, cascade)
- `senderUid` (string)
- `message` (text)
- `createdAt`, `updatedAt`

## Setup Cepat

## 1) Backend

```bash
cd chatify-be
npm install
```

Buat `.env` (lihat `chatify-be/.env.example`):

```env
NODE_ENV=development
PROJECT_ID=your-firebase-project-id
GEMINI_API_KEY=your-gemini-api-key
PORT=3000
```

Catatan penting:

- `AuthMiddleware` memerlukan file service account JSON Firebase di root backend.
- File yang dirujuk kode saat ini: `chatify-be/group-project-459009-firebase-adminsdk-fbsvc-82ad1847c2.json`.

Jalankan backend:

```bash
npm run ex
```

## 2) Frontend

```bash
cd chatify-fe
npm install
```

Buat `.env` (lihat `chatify-fe/.env.example`):

```env
VITE_API_KEY=...
VITE_AUTH_DOMAIN=...
VITE_PROJECT_ID=...
VITE_STORAGE_BUCKET=...
VITE_MESSAGING_SENDER_ID=...
VITE_APP_ID=...
VITE_MEASUREMENT_IDmeasurementId=...
VITE_SERVER_BASE_URL=http://localhost:3000
```

Jalankan frontend:

```bash
npm run dev
```

## Alur Frontend Singkat

Routes di `chatify-fe/src/main.jsx`:

- `/login` -> `LoginPage`
- `/register` -> `RegisterPage`
- `/` -> `Home` (dibungkus `ProtectedPage`)

Di halaman `Home`:

- Sidebar menampilkan kontak/friend room.
- Klik kontak memanggil `GET /api/conversations/:roomId`.
- Kirim pesan lewat socket `send-message`.
- Tombol AI memanggil `POST /api/conversations/analyze-chat` untuk saran respons.

## Source of Truth

Dokumen ini mengikuti implementasi kode saat ini. Jika ada perubahan endpoint atau flow, update berdasarkan file berikut:

- `chatify-be/app.js`
- `chatify-be/routers/FriendRouter.js`
- `chatify-be/routers/ConversationRouter.js`
- `chatify-be/controllers/FriendController.js`
- `chatify-be/controllers/ConversationController.js`
- `chatify-fe/src/main.jsx`
- `chatify-fe/src/Pages/Home.jsx`
- `chatify-fe/src/server/FriendServer.js`
- `chatify-fe/src/server/ConversationServer.js`

## Dokumentasi Per Service

- `chatify-be/README.md`
- `chatify-fe/README.md`

Last updated: 2026-03-27
