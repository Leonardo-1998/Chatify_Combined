# Chatify API Reference

Dokumentasi lengkap untuk semua API endpoints dan Socket.io events.

## 📋 Table of Contents

- [Base URL](#base-url)
- [Authentication](#authentication)
- [Response Format](#response-format)
- [Conversation Endpoints](#conversation-endpoints)
- [Friend Endpoints](#friend-endpoints)
- [Socket.io Events](#socketio-events)
- [Error Codes](#error-codes)
- [Rate Limiting](#rate-limiting)

## 🌐 Base URL

```
Production:  https://api.chatify.app
Development: http://localhost:3000
```

## 🔐 Authentication

### Header

Semua endpoint require Firebase ID Token di header:

```
Authorization: Bearer {firebase_id_token}
```

### Socket.io

Pass token via auth object saat connect:

```javascript
const socket = io(SOCKET_URL, {
  auth: {
    token: firebaseIdToken,
  },
});
```

### Getting Token

```javascript
// Frontend dengan Firebase
import { getAuth } from "firebase/auth";

const auth = getAuth();
const token = await auth.currentUser.getIdToken();
```

## 📨 Response Format

### Success Response

```json
{
  "status": 200,
  "message": "Success",
  "data": {
    // Response data
  }
}
```

### Error Response

```json
{
  "status": 400,
  "message": "Error message",
  "errors": [
    {
      "field": "email",
      "message": "Invalid email format"
    }
  ]
}
```

## 💬 Conversation Endpoints

### Create Conversation

```http
POST /api/conversations
Authorization: Bearer {token}
Content-Type: application/json

{
  "name": "Study Group",
  "description": "Discuss algorithms",
  "participantIds": ["user123", "user456"]
}
```

**Response:**

```json
{
  "status": 201,
  "message": "Conversation created",
  "data": {
    "id": "conv123",
    "name": "Study Group",
    "description": "Discuss algorithms",
    "createdBy": "currentUserId",
    "participants": [
      {
        "id": "user123",
        "name": "John Doe",
        "avatar": "url"
      },
      {
        "id": "user456",
        "name": "Jane Smith",
        "avatar": "url"
      }
    ],
    "createdAt": "2024-03-27T10:00:00Z",
    "updatedAt": "2024-03-27T10:00:00Z"
  }
}
```

**Status Codes:**

- `201` Created
- `400` Bad request (validation error)
- `401` Unauthorized (invalid token)
- `403` Forbidden (insufficient permissions)

---

### Get All Conversations

```http
GET /api/conversations?limit=20&offset=0
Authorization: Bearer {token}
```

**Query Parameters:**

- `limit` (optional): Number of results (default: 20, max: 100)
- `offset` (optional): Pagination offset (default: 0)
- `sort` (optional): Sort by (createdAt, updatedAt)

**Response:**

```json
{
  "status": 200,
  "message": "Success",
  "data": {
    "conversations": [
      {
        "id": "conv123",
        "name": "Study Group",
        "description": "Discuss algorithms",
        "lastMessage": {
          "id": "msg123",
          "content": "See you tomorrow!",
          "sender": "user456",
          "createdAt": "2024-03-27T15:30:00Z"
        },
        "participants": 5,
        "unreadCount": 2,
        "createdAt": "2024-03-27T10:00:00Z"
      }
    ],
    "total": 15,
    "hasMore": true
  }
}
```

**Status Codes:**

- `200` Success
- `401` Unauthorized

---

### Get Conversation Details

```http
GET /api/conversations/{conversationId}
Authorization: Bearer {token}
```

**Path Parameters:**

- `conversationId` (required): ID of conversation

**Response:**

```json
{
  "status": 200,
  "message": "Success",
  "data": {
    "id": "conv123",
    "name": "Study Group",
    "description": "Discuss algorithms",
    "createdBy": "user123",
    "participants": [
      {
        "id": "user123",
        "name": "John Doe",
        "avatar": "url",
        "email": "john@example.com",
        "joinedAt": "2024-03-27T10:00:00Z"
      }
    ],
    "messageCount": 42,
    "createdAt": "2024-03-27T10:00:00Z",
    "updatedAt": "2024-03-27T15:30:00Z"
  }
}
```

**Status Codes:**

- `200` Success
- `401` Unauthorized
- `404` Not found

---

### Get Conversation Messages

```http
GET /api/conversations/{conversationId}/messages?limit=50&offset=0
Authorization: Bearer {token}
```

**Query Parameters:**

- `limit` (optional): Number of messages (default: 50, max: 100)
- `offset` (optional): Pagination offset for older messages
- `sort` (optional): asc | desc (default: desc - newest first)

**Response:**

```json
{
  "status": 200,
  "message": "Success",
  "data": {
    "messages": [
      {
        "id": "msg123",
        "conversationId": "conv123",
        "sender": {
          "id": "user123",
          "name": "John Doe",
          "avatar": "url"
        },
        "content": "Hello everyone!",
        "reactions": {
          "👍": ["user456", "user789"],
          "❤️": ["user456"]
        },
        "createdAt": "2024-03-27T15:30:00Z",
        "updatedAt": "2024-03-27T15:30:00Z"
      }
    ],
    "total": 42,
    "hasMore": true
  }
}
```

**Status Codes:**

- `200` Success
- `401` Unauthorized
- `403` Forbidden (no access to conversation)
- `404` Not found

---

### Create Message

```http
POST /api/conversations/{conversationId}/messages
Authorization: Bearer {token}
Content-Type: application/json

{
  "content": "Hello team!",
  "replyTo": "msg122"
}
```

**Request Body:**

- `content` (required): Message text (max: 5000 chars)
- `replyTo` (optional): ID of message to reply to

**Response:**

```json
{
  "status": 201,
  "message": "Message created",
  "data": {
    "id": "msg123",
    "conversationId": "conv123",
    "sender": {
      "id": "user123",
      "name": "John Doe",
      "avatar": "url"
    },
    "content": "Hello team!",
    "replyTo": null,
    "createdAt": "2024-03-27T15:30:00Z",
    "updatedAt": "2024-03-27T15:30:00Z"
  }
}
```

**Status Codes:**

- `201` Created
- `400` Bad request
- `401` Unauthorized
- `403` Forbidden (no access)

---

### Update Message

```http
PUT /api/conversations/{conversationId}/messages/{messageId}
Authorization: Bearer {token}
Content-Type: application/json

{
  "content": "Updated message"
}
```

**Response:**

```json
{
  "status": 200,
  "message": "Message updated",
  "data": {
    "id": "msg123",
    "content": "Updated message",
    "edited": true,
    "editedAt": "2024-03-27T15:35:00Z"
  }
}
```

---

### Delete Message

```http
DELETE /api/conversations/{conversationId}/messages/{messageId}
Authorization: Bearer {token}
```

**Response:**

```json
{
  "status": 200,
  "message": "Message deleted",
  "data": {
    "id": "msg123"
  }
}
```

---

### Leave Conversation

```http
PUT /api/conversations/{conversationId}/leave
Authorization: Bearer {token}
```

**Response:**

```json
{
  "status": 200,
  "message": "Left conversation",
  "data": {
    "conversationId": "conv123"
  }
}
```

---

### Delete Conversation

```http
DELETE /api/conversations/{conversationId}
Authorization: Bearer {token}
```

**Note:** Only creator dapat delete conversation.

**Response:**

```json
{
  "status": 200,
  "message": "Conversation deleted",
  "data": {
    "id": "conv123"
  }
}
```

---

## 👥 Friend Endpoints

### Get All Friends

```http
GET /api/friends?limit=50&offset=0
Authorization: Bearer {token}
```

**Response:**

```json
{
  "status": 200,
  "message": "Success",
  "data": {
    "friends": [
      {
        "id": "friend123",
        "name": "Jane Smith",
        "email": "jane@example.com",
        "avatar": "url",
        "addedAt": "2024-03-20T10:00:00Z"
      }
    ],
    "total": 15
  }
}
```

---

### Send Friend Request

```http
POST /api/friends/requests
Authorization: Bearer {token}
Content-Type: application/json

{
  "friendId": "user456"
}
```

**Response:**

```json
{
  "status": 201,
  "message": "Friend request sent",
  "data": {
    "id": "freq123",
    "to": "user456",
    "status": "pending",
    "createdAt": "2024-03-27T15:30:00Z"
  }
}
```

---

### Get Friend Requests

```http
GET /api/friends/requests
Authorization: Bearer {token}
```

**Response:**

```json
{
  "status": 200,
  "message": "Success",
  "data": {
    "sent": [
      {
        "id": "freq123",
        "to": {
          "id": "user456",
          "name": "Jane Smith",
          "avatar": "url"
        },
        "status": "pending",
        "createdAt": "2024-03-27T15:30:00Z"
      }
    ],
    "received": [
      {
        "id": "freq124",
        "from": {
          "id": "user789",
          "name": "Bob Johnson",
          "avatar": "url"
        },
        "status": "pending",
        "createdAt": "2024-03-27T14:00:00Z"
      }
    ]
  }
}
```

---

### Accept Friend Request

```http
PUT /api/friends/requests/{requestId}/accept
Authorization: Bearer {token}
```

**Response:**

```json
{
  "status": 200,
  "message": "Friend request accepted",
  "data": {
    "friendId": "user789",
    "name": "Bob Johnson",
    "avatar": "url"
  }
}
```

---

### Reject Friend Request

```http
PUT /api/friends/requests/{requestId}/reject
Authorization: Bearer {token}
```

**Response:**

```json
{
  "status": 200,
  "message": "Friend request rejected",
  "data": {
    "requestId": "freq124"
  }
}
```

---

### Remove Friend

```http
DELETE /api/friends/{friendId}
Authorization: Bearer {token}
```

**Response:**

```json
{
  "status": 200,
  "message": "Friend removed",
  "data": {
    "friendId": "user456"
  }
}
```

---

## 🔌 Socket.io Events

### Connection

```javascript
const socket = io("http://localhost:3000", {
  auth: {
    token: firebaseIdToken,
  },
});

socket.on("connect", () => {
  console.log("Connected to server");
});

socket.on("connect_error", (error) => {
  console.error("Connection error:", error);
});
```

---

### Join Room

**Client → Server:**

```javascript
socket.emit("join-room", conversationId);

// Example
socket.emit("join-room", "conv123");
```

**Server Response:**

```javascript
socket.on("error", (errorMessage) => {
  // Handle error if join failed
  console.error("Cannot join room:", errorMessage);
});
```

---

### Send Message

**Client → Server:**

```javascript
socket.emit("send-message", {
  roomId: conversationId,
  message: {
    content: "Hello everyone!",
  },
});

// Example
socket.emit("send-message", {
  roomId: "conv123",
  message: {
    content: "See you tomorrow!",
  },
});
```

---

### Receive Message

**Server → Client:**

```javascript
socket.on("receive-message", (messageData) => {
  console.log("New message:", messageData);

  // messageData structure:
  // {
  //   id: "msg123",
  //   conversationId: "conv123",
  //   sender: { id: "user123", name: "John", avatar: "url" },
  //   content: "Hello!",
  //   createdAt: "2024-03-27T15:30:00Z"
  // }
});
```

---

### Leave Room

**Client → Server:**

```javascript
socket.emit("leave-room", conversationId);

// Example
socket.emit("leave-room", "conv123");
```

---

### User Joined

**Server → Client:**

```javascript
socket.on("user-joined", (userData) => {
  console.log("User joined:", userData);

  // userData structure:
  // {
  //   id: "user456",
  //   name: "Jane Smith",
  //   avatar: "url"
  // }
});
```

---

### User Left

**Server → Client:**

```javascript
socket.on("user-left", (userData) => {
  console.log("User left:", userData);
});
```

---

### Socket Error

**Server → Client:**

```javascript
socket.on("error", (errorMessage) => {
  console.error("Socket error:", errorMessage);
});
```

---

### Disconnect

```javascript
socket.on("disconnect", () => {
  console.log("Disconnected from server");
  // Try to reconnect
  // Handle UI state changes
});
```

---

## ❌ Error Codes

### HTTP Status Codes

| Code | Meaning               | Description                    |
| ---- | --------------------- | ------------------------------ |
| 200  | OK                    | Request successful             |
| 201  | Created               | Resource created               |
| 400  | Bad Request           | Invalid input/validation error |
| 401  | Unauthorized          | Missing/invalid token          |
| 403  | Forbidden             | Access denied                  |
| 404  | Not Found             | Resource not found             |
| 409  | Conflict              | Duplicate request              |
| 422  | Unprocessable Entity  | Validation error               |
| 500  | Internal Server Error | Server error                   |
| 503  | Service Unavailable   | Server maintenance             |

### Error Response Example

```json
{
  "status": 400,
  "message": "Validation error",
  "errors": [
    {
      "field": "content",
      "message": "Message cannot be empty"
    },
    {
      "field": "content",
      "message": "Message too long (max 5000 characters)"
    }
  ]
}
```

### Common Error Messages

| Message                    | Cause                 | Solution                                |
| -------------------------- | --------------------- | --------------------------------------- |
| Invalid token format       | Malformed token       | Ensure token is in Authorization header |
| Token invalid              | Expired/invalid token | Get new token from Firebase             |
| Access denied to this room | No permission         | Verify user is conversation member      |
| User not found             | Invalid userId        | Check user ID                           |
| Conversation not found     | Invalid convId        | Check conversation ID                   |
| Message not found          | Invalid msgId         | Check message ID                        |

---

## ⏱️ Rate Limiting

### Limits

- **Message sending**: 10 messages per 10 seconds per user
- **Friend requests**: 20 requests per hour per user
- **Create conversation**: 5 conversations per hour per user
- **API calls**: 100 requests per minute per user

### Rate Limit Headers

```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1234567890
```

### Rate Limit Error

```json
{
  "status": 429,
  "message": "Too many requests",
  "retryAfter": 60
}
```

---

## 🧪 Testing API

### Using cURL

```bash
# Get conversations
curl -X GET http://localhost:3000/api/conversations \
  -H "Authorization: Bearer YOUR_TOKEN"

# Create message
curl -X POST http://localhost:3000/api/conversations/conv123/messages \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"content":"Hello!"}'
```

### Using Postman

1. Import [Postman Collection](./postman-collection.json)
2. Set token in Environment variables
3. Test each endpoint

---

**Last Updated:** March 27, 2026

Untuk bantuan lebih, lihat [Main README](./README.md)
