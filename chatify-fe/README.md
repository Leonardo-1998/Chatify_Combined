# Chatify Frontend - React + Vite Application

Frontend application untuk Chatify yang dibangun dengan React 19 dan Vite untuk pengalaman development yang cepat dan modern.

## 📋 Daftar Isi

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Struktur Folder](#struktur-folder)
- [Setup & Installation](#setup--installation)
- [Environment Variables](#environment-variables)
- [Running the App](#running-the-app)
- [Project Structure](#project-structure)
- [Components](#components)
- [Pages](#pages)
- [Hooks](#hooks)
- [State Management](#state-management)
- [API Integration](#api-integration)
- [Socket.io Integration](#socketio-integration)
- [Development Guide](#development-guide)

## 🎯 Overview

Frontend Chatify adalah single-page application (SPA) yang menyediakan:

- User interface untuk real-time messaging
- Autentikasi dengan Firebase
- Manajemen friends/kontak
- Conversation management
- Real-time updates dengan Socket.io
- Responsive design dengan Bootstrap

**Features:**

- ✅ Real-time chat messaging
- ✅ Friend request management
- ✅ Conversation creation & management
- ✅ User authentication
- ✅ Responsive & modern UI
- ✅ State persistence dengan Zustand
- ✅ Form validation dengan React Hook Form & Zod

## 🛠️ Tech Stack

- **Framework:** React 19.1.0
- **Build Tool:** Vite 6.3.5
- **Language:** JavaScript (ES Modules)
- **State Management:** Zustand 5.0.4
- **Data Fetching:**
  - React Query (@tanstack/react-query 5.75.5)
  - Axios 1.9.0
- **UI Framework:** Bootstrap 5.3.6 + React-Bootstrap 2.10.9
- **Form Management:** React Hook Form 7.56.2
- **Validation:** Zod 3.24.4 + @hookform/resolvers 5.0.1
- **Routing:** React Router 7.5.3
- **Real-time:** Socket.io Client 4.8.1
- **Authentication:** Firebase 11.6.1
- **State Immutability:** Immer 10.1.1
- **Icons:** Font Awesome (React 6.7.2)
- **Utilities:** Moment.js 2.30.1
- **Alerts:** SweetAlert2 11.21.0
- **Dev Tools:** ESLint 9.25.0, Vite Plugin React 4.4.1

## 📁 Struktur Folder

```
chatify-fe/
├── src/
│   ├── components/           # Reusable React components
│   │   ├── ChatBox.jsx       # Main chat interface
│   │   ├── Sidebar.jsx       # Sidebar navigation
│   │   ├── UserCard.jsx      # User display card
│   │   ├── MessageList.jsx   # Message rendering
│   │   └── [other components]
│   ├── Pages/                # Page components (routed)
│   │   ├── Home.jsx          # Home page
│   │   ├── Login.jsx         # Login page
│   │   ├── Register.jsx      # Registration page
│   │   ├── Chat.jsx          # Chat page
│   │   ├── Friends.jsx       # Friends management
│   │   └── [other pages]
│   ├── Contexts/             # React Context API
│   │   └── [context providers]
│   ├── hooks/                # Custom React hooks
│   │   ├── useAuth.js        # Authentication hook
│   │   ├── useSocket.js      # Socket.io hook
│   │   ├── useChat.js        # Chat operations hook
│   │   └── [other hooks]
│   ├── utils/                # Utility functions
│   │   ├── api.js            # API configuration
│   │   ├── formatters.js     # Data formatting utilities
│   │   └── [other utilities]
│   ├── server/               # API integration
│   │   ├── conversation.js   # Conversation API calls
│   │   ├── auth.js           # Authentication API
│   │   ├── friends.js        # Friends API
│   │   └── [other APIs]
│   ├── data/                 # Static data/constants
│   │   └── [constants file]
│   ├── main.jsx              # Entry point
│   ├── App.jsx               # Root component
│   └── index.css             # Global styles
├── public/                   # Static assets
│   ├── images/
│   └── [other assets]
├── firebase.js               # Firebase configuration
├── vite.config.js            # Vite configuration
├── eslint.config.js          # ESLint configuration
├── package.json
├── package-lock.json
├── .env.example
├── .gitignore
├── index.html                # HTML entry point
└── README.md                 # Dokumentasi ini
```

### Penjelasan Folder

#### `src/components/`

Reusable React components yang digunakan di multiple pages.

**Contoh Components:**

- `ChatBox.jsx` - Main chat interface dengan message input
- `Sidebar.jsx` - Navigation sidebar
- `MessageList.jsx` - List of messages
- `UserCard.jsx` - User profile card
- `FriendRequest.jsx` - Friend request display
- `ConversationItem.jsx` - Conversation list item

#### `src/Pages/`

Page components yang di-route oleh React Router.

**Pages:**

- `Home.jsx` - Landing/home page
- `Login.jsx` - Login form
- `Register.jsx` - Registration form
- `Chat.jsx` - Main chat interface
- `Friends.jsx` - Friends management page
- `Profile.jsx` - User profile page

#### `src/hooks/`

Custom React hooks untuk logic reusability.

**Hooks:**

- `useAuth.js` - Firebase authentication logic
- `useSocket.js` - Socket.io connection & events
- `useChat.js` - Chat operations (send, receive messages)
- `useFetch.js` - Data fetching dengan error handling
- `useForm.js` - Form handling dengan validation

#### `src/utils/`

Utility functions untuk common operations.

**Utils:**

- `api.js` - Axios instance & API configuration
- `formatters.js` - Date/time formatting
- `validators.js` - Input validation functions
- `localStorage.js` - Local storage helpers
- `constants.js` - App constants & config

#### `src/server/`

API integration layer untuk backend communication.

**APIs:**

- `conversation.js` - Conversation endpoints
- `auth.js` - Authentication endpoints
- `friends.js` - Friends endpoints
- `user.js` - User endpoints

## 🚀 Setup & Installation

### Prerequisites

- Node.js v14.0+
- npm atau yarn
- Backend server running (localhost:3000)
- Firebase project

### Installation Steps

```bash
# 1. Navigate ke folder frontend
cd chatify-fe

# 2. Install dependencies
npm install

# 3. Setup environment variables
cp .env.example .env
# Edit .env dengan kredensial Firebase Anda

# 4. Start development server
npm run dev
```

### Verification

Frontend harus berjalan di `http://localhost:5173`:

```
  VITE v6.3.5  ready in 234 ms

  ➜  Local:   http://localhost:5173/
```

## ⚙️ Environment Variables

Buat file `.env` di root folder `chatify-fe/`:

```env
# === API Configuration ===
VITE_API_URL=http://localhost:3000
VITE_SOCKET_URL=http://localhost:3000

# === Firebase Configuration ===
VITE_FIREBASE_API_KEY=your_firebase_api_key
VITE_FIREBASE_AUTH_DOMAIN=your-project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your-project-id
VITE_FIREBASE_STORAGE_BUCKET=your-project.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
VITE_FIREBASE_APP_ID=your_app_id

# === App Configuration ===
VITE_APP_NAME=Chatify
VITE_MAX_MESSAGE_LENGTH=5000
```

### Mendapatkan Firebase Credentials

1. Buka [Firebase Console](https://console.firebase.google.com)
2. Pilih project Anda
3. Settings (gear icon) → Project Settings
4. Copy Web credentials
5. Paste ke `.env`

## 🏃 Running the App

### Development Mode

```bash
npm run dev
```

App akan berjalan di `http://localhost:5173` dengan HMR (Hot Module Replacement).

### Production Build

```bash
npm run build
```

Menghasilkan optimized build di folder `dist/`.

### Preview Build

```bash
npm run preview
```

Preview production build secara lokal sebelum deploy.

### Linting

```bash
npm run lint
```

Check code dengan ESLint rules.

## 📊 Project Structure

### Component Hierarchy

```
<App>
  ├── <AuthProvider>
  ├── <Router>
  │   ├── <Home />
  │   ├── <Login />
  │   ├── <Register />
  │   ├── <Chat>
  │   │   ├── <Sidebar />
  │   │   │   ├── <ConversationList />
  │   │   │   └── <FriendRequest />
  │   │   └── <ChatBox>
  │   │       ├── <MessageList />
  │   │       ├── <MessageInput />
  │   │       └── <UserInfo />
  │   ├── <Friends />
  │   │   ├── <FriendsList />
  │   │   └── <FriendRequests />
  │   └── <Profile />
```

## 🧩 Components

### Presentational Components

Komponen yang menerima props dan render UI.

**Contoh Pattern:**

```jsx
// ChatBox.jsx
export function ChatBox({ conversationId, messages, onSendMessage }) {
  return (
    <div className="chat-box">
      <MessageList messages={messages} />
      <MessageInput onSend={onSendMessage} />
    </div>
  );
}
```

### Container Components

Komponen yang handle logic dan state.

```jsx
// ChatContainer.jsx
export function ChatContainer() {
  const { messages, sendMessage } = useChat();

  return <ChatBox messages={messages} onSendMessage={sendMessage} />;
}
```

## 📄 Pages

### Authentication Pages

- **Login.jsx**
  - Form login dengan email/password
  - Firebase authentication
  - Redirect ke chat jika sudah login

- **Register.jsx**
  - Form registrasi user baru
  - Input validation
  - Automatic login setelah register

### Main Pages

- **Chat.jsx**
  - Layout utama dengan sidebar & chatbox
  - Real-time messaging
  - Conversation management

- **Friends.jsx**
  - Daftar teman
  - Friend requests (sent & received)
  - Add/remove friends

## 🎣 Hooks

### useAuth()

```javascript
const { user, login, logout, register, loading } = useAuth();
```

Manage user authentication state dan operations.

### useSocket()

```javascript
const { socket, connected } = useSocket();

// Emit event
socket?.emit("join-room", conversationId);

// Listen event
socket?.on("receive-message", handleMessage);
```

Manage Socket.io connection dan events.

### useChat()

```javascript
const { messages, sendMessage, loadMessages } = useChat();
```

Manage chat state dan operations.

### Custom Query Hooks (React Query)

```javascript
const {
  useGetConversations,
  useGetMessages,
  useSendMessage,
} = require(".../query");

// Usage
const { data: conversations } = useGetConversations();
const { mutate: sendMessage } = useSendMessage();
```

## 🎛️ State Management

### Zustand Store

```javascript
// store/useAuthStore.js
import { create } from "zustand";

export const useAuthStore = create((set) => ({
  user: null,
  token: null,
  setUser: (user) => set({ user }),
  setToken: (token) => set({ token }),
}));

// Usage di component
const { user, setUser } = useAuthStore();
```

### React Query

```javascript
const { data, isLoading, error } = useQuery({
  queryKey: ["conversations"],
  queryFn: fetchConversations,
});
```

Manage server state dengan automatic caching & synchronization.

## 🔌 API Integration

### API Configuration

```javascript
// utils/api.js
import axios from "axios";

const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
  timeout: 10000,
});

// Add token ke request
api.interceptors.request.use((config) => {
  const token = localStorage.getItem("firebaseToken");
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});
```

### Making API Calls

```javascript
// server/conversation.js
import api from "../utils/api";

export const getConversations = async () => {
  const response = await api.get("/conversations");
  return response.data;
};

export const sendMessage = async (conversationId, message) => {
  const response = await api.post(`/conversations/${conversationId}/messages`, {
    content: message,
  });
  return response.data;
};
```

### Using dengan React Query

```javascript
import { useQuery, useMutation } from "@tanstack/react-query";
import { getConversations, sendMessage } from "../server/conversation";

export function useGetConversations() {
  return useQuery({
    queryKey: ["conversations"],
    queryFn: getConversations,
  });
}

export function useSendMessage() {
  return useMutation({
    mutationFn: ({ conversationId, message }) =>
      sendMessage(conversationId, message),
    onSuccess: () => {
      // Invalidate cache & refetch
      queryClient.invalidateQueries({ queryKey: ["conversations"] });
    },
  });
}
```

## 🔌 Socket.io Integration

### Setup Socket.io Client

```javascript
// hooks/useSocket.js
import { useEffect, useRef } from "react";
import io from "socket.io-client";

export function useSocket() {
  const socketRef = useRef(null);
  const { user } = useAuth();

  useEffect(() => {
    if (!user) return;

    // Initialize socket
    socketRef.current = io(import.meta.env.VITE_SOCKET_URL, {
      auth: {
        token: user.accessToken,
      },
    });

    // Event listeners
    socketRef.current.on("receive-message", handleMessage);
    socketRef.current.on("error", handleError);

    return () => socketRef.current?.disconnect();
  }, [user]);

  return socketRef.current;
}
```

### Joining Room & Sending Message

```javascript
// Join room
socket?.emit("join-room", conversationId);

// Listen for messages
socket?.on("receive-message", (message) => {
  // Update UI dengan message baru
});

// Send message
socket?.emit("send-message", {
  roomId: conversationId,
  message: { content: "Hello!" },
});

// Leave room
socket?.emit("leave-room", conversationId);
```

## 📝 Development Guide

### Adding New Page

1. Create page component di `src/Pages/NewPage.jsx`
2. Add route di `App.jsx`:

```jsx
<Router>
  <Route path="/new-page" element={<NewPage />} />
</Router>
```

3. Add navigation link di `Sidebar.jsx`

### Adding New Component

1. Create component di `src/components/NewComponent.jsx`
2. Define props interface
3. Export component
4. Import & use di pages

### Adding API Call

1. Add function di `src/server/[domain].js`
2. Create React Query hook di `src/hooks/useQuery[Domain].js`
3. Use hook di component:

```jsx
const { data, isLoading } = useGetData();
```

### Form Handling

```jsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  email: z.string().email(),
  message: z.string().min(1),
});

export function MessageForm() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm({
    resolver: zodResolver(schema),
  });

  const onSubmit = (data) => {
    // Handle form submission
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("email")} />
      {errors.email && <span>{errors.email.message}</span>}
      <input {...register("message")} />
      {errors.message && <span>{errors.message.message}</span>}
      <button type="submit">Send</button>
    </form>
  );
}
```

## 🧪 Testing

### Linting

```bash
npm run lint
```

### Unit Testing

Untuk menambah unit tests:

1. Install testing library:

```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom
```

2. Create test file: `src/components/__tests__/Component.test.jsx`

3. Write tests:

```jsx
import { render, screen } from "@testing-library/react";
import { ChatBox } from "../ChatBox";

describe("ChatBox", () => {
  it("renders message list", () => {
    render(<ChatBox messages={[]} />);
    expect(screen.getByTestId("message-list")).toBeInTheDocument();
  });
});
```

## 🔧 Development Tips

### Hot Module Replacement (HMR)

Vite automatically reloads components saat file di-save.

### Browser DevTools

1. Install [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/) extension
2. Inspect components & state di DevTools

### Environment Variables

- Hanya variable dengan prefix `VITE_` yang accessible di client
- Restart dev server setelah ubah `.env`

### Performance

```jsx
// Use React.memo untuk expensive components
export const UserCard = React.memo(function UserCard({ user }) {
  return <div>{user.name}</div>;
});

// Use useCallback untuk stable function references
const handleSendMessage = useCallback(
  (message) => {
    sendMessage(message);
  },
  [sendMessage],
);
```

## 📚 Useful Resources

- [React Documentation](https://react.dev)
- [Vite Documentation](https://vitejs.dev/)
- [React Router](https://reactrouter.com/)
- [React Query Documentation](https://tanstack.com/query/latest)
- [Zustand Documentation](https://github.com/pmndrs/zustand)
- [Socket.io Client](https://socket.io/docs/v4/client-api/)
- [Bootstrap Documentation](https://getbootstrap.com/)
- [Firebase Documentation](https://firebase.google.com/docs)

## 🐛 Troubleshooting

### CORS Error

**Problem:** `Access to XMLHttpRequest blocked by CORS`

**Solution:** Pastikan backend memiliki CORS setting yang benar:

```javascript
app.use(cors());
```

### Socket.io Connection Failed

**Problem:** WebSocket connection error

**Solution:**

- Pastikan backend running dan Socket.io enabled
- Check `VITE_SOCKET_URL` di `.env`
- Verify authentication token

### Firebase Error: "auth/invalid-api-key"

**Problem:** Invalid Firebase credentials

**Solution:**

- Verify credentials di `.env`
- Check Firebase project ID
- Ensure API key is enabled

---

**Last Updated:** March 27, 2026

Untuk bantuan lebih, lihat [Main README](../README.md)
