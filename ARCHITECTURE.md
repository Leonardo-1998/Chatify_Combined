# Chatify - System Architecture

Dokumentasi lengkap tentang arsitektur sistem Chatify dan cara komponennya berinteraksi.

## 📋 Table of Contents

- [System Overview](#system-overview)
- [Architecture Diagram](#architecture-diagram)
- [Backend Architecture](#backend-architecture)
- [Frontend Architecture](#frontend-architecture)
- [Data Flow](#data-flow)
- [Authentication Flow](#authentication-flow)
- [Real-time Communication](#real-time-communication)
- [Database Design](#database-design)

## 🏗️ System Overview

Chatify adalah aplikasi chat real-time yang terdiri dari:

```
┌─────────────────────────────────────────────────────────────┐
│                        Chatify System                        │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌────────────────┐          ┌──────────────────────────┐   │
│  │   Frontend     │          │     Backend              │   │
│  │  (React/Vite) │◄────────►│  (Express/Node.js)       │   │
│  └────────────────┘          └──────────────────────────┘   │
│         ▲                              ▲                     │
│         │                              │                     │
│    WebSocket                      PostgreSQL               │
│         │                              │                     │
│         └──────────────┬───────────────┘                    │
│                        │                                     │
│                  ┌─────▼─────┐                              │
│                  │ Socket.io  │  External Services:         │
│                  └────┬───────┘  • Firebase Auth            │
│                       │           • Google Generative AI    │
│                       │           • PostgreSQL DB           │
│                       │                                     │
└─────────────────────────────────────────────────────────────┘
```

## 🎭 Architecture Diagram

### High-Level System Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                      External Services                       │
├──────────────┬──────────────────┬──────────────────────────────┤
│  Firebase    │  Google AI       │  PostgreSQL Database        │
│  • Auth      │  • Generative AI │  • Users                    │
│  • JWT       │                  │  • Conversations            │
└──────┬──────┴──────────┬────────┴──────────────┬────────────────┘
       │                 │                       │
       └────────┬────────┴───────────┬───────────┘
                │                   │
        ┌───────▼──────┐      ┌─────▼─────────┐
        │   Frontend   │      │   Backend     │
        │   (React)    │◄────►│  (Express)    │
        └───────┬──────┘      └──────┬────────┘
                │                    │
                └────────┬───────────┘
                       HTTP + WebSocket
                       Socket.io
```

## 🔙 Backend Architecture

### Backend Structure

```
┌────────────────────────────────────────┐
│           Express Server               │
├────────────────────────────────────────┤
│                                        │
│  ┌──────────────────────────────────┐ │
│  │    Request/Response Layer        │ │
│  │  • CORS Middleware               │ │
│  │  • Body Parser                   │ │
│  │  • Error Handler                 │ │
│  └──────────────────────────────────┘ │
│                                        │
│  ┌──────────────────────────────────┐ │
│  │   Authentication Layer           │ │
│  │  • Firebase Token Verification   │ │
│  │  • JWT Middleware                │ │
│  │  • User Context Injection        │ │
│  └──────────────────────────────────┘ │
│                                        │
│  ┌──────────────────────────────────┐ │
│  │     Socket.io Layer              │ │
│  │  • Connection Management         │ │
│  │  • Event Handling                │ │
│  │  • Room Management               │ │
│  └──────────────────────────────────┘ │
│                                        │
│  ┌──────────────────────────────────┐ │
│  │   Router Layer                   │ │
│  │  • FriendRouter                  │ │
│  │  • ConversationRouter            │ │
│  └──────────────────────────────────┘ │
│                                        │
│  ┌──────────────────────────────────┐ │
│  │   Controller Layer               │ │
│  │  • Business Logic                │ │
│  │  • Request Handling              │ │
│  │  • Validation Coordination       │ │
│  └──────────────────────────────────┘ │
│                                        │
│  ┌──────────────────────────────────┐ │
│  │   Service/Helper Layer           │ │
│  │  • Business Rules                │ │
│  │  • External API calls            │ │
│  │  • Utilities                     │ │
│  └──────────────────────────────────┘ │
│                                        │
│  ┌──────────────────────────────────┐ │
│  │   Data Access Layer (Sequelize)  │ │
│  │  • Models                        │ │
│  │  • Database Operations           │ │
│  │  • Associations                  │ │
│  └──────────────────────────────────┘ │
│                                        │
└────────────────────────────────────────┘
            ▼
    ┌───────────────┐
    │ PostgreSQL DB │
    └───────────────┘
```

### Request Flow

```
Client Request
    ▼
CORS/Body Parser Middleware
    ▼
AuthMiddleware (Verify Firebase Token)
    ▼
Route Matching (/api/conversations, /api/friends)
    ▼
Controller (Business Logic)
    ▼
Service/Validation Layer
    ▼
Sequelize Model (Database Query)
    ▼
PostgreSQL Database
    ▼
Response Construction
    ▼
ErrorMiddleware (Error Handling)
    ▼
HTTP Response to Client
```

## 🖥️ Frontend Architecture

### Frontend Component Structure

```
┌──────────────────────────────────────┐
│         App.jsx (Root)               │
├──────────────────────────────────────┤
│  • Router Setup                      │
│  • Provider Setup                    │
│  • Global State                      │
└──────────────────────────────────────┘
            ▼
┌──────────────────────────────────────┐
│      Route Components                │
├──────────────────────────────────────┤
│  • Home / Login / Register (Auth)    │
│  • Chat (Main App)                   │
│  • Friends (Management)              │
│  • Profile (User)                    │
└──────────────────────────────────────┘
            ▼
┌──────────────────────────────────────┐
│     Container/Smart Components       │
├──────────────────────────────────────┤
│  • Connect to Redux/Zustand          │
│  • Fetch Data (React Query)          │
│  • Handle Socket Events              │
└──────────────────────────────────────┘
            ▼
┌──────────────────────────────────────┐
│    Presentational Components         │
├──────────────────────────────────────┤
│  • Render UI                         │
│  • Props-based                       │
│  • Reusable                          │
└──────────────────────────────────────┘
```

### State Management Strategy

```
┌──────────────────────────────────────┐
│   Zustand Store (Client State)       │
├──────────────────────────────────────┤
│  • useAuthStore (User, Token)        │
│  • useUIStore (UI State)             │
│  • useChatStore (Chat State)         │
└──────────────────────────────────────┘
           ▲        ▲
           │        │
    ┌──────┘   ┌────┴──────┐
    │          │           │
    │      ┌───▼────┐  ┌────▼──────┐
    │      │React   │  │ Socket.io  │
    │      │Query   │  │ Events     │
    │      │(Server │  │            │
    │      │state)  │  │            │
    │      └────────┘  └────────────┘
    │
    └──► Components (via Hooks)
```

## 📊 Data Flow

### User Sends Message Flow

```
1. User Types in MessageInput
   ▼
2. Form Handler validates input
   ▼
3. useSendMessage() hook triggered
   ▼
4. API Call: POST /api/conversations/:id/messages
   ▼
5. Backend Controller receives request
   ▼
6. Controller validates message
   ▼
7. Controller saves to database (Sequelize)
   ▼
8. Socket.io event emitted: "receive-message"
   ▼
9. All users in room receive message via Socket
   ▼
10. React component updates (Zustand + local state)
    ▼
11. UI re-renders with new message
```

### Receive Message from Socket

```
User A sends message
    ▼
Backend Socket.io event "send-message"
    ▼
Backend saves to DB
    ▼
Backend emits "receive-message" to room
    ▼
User B's Socket.io client receives event
    ▼
useSocket() hook triggers callback
    ▼
Zustand store updated
    ▼
Component re-renders
    ▼
Message visible in UI
```

## 🔐 Authentication Flow

### Initial Login

```
┌─────────────────────────────────────────────┐
│               Frontend (React)              │
├─────────────────────────────────────────────┤
│                                             │
│  1. User submits Login Form                │
│     ▼                                       │
│  2. useAuth() hook calls Firebase.auth()  │
│     ▼                                       │
│  3. Firebase returns IDToken               │
│     ▼                                       │
│  4. Store token in localStorage            │
│     ▼                                       │
│  5. Save to Zustand store                  │
│     ▼                                       │
│  6. Redirect to /chat                      │
│                                             │
└─────────────────────────────────────────────┘
            │
            │ IDToken
            ▼
┌─────────────────────────────────────────────┐
│           Backend (Express)                 │
├─────────────────────────────────────────────┤
│                                             │
│  1. Receive IDToken in Authorization header│
│     ▼                                       │
│  2. AuthMiddleware extracts token          │
│     ▼                                       │
│  3. Firebase.auth().verifyIdToken(token)  │
│     ▼                                       │
│  4. If valid: Extract user info (uid, etc) │
│     ▼                                       │
│  5. Attach to request.user                 │
│     ▼                                       │
│  6. Pass to controller                     │
│                                             │
└─────────────────────────────────────────────┘
```

### Socket.io Subscribe

```
1. Frontend connects to Socket.io
   socket = io(SOCKET_URL, {
     auth: { token: firebaseToken }
   })
   ▼
2. Backend Socket.io middleware verifies token
   io.use((socket, next) => {
     verify token → extract user
   })
   ▼
3. User authorized → socket.user = payload
   ▼
4. Socket connected & ready for events
```

## 🔌 Real-time Communication

### Socket.io Event Architecture

```
┌─────────────────────┐         ┌──────────────────────┐
│   Client Socket     │         │  Server Socket.io    │
├─────────────────────┤         ├──────────────────────┤
│                     │         │                      │
│ emit('join-room')   │────────►│ on('join-room')      │
│                     │         │ → socket.join()      │
│                     │         │                      │
│ emit('send-message')│────────►│ on('send-message')   │
│                     │         │ → Save to DB         │
│                     │         │ → broadcast()        │
│                     │         │                      │
│◄────────────────────┤◄────────│ io.to().emit()       │
│ on('receive-message')         │                      │
│ → Update Zustand    │         │                      │
│ → Re-render         │         │                      │
│                     │         │                      │
│ emit('leave-room')  │────────►│ on('leave-room')     │
│                     │         │ → socket.leave()     │
│                     │         │                      │
└─────────────────────┘         └──────────────────────┘
```

### Room Management

```
Conversation = Room

User joins conversation
    ▼
socket.join(conversationId)
    ▼
User added to Socket.io room
    ▼
socket.to(roomId).emit() - broadcasts to room
    ▼
Only users in room receive message

User leaves conversation
    ▼
socket.leave(conversationId)
    ▼
User removed from Socket.io room
    ▼
No longer receives messages for that room
```

## 🗄️ Database Design

### Entity Relationship Diagram

```
┌──────────────────┐
│     Users        │
├──────────────────┤
│ id (PK)          │
│ email (UNIQUE)   │◄─┐
│ name             │  │ 1:N
│ avatar           │  │
│ firebaseUid      │  │
│ createdAt        │  │
│ updatedAt        │  │
└──────────────────┘  │
        ▲             │
        │             │
        │ 1:N         │
   ┌────┴─────────────┘
   │
   │
   │  ┌────────────────────────┐
   │  │   Conversations        │
   │  ├────────────────────────┤
   │  │ id (PK)                │
   │  │ name                   │
   │  │ description            │
   │  │ createdBy (FK) ◄───────┤ 1:N
   │  │ createdAt              │
   │  │ updatedAt              │
   │  └────────┬──────┬────────┘
   │           │      │
   │    N:N    │      │ 1:N
   │ ┌─────────┘      │
   │ │         ┌──────┴─────────┐
   │ │         │                │
   │ │    ┌────▼──────────────┐ │
   │ │    │   Messages        │ │
   │ │    ├───────────────────┤ │
   │ │    │ id (PK)           │ │
   │ └───►│ conversationId (FK)│ │
   │      │ userId (FK)       │ │
   │ ┌───►│ content           │ │
   │ │    │ createdAt         │ │
   │ │    │ updatedAt         │ │
   │ │    └───────────────────┘ │
   │ │                         │
   │ │  ┌─────────────────────┐│
   │ │  │ ConversationUsers   ││
   │ │  ├─────────────────────┤│
   │ └──┤ conversationId (FK) ││
   │    │ userId (FK) ────────┼┘
   │    │ joinedAt            │
   │    └─────────────────────┘
   │
   │
   │  ┌──────────────────────┐
   │  │   Friends            │
   │  ├──────────────────────┤
   │  │ id (PK)              │
   └──┤ userId (FK) ─┐   1:N │
      │ friendId(FK)─┼──────►│
      │ status       │       │
      │ createdAt    │       │
      │ updatedAt    │       │
      └──────────────┘       │
           N:N ◄─────────────┘
```

### Query Patterns

```javascript
// Get user with conversations
const user = await User.findByPk(userId, {
  include: [
    {
      model: Conversation,
      through: { attributes: [] }, // Junction table
    },
  ],
});

// Get conversation with messages and users
const conv = await Conversation.findByPk(id, {
  include: [
    {
      model: Message,
      include: [{ model: User, attributes: ["id", "name"] }],
    },
    { model: User, through: { attributes: [] } },
  ],
});
```

## 🔄 Component Interaction

### Chat Flow Example

```
┌──────────────────┐
│   App.jsx        │
└────────┬─────────┘
         │
    ┌────▼─────────────────────┐
    │  Chat Page Component      │
    │  • useAuth()              │
    │  • useSocket()            │
    │  • useGetConversations()  │
    └────┬──────────┬──────────┬─────────┐
         │          │          │         │
    ┌────▼──┐  ┌────▼───┐  ┌──▼──┐ ┌───▼────────┐
    │Sidebar│  │ChatBox │  │Info │ │MessageList │
    │       │  │        │  │     │ │            │
    │∘Convs │  │∘Msgs   │  │∘User│ │∘Messages   │
    │∘Friends│ │∘Input  │  │Info │ │            │
    └───────┘  └────┬───┘  └─────┘ └────────────┘
                    │
           ┌────────┴───────┐
           │                │
      Send Message       Join Room
           │                │
      ┌────▼───┐       ┌────▼─────┐
      │Backend API│     │Socket.io  │
      └────────┘       └───────────┘
           │                │
      ┌────▼────────────────▼──┐
      │  PostgreSQL / Firebase  │
      └─────────────────────────┘
```

## 🚀 Performance Considerations

### Frontend Optimization

```javascript
// 1. Code Splitting
const Chat = React.lazy(() => import("./Chat"));

// 2. Memoization
const MessageItem = React.memo(({ msg }) => {
  return <div>{msg.content}</div>;
});

// 3. Query Caching (React Query)
const { data } = useQuery({
  queryKey: ["conversations"],
  queryFn: fetchConversations,
  staleTime: 5 * 60 * 1000, // 5 min
});

// 4. Virtual Scrolling untuk long lists
<FixedSizeList height={600} itemCount={messages.length} itemSize={50} />;
```

### Backend Optimization

```javascript
// 1. Database Indexing
// In migrations: CREATE INDEX idx_user_email ON users(email);

// 2. Query Optimization (eager loading)
await Conversation.findByPk(id, {
  include: [Message, ConversationUser],
});

// 3. Connection Pooling (default di Sequelize)

// 4. Caching layer (Redis - optional)
const cached = await redis.get(`conv:${id}`);
```

---

**Last Updated:** March 27, 2026

Untuk informasi lebih detail, lihat dokumentasi masing-masing bagian:

- [Backend Architecture](./chatify-be/README.md)
- [Frontend Architecture](./chatify-fe/README.md)
