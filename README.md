# Restaurant Order Management System

A complete, production-ready restaurant order management system with real-time notifications built with modern technologies.

## 🚀 Features

### Core Functionality
- **Multi-role System**: Client, Chef, Server, Admin with role-based access control
- **Real-time Order Flow**: Client → Chef → Server → Completed with live notifications
- **Menu Management**: Categories, items with images, prices, availability
- **Table Management**: QR codes, status tracking (Free/Occupied/Reserved)
- **Order Tracking**: Live status updates with visual stepper
- **Admin Dashboard**: Sales reports, popular items, revenue analytics
- **Notifications**: Toast + in-app bell with sound alerts

### Technical Features
- **Real-time**: Socket.io WebSockets with room-based broadcasting
- **Authentication**: JWT access + refresh tokens with HttpOnly cookies
- **Security**: bcrypt (12 rounds), Helmet, CORS, Rate limiting, Zod validation
- **Database**: PostgreSQL with Prisma ORM
- **State Management**: Zustand with persistence
- **Internationalization**: English + Kinyarwanda (i18next)
- **Responsive**: Mobile-first, tablet-optimized for kitchen/server use
- **Dark Mode**: Full support with system preference detection
- **Testing**: Jest + Supertest setup
- **Deployment**: Docker + docker-compose

## 🛠 Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React 18 + Vite + TypeScript + TailwindCSS |
| Backend | Node.js + Express + TypeScript |
| Database | PostgreSQL + Prisma ORM |
| Real-time | Socket.io |
| Auth | JWT + bcrypt |
| State | Zustand |
| Forms | React Hook Form + Zod |
| Notifications | react-hot-toast |
| Charts | Recharts |
| i18n | i18next + react-i18next |
| Deployment | Docker + docker-compose |

## 📁 Project Structure

```
restaurant-system/
├── backend/
│   ├── src/
│   │   ├── config/         # Database, env, socket config
│   │   ├── controllers/    # Route handlers
│   │   ├── routes/         # API routes
│   │   ├── middleware/     # Auth, validation, error handling
│   │   ├── services/       # Business logic
│   │   ├── sockets/        # Socket.io handlers
│   │   ├── utils/          # Helpers (JWT, password, errors)
│   │   ├── validators/     # Zod schemas
│   │   └── app.ts          # Express app entry
│   ├── prisma/
│   │   ├── schema.prisma   # Database schema
│   │   └── seed.ts         # Seed script
│   ├── tests/              # Jest tests
│   ├── Dockerfile
│   ├── package.json
│   └── .env.example
├── frontend/
│   ├── src/
│   │   ├── api/            # Axios instance + API calls
│   │   ├── components/     # Reusable components
│   │   ├── pages/          # Page components by role
│   │   ├── hooks/          # Custom hooks (socket, auth)
│   │   ├── store/          # Zustand stores
│   │   ├── i18n/           # Translations (EN/RW)
│   │   ├── lib/            # Utilities
│   │   └── App.tsx         # Main app with routing
│   ├── Dockerfile
│   ├── nginx.conf
│   ├── package.json
│   └── vite.config.ts
├── docker-compose.yml
└── README.md
```

## 🚀 Quick Start

### Prerequisites
- Node.js 20+
- PostgreSQL 16+
- Docker & Docker Compose (for containerized deployment)

### Local Development

1. **Clone and navigate**
   ```bash
   cd restaurant-system
   ```

2. **Backend setup**
   ```bash
   cd backend
   cp .env.example .env
   # Edit .env with your database URL and secrets
   npm install
   npx prisma generate
   npx prisma migrate dev --name init
   npx prisma db seed
   npm run dev
   ```

3. **Frontend setup** (new terminal)
   ```bash
   cd frontend
   npm install
   npm run dev
   ```

4. **Access the app**
   - Frontend: http://localhost:5173
   - Backend API: http://localhost:3000
   - Socket.io: http://localhost:3001

### Docker Deployment

1. **Configure environment**
   ```bash
   cp backend/.env.example backend/.env
   # Edit backend/.env with production secrets
   ```

2. **Build and start**
   ```bash
   docker-compose up -d --build
   ```

3. **Initialize database** (first run only)
   ```bash
   docker-compose exec backend npx prisma migrate deploy
   docker-compose exec backend npx prisma db seed
   ```

4. **Access the app**
   - Frontend: http://localhost:5173
   - Backend API: http://localhost:3000

## 🔐 Demo Accounts

After seeding, use these accounts:

| Role | Email | Password |
|------|-------|----------|
| Admin | admin@restaurant.com | password123 |
| Chef | chef@restaurant.com | password123 |
| Server | server@restaurant.com | password123 |
| Client | client@restaurant.com | password123 |

## 📚 API Documentation

### Authentication
```
POST   /api/auth/register     # Register new user
POST   /api/auth/login        # Login
POST   /api/auth/refresh      # Refresh access token
POST   /api/auth/logout       # Logout
GET    /api/auth/me           # Get current user
```

### Menu
```
GET    /api/categories              # List categories
GET    /api/categories/:id          # Get category with items
GET    /api/menu                    # List menu items (with filters)
GET    /api/menu/:id                # Get menu item
POST   /api/menu                    # Create item (admin)
PUT    /api/menu/:id                # Update item (admin)
DELETE /api/menu/:id                # Delete item (admin)
POST   /api/categories              # Create category (admin)
PUT    /api/categories/:id          # Update category (admin)
DELETE /api/categories/:id          # Delete category (admin)
```

### Orders
```
POST   /api/orders                  # Place order (client)
GET    /api/orders                  # List orders (filtered by role)
GET    /api/orders/:id              # Get order details
PATCH  /api/orders/:id/status       # Update status (chef/server/admin)
DELETE /api/orders/:id              # Cancel order
```

### Tables
```
GET    /api/tables                  # List tables
GET    /api/tables/:id              # Get table with active orders
POST   /api/tables                  # Create table (admin)
PUT    /api/tables/:id              # Update table (admin)
DELETE /api/tables/:id              # Delete table (admin)
POST   /api/tables/generate-qr      # Generate QR codes (admin)
```

### Users (Admin)
```
GET    /api/users                   # List users
GET    /api/users/stats             # User statistics
GET    /api/users/:id               # Get user
POST   /api/users                   # Create user
PUT    /api/users/:id               # Update user
DELETE /api/users/:id               # Delete user
```

### Reports (Admin)
```
GET    /api/reports/daily           # Daily sales report
GET    /api/reports/top-items       # Popular items
GET    /api/reports/revenue         # Revenue breakdown
```

### Notifications
```
GET    /api/notifications           # List notifications
GET    /api/notifications/unread-count  # Get unread count
PATCH  /api/notifications/:id/read  # Mark as read
PATCH  /api/notifications/read-all  # Mark all as read
DELETE /api/notifications/:id       # Delete notification
```

## 🔌 Socket.io Events

### Client → Server
```javascript
// Join role-based room
socket.emit('join_room', { room: 'chef_room' | 'server_room' | 'user_<userId>' });

// New order (client)
socket.emit('new_order', { orderId, tableId, items: [] });

// Status update (chef/server)
socket.emit('order_status_update', { orderId, status });
```

### Server → Client
```javascript
// Chef receives new order
socket.on('order_received', (data) => { /* orderId, tableNumber, items */ });

// Client order updates
socket.on('order_preparing', (data) => { /* orderId, message */ });
socket.on('order_ready', (data) => { /* orderId, tableNumber, items */ });
socket.on('order_delivered', (data) => { /* orderId, message */ });
socket.on('order_completed', (data) => { /* orderId, message */ });

// Generic notifications
socket.on('notification', (notification) => { /* notification object */ });
```

## 🌐 Internationalization

The app supports English and Kinyarwanda. Add translations in:
- `frontend/src/i18n/index.ts`

To add a new language:
1. Add translation object to resources
2. Update language selector in Layout component
3. Add to i18n initialization

## 🧪 Testing

```bash
# Backend tests
cd backend
npm test

# Frontend tests
cd frontend
npm test
```

## 📦 Production Deployment

### Environment Variables

**Backend (.env)**
```env
DATABASE_URL=postgresql://user:pass@host:5432/db
JWT_ACCESS_SECRET=your-super-secret-access-key
JWT_REFRESH_SECRET=your-super-secret-refresh-key
NODE_ENV=production
FRONTEND_URL=https://your-domain.com
PORT=3000
```

**Frontend (build time)**
```env
VITE_API_URL=https://api.your-domain.com/api
VITE_SOCKET_URL=https://api.your-domain.com
```

### SSL/HTTPS
Configure reverse proxy (nginx/Traefik) to handle SSL termination and forward to Docker containers.

### Database Migrations
```bash
docker-compose exec backend npx prisma migrate deploy
```

## 🛡 Security Checklist

- [ ] Change default JWT secrets in production
- [ ] Use strong database passwords
- [ ] Enable HTTPS/TLS
- [ ] Configure CORS for production domain only
- [ ] Set up rate limiting for auth endpoints
- [ ] Regular security updates for dependencies
- [ ] Enable database SSL connections
- [ ] Set up monitoring and logging

## 📝 License

MIT License - feel free to use for your projects!

## 🤝 Contributing

1. Fork the repository
2. Create feature branch
3. Commit changes
4. Push to branch
5. Open Pull Request

## 📞 Support

For issues and questions, please open a GitHub issue."# restaurant-system" 
"# restaurant-system" 
#   r e s t a u r a n t - s y s t e m  
 