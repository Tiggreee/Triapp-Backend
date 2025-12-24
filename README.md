# TriApp Backend

## 🚀 **[VER PROYECTO EN VIVO →](https://tri-app-frontend.vercel.app/music)**

**Full-Stack Project**
Frontend: [github.com/Tiggreee/TriApp-Frontend](https://github.com/Tiggreee/TriApp-Frontend)
Backend: [github.com/Tiggreee/Triapp-Backend](https://github.com/Tiggreee/Triapp-Backend)

---

Backend API for Renata's Finder app. Provides authentication, user management, and favorites persistence.

BACKEND ON RENDER: https://triapp-backend.onrender.com


## Features

- JWT authentication (signup/signin)
- User profile management
- Favorites CRUD (music, colors, avatars)
- Request/error logging
- Rate limiting & security headers
- MongoDB + Mongoose

## Tech Stack

- **Node.js** + **Express**
- **MongoDB** (Mongoose ODM)
- **JWT** for auth
- **bcryptjs** for password hashing
- **winston** + **express-winston** for logging
- **celebrate** (Joi) for validation
- **helmet**, **cors**, **express-rate-limit** for security

## Project Structure

```
├── app.js                 # Main application entry
├── models/
│   ├── User.js            # User schema
│   └── Favorite.js        # Favorite schema
├── routes/
│   ├── index.js           # Root router
│   ├── users.js           # /users/me
│   └── favorites.js       # /favorites CRUD
├── controllers/
│   ├── users.js           # createUser, login, getCurrentUser
│   └── favorites.js       # getFavorites, createFavorite, deleteFavorite
├── middleware/
│   ├── auth.js            # JWT verification
│   ├── logger.js          # Winston request/error loggers
│   └── errorHandler.js    # Centralized error handling
├── utils/
│   ├── NotFoundError.js   # 404
│   ├── UnauthorizedError.js # 401
│   ├── ForbiddenError.js  # 403
│   ├── ConflictError.js   # 409
│   └── BadRequestError.js # 400
├── .env.example           # Environment variables template
├── .gitignore
├── .editorconfig
├── .eslintrc.json
└── package.json
```

## Getting Started

### Prerequisites

- Node.js 16+
- MongoDB (local or Atlas)

### Installation

```bash
# Clone
git clone https://github.com/YOUR_USERNAME/TriApp-Frontend_backend.git
cd TriApp-Frontend_backend

# Install dependencies
npm install

# Create .env from example
cp .env.example .env

# Edit .env with your values
# NODE_ENV=development
# PORT=3000
# MONGODB_URI=mongodb://localhost:27017/triapp
# JWT_SECRET=your-super-secret-key
```

### Run Locally

```bash
# Development (with hot reload)
npm run dev

# Production
npm start

# Lint
npm run lint
```

## API Endpoints

### Public Routes

| Method | Endpoint   | Description                     | Body                                  |
|--------|------------|---------------------------------|---------------------------------------|
| POST   | /signup    | Create new user                 | `{ email, password, name }`           |
| POST   | /signin    | Login user, returns JWT         | `{ email, password }`                 |

### Protected Routes (require `Authorization: Bearer <token>`)

| Method | Endpoint          | Description                  | Body/Params                           |
|--------|-------------------|------------------------------|---------------------------------------|
| GET    | /users/me         | Get current user info        | -                                     |
| GET    | /favorites        | Get all user favorites       | -                                     |
| POST   | /favorites        | Create favorite              | `{ type, data }`                      |
| DELETE | /favorites/:id    | Delete favorite by ID        | `:id` (MongoDB ObjectId)              |

### Example Requests

**Signup**
```bash
curl -X POST http://localhost:3000/signup \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"securepass","name":"Test User"}'
```

**Signin**
```bash
curl -X POST http://localhost:3000/signin \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"securepass"}'
```

**Get Current User**
```bash
curl http://localhost:3000/users/me \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

**Create Favorite**
```bash
curl -X POST http://localhost:3000/favorites \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"type":"music","data":{"trackName":"Song","artist":"Artist"}}'
```

## Environment Variables

Create a `.env` file (not committed to git):

```
NODE_ENV=production
PORT=3000
MONGODB_URI=mongodb://localhost:27017/triapp
JWT_SECRET=your-super-secret-jwt-key-change-in-production
```

In production (server):
- Set `NODE_ENV=production`
- Use strong `JWT_SECRET`
- Use MongoDB Atlas or secure MongoDB instance

## Deployment

**Status**: ✅ Deployed on Render.com (free tier)

### Current Production Environment

- **API Base URL**: https://triapp-backend.onrender.com
- **Database**: MongoDB Atlas (cluster "tigerDev")
- **Hosting Platform**: Render.com
- **Port**: 443 (HTTPS)

### Live Endpoints

```
POST   https://triapp-backend.onrender.com/signup
POST   https://triapp-backend.onrender.com/signin
GET    https://triapp-backend.onrender.com/users/me
GET    https://triapp-backend.onrender.com/favorites
POST   https://triapp-backend.onrender.com/favorites
DELETE https://triapp-backend.onrender.com/favorites/:id
```

### Notes

- Render free tier: App spins down after 15 min of inactivity
- MongoDB Atlas free tier: 512 MB storage
- All sensitive data stored in server environment variables
- Git integration: Auto-deploy from GitHub commits

### Deploy Locally

To test locally before pushing:

```bash
# Install dependencies
npm install

# Create .env file
cp .env.example .env
# Edit with your MongoDB URI and JWT secret

# Start dev server (hot reload)
npm run dev

# Runs on http://localhost:3000
```

## Logging

- **request.log**: All incoming requests (JSON format)
- **error.log**: All errors (JSON format)
- Logs are excluded from git (see `.gitignore`)

## Security

- Passwords hashed with bcryptjs (10 rounds)
- JWT tokens expire in 7 days
- Rate limiting: max 100 requests per 15 min
- Helmet.js for secure headers
- CORS enabled for frontend origin
- Input validation with celebrate/Joi

## Code Standards

- ESLint (Airbnb base config)
- No code comments (self-documenting code)
- Consistent 2-space indentation (.editorconfig)
- Semantic commit messages
- Clean, idiomatic JavaScript

## Development Workflow

1. Create feature branch from `stage-back-end`
2. Implement feature with no comments in code
3. Test locally with MongoDB
4. Commit with semantic message (feat/fix/chore/docs)
5. Open PR to review
6. After approval, merge to `main` and Render auto-deploys

## License

ISC

## Built By

Victor - TripleTen Web Development TripleTen Final Project

---

## 🚀 **[VER PROYECTO EN VIVO →](https://tri-app-frontend.vercel.app/music)**

**Full-Stack Repositories**
Frontend: [github.com/Tiggreee/TriApp-Frontend](https://github.com/Tiggreee/TriApp-Frontend)
Backend: [github.com/Tiggreee/Triapp-Backend](https://github.com/Tiggreee/Triapp-Backend)

---

**Update hotfix:**
Recuerda que para que el frontend se comunique con el backend en producción, tienes que agregar la variable VITE_API_BASE_URL en Vercel. Ve a Settings → Environment Variables y pon:

VITE_API_BASE_URL=https://triapp-backend.onrender.com

No la marques como Sensitive, y redeploya el proyecto. Si no la pones, el login y favoritos no van a funcionar.

