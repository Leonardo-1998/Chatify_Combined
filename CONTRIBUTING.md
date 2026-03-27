# Contributing to Chatify

Terima kasih telah tertarik untuk berkontribusi pada Chatify! Panduan ini akan membantu Anda memulai.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Process](#development-process)
- [Coding Standards](#coding-standards)
- [Commit Guidelines](#commit-guidelines)
- [Pull Request Process](#pull-request-process)
- [Testing](#testing)
- [Documentation](#documentation)

## 🤝 Code of Conduct

Kami berkomitmen untuk menyediakan lingkungan yang inklusif dan menghormati untuk semua kontributor. Mohon:

- Gunakan bahasa yang sopan dan profesional
- Hormati pendapat orang lain
- Laporkan perilaku yang tidak pantas ke tim development

## 🚀 Getting Started

### 1. Fork Repository

```bash
# Visit github.com/hacktive8/chatify
# Click "Fork" button
```

### 2. Clone Your Fork

```bash
git clone https://github.com/your-username/chatify.git
cd chatify
```

### 3. Add Upstream Remote

```bash
git remote add upstream https://github.com/hacktive8/chatify.git
```

### 4. Create Feature Branch

```bash
git checkout -b feature/your-feature-name
# atau
git checkout -b bugfix/your-bug-name
```

### 5. Setup Development Environment

Ikuti [SETUP.md](./SETUP.md) untuk setup lengkap.

## 📝 Development Process

### Before You Start

1. Check [Issues](https://github.com/hacktive8/chatify/issues) untuk memastikan tidak ada duplicate
2. Diskusi fitur besar dengan tim sebelum mulai development
3. Assign issue ke diri Anda untuk menghindari duplikasi effort

### During Development

```bash
# Keep branch updated
git fetch upstream
git rebase upstream/main

# Make commits regularly
git add .
git commit -m "meaningful message"

# Push ke fork Anda
git push origin feature/your-feature-name
```

### File Organization

```
# Untuk Backend: Ikuti struktur existing
controllers/    → Business logic
models/         → Database models
routers/        → API routes
middlewares/    → Custom middleware

# Untuk Frontend: Ikuti struktur existing
components/     → Reusable components
Pages/          → Page components
hooks/          → Custom hooks
utils/          → Utility functions
server/         → API calls
```

## 💻 Coding Standards

### JavaScript/Node.js

**Style:**

- Use `const` by default, `let` when needed
- Use arrow functions `() => {}`
- Use template literals `` `string ${var}` ``
- Add semicolons at end of statements

**Example:**

```javascript
// Good
const getUserById = (id) => {
  const user = database.get(id);
  return user;
};

// Bad
function getUserById(id) {
  var user = database.get(id);
  return user;
}
```

### React

**Component Pattern:**

```jsx
// Use functional components with hooks
export function MyComponent({ prop1, prop2 }) {
  const [state, setState] = useState(null);

  const handleClick = useCallback(() => {
    setState(true);
  }, []);

  return <div onClick={handleClick}>{state ? prop1 : prop2}</div>;
}
```

**Naming Conventions:**

- Components: PascalCase (MyComponent.jsx)
- Files: PascalCase for components, camelCase for utilities
- Functions: camelCase (getData)
- Constants: UPPER_SNAKE_CASE (API_URL)

### General Rules

```javascript
// 1. Keep functions small and focused
// Good - one responsibility
const getUserAge = (user) => {
  return new Date().getFullYear() - user.birthYear;
};

// Bad - multiple responsibilities
const processUserData = (user) => {
  // calculate age
  // verify email
  // check permissions
  // ...
};

// 2. Use meaningful names
// Good
const isUserAuthenticated = true;
const calculateTotalPrice = (items) => {};

// Bad
const flag = true;
const calc = (arr) => {};

// 3. Add comments untuk logika kompleks
// Good
// Menggunakan exponential backoff untuk retry logic
const exponentialBackoff = (attempt) => Math.pow(2, attempt) * 1000;

// Bad
// tidak perlu comment
const name = user.name;
```

### Linting

```bash
# Backend - Code quality
npm run lint
npm run lint -- --fix  # Auto-fix issues

# Frontend
npm run lint
npm run lint -- --fix
```

## 📌 Commit Guidelines

Gunakan [Conventional Commits](https://www.conventionalcommits.org/) format:

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type

- **feat**: Feature baru
- **fix**: Bug fix
- **docs**: Documentation changes
- **style**: Format changes (tidak mengubah logic)
- **refactor**: Code refactoring
- **perf**: Performance improvements
- **test**: Tests
- **chore**: Build, dependencies, tools

### Scope

- **backend**: Backend-related changes
- **frontend**: Frontend-related changes
- **auth**: Authentication-related
- **socket**: Socket.io-related
- **database**: Database-related

### Examples

**Good Commits:**

```bash
# Feature
git commit -m "feat(auth): add email verification"

# Bug fix
git commit -m "fix(chat): handle message encoding for special characters"

# Documentation
git commit -m "docs(readme): add setup instructions"

# Refactoring
git commit -m "refactor(api): extract error handling to middleware"
```

**Bad Commits:**

```bash
# Terlalu generic
git commit -m "update"
git commit -m "fix"

# Terlalu detail
git commit -m "change line 45 in file.js"

# Multiple changes dalam satu commit
git commit -m "add feature and fix bug and update docs"
```

## 🔄 Pull Request Process

### 1. Create Pull Request

```bash
git push origin feature/your-feature-name
```

Kemudian di GitHub, click "New Pull Request"

### 2. PR Description

Gunakan template ini:

```markdown
## Description

Brief description dari changes

## Type of change

- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing

Describe how you tested this:

- [ ] Tested on development environment
- [ ] Added unit tests
- [ ] Manual testing done

## Checklist

- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex logic
- [ ] Documentation updated
- [ ] No new warnings generated
- [ ] Tests pass locally

## Screenshots (if applicable)

Add screenshots untuk UI changes
```

### 3. Review Process

- Minimum 1 approval required
- All CI checks harus pass
- Address feedback dari reviewers
- Rebase dengan upstream jika ada conflicts

### 4. Merge

Merge dengan "Squash and merge" untuk clean history:

```bash
git checkout main
git pull upstream main
```

## 🧪 Testing

### Backend Testing

```javascript
// Example test struktur
describe("ConversationController", () => {
  describe("getConversations", () => {
    it("should return list of user conversations", async () => {
      // Arrange - setup
      const userId = "test-user-123";
      const mockConversations = [
        /* ... */
      ];

      // Act - execute
      const result = await getConversations(userId);

      // Assert - verify
      expect(result).toEqual(mockConversations);
    });

    it("should handle errors gracefully", async () => {
      // Arrange
      // Act & Assert
      expect(() => getConversations(null)).toThrow();
    });
  });
});
```

### Run Tests

```bash
cd chatify-be
npm test

# Watch mode
npm test -- --watch

# Coverage
npm test -- --coverage
```

### Frontend Testing

```bash
cd chatify-fe

# Run linting
npm run lint

# Run tests (jika ada)
npm test
```

## 📚 Documentation

### Code Comments

```javascript
/**
 * Calculate age dari birth year
 * @param {number} birthYear - Year of birth
 * @returns {number} Age in years
 */
const calculateAge = (birthYear) => {
  return new Date().getFullYear() - birthYear;
};
```

### README Updates

Update relevant README jika Anda:

- Menambah fitur baru
- Mengubah API endpoint
- Menambah dependencies
- Mengubah setup process

### API Documentation

Jika menambah endpoint, update [Backend README](./chatify-be/README.md#api-endpoints):

```markdown
POST /api/conversations

- Create new conversation
- Auth: Required
- Body: { name, description, participantIds: [] }
- Response: { id, name, description, ... }
- Errors: 400 (invalid), 401 (unauthorized)
```

## 🎯 Areas for Contribution

### Backend

- [ ] Add more validation
- [ ] Improve error handling
- [ ] Add logging
- [ ] Performance optimization
- [ ] Tests coverage

### Frontend

- [ ] UI/UX improvements
- [ ] Responsive design fixes
- [ ] Performance optimization
- [ ] Accessibility improvements
- [ ] Tests coverage

### Documentation

- [ ] API documentation
- [ ] Setup guides
- [ ] Troubleshooting guides
- [ ] Code examples

### Others

- [ ] Bug reports
- [ ] Feature requests
- [ ] Security reports (please email privately)

## 💬 Questions?

- **Issues/Features**: Open GitHub issue
- **Discussions**: Use GitHub discussions
- **Security**: Email security@chatify.local (jangan open issue)
- **Questions**: Join our Discord/Slack

## 📜 License

Dengan berkontribusi, Anda setuju bahwa kontribusi Anda akan dilisensikan di bawah ISC License.

---

**Thank you untuk kontribusi Anda! 🎉**

Kami sangat menghargai setiap kontribusi, baik kecil atau besar.

**Last Updated:** March 27, 2026
