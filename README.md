# TriApp Backend

Backend API for Renata's Finder app. Provides authentication, user management, and favorites persistence.

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

## Deployment (Google Cloud VM)

1. **Create VM** on Google Cloud (Ubuntu recommended)
2. **Install Node.js** and **MongoDB** (or use MongoDB Atlas)
3. **Clone repo** on server
4. **Install deps**: `npm install --production`
5. **Create `.env`** with production values
6. **Start app**: `npm start` (or use PM2 for process management)
7. **Setup domain**: Point DNS A record to VM public IP
8. **Install SSL certificate** (Let's Encrypt): `sudo certbot --nginx`
9. **Configure Nginx** as reverse proxy to Node.js app

Example Nginx config:
```nginx
server {
  listen 80;
  server_name yourdomain.com;
  return 301 https://$host$request_uri;
}

server {
  listen 443 ssl;
  server_name yourdomain.com;

  ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;

  location / {
    proxy_pass http://localhost:3000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
  }
}
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

## Development Workflow

1. Create feature branch from `stage-back-end`
2. Implement feature (no comments in code)
3. Test locally with MongoDB
4. Commit with semantic message (feat/fix/chore/docs)
5. Open PR to `stage-back-end`
6. After review, merge to `main`

## Deployment URL

**API Base URL**: https://triapp-backend.onrender.com

**Live Endpoints:**
- POST https://triapp-backend.onrender.com/signup
- POST https://triapp-backend.onrender.com/signin
- GET https://triapp-backend.onrender.com/users/me
- GET https://triapp-backend.onrender.com/favorites
- POST https://triapp-backend.onrender.com/favorites
- DELETE https://triapp-backend.onrender.com/favorites/:id

**Hosting:** Render (Free tier - spins down after 15 min inactivity)

## License

ISC

---

Made with 💖 for Renata by Victor

