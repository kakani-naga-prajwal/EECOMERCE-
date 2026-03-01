# E-Commerce Order Processing – Full-Stack Application

Production-ready full-stack e-commerce with order processing, JWT auth, cart, and admin dashboard.

## Tech Stack

- **Frontend:** React (Vite), React Router, Axios, Context API, Tailwind CSS, form validation, protected routes
- **Backend:** Node.js, Express.js, PostgreSQL, Sequelize ORM, JWT, bcrypt, RESTful API, MVC structure
- **Database:** PostgreSQL with UUID primary keys, foreign keys, indexes

## Folder Structure

```
ecommerce/
├── backend/                 # Express API
│   ├── src/
│   │   ├── config/          # database, syncDb
│   │   ├── controllers/     # auth, product, cart, order
│   │   ├── middleware/     # auth, errorHandler, logging, validate
│   │   ├── models/         # User, Category, Product, CartItem, Order, OrderItem
│   │   ├── routes/         # auth, product, category, cart, order, admin
│   │   └── server.js
│   ├── .env.example
│   └── package.json
├── frontend/                # React Vite app
│   ├── src/
│   │   ├── api/
│   │   ├── components/
│   │   ├── context/         # AuthContext, CartContext
│   │   ├── pages/
│   │   └── main.jsx, App.jsx, index.css
│   ├── index.html
│   ├── tailwind.config.js
│   ├── vite.config.js
│   └── package.json
├── database/
│   └── schema.sql           # PostgreSQL schema for pgAdmin
└── docs/
    └── API.md               # API documentation
```

## Prerequisites

- Node.js 18+
- PostgreSQL (and pgAdmin if you prefer GUI)
- npm or yarn

## 1. Database Setup (PostgreSQL / pgAdmin)

1. Create a database, e.g. `ecommerce_db`.
2. Run the schema:
   - Open pgAdmin and connect to your server.
   - Select the database and open Query Tool.
   - Copy and run the contents of `database/schema.sql`.

Alternatively with Sequelize sync (creates tables automatically):

- Skip running `schema.sql` and use the backend’s sync script (see Backend below). Schema file is still useful for reference and manual setup.

## 2. Backend Setup

```bash
cd ecommerce/backend
cp .env.example .env
```

Edit `.env`:

```env
NODE_ENV=development
PORT=5000
DB_HOST=localhost
DB_PORT=5432
DB_NAME=ecommerce_db
DB_USER=postgres
DB_PASSWORD=your_postgres_password
JWT_SECRET=your_super_secret_jwt_key_change_in_production
JWT_EXPIRES_IN=7d
```

Install and run:

```bash
npm install
npm run db:sync    # Syncs Sequelize models to DB (creates/alters tables)
npm run dev        # Start server (with --watch), or: npm start
```

Server runs at `http://localhost:5000`.

## 3. Frontend Setup

```bash
cd ecommerce/frontend
npm install
npm run dev
```

Frontend runs at `http://localhost:3000` and proxies `/api` to the backend (see `vite.config.js`).

## 4. Environment Variables Summary

### Backend (`.env` in `backend/`)

| Variable       | Description                |
|----------------|----------------------------|
| PORT           | Server port (default 5000) |
| DB_HOST        | PostgreSQL host            |
| DB_PORT        | PostgreSQL port            |
| DB_NAME        | Database name              |
| DB_USER        | Database user              |
| DB_PASSWORD    | Database password          |
| JWT_SECRET     | Secret for JWT signing     |
| JWT_EXPIRES_IN | Token expiry (e.g. 7d)     |

### Frontend

No env file required for local dev; API is proxied to `http://localhost:5000`. For a different API URL, set `VITE_API_URL` and use it in `src/api/axios.js` if you add that later.

## 5. Features

1. **Authentication:** Register (customer/admin), login, JWT, role-based access, password hashing (bcrypt).
2. **Products:** Admin CRUD, categories, image URL, stock; list with search, filter, sort, pagination.
3. **Cart:** Add, update quantity, remove; cart stored in DB and synced on login.
4. **Orders:** Place order, unique order number, status workflow (pending → confirmed → shipped → delivered / cancelled), auto-reduce stock, order items stored separately.
5. **Payment:** Dummy gateway; simulate payment and set payment status to paid.
6. **Admin:** Dashboard (revenue, orders count, by status), manage products, view orders (filter by status), view users.
7. **Quality:** Validation (express-validator), error and logging middleware, proper HTTP status codes.

## 6. API Documentation

See `docs/API.md` for full endpoint list, request/response shapes, and status codes.

## 7. Quick Test

1. Register a user (e.g. role: customer).
2. Register another with role **admin** (or create in DB).
3. Login as admin → create categories and products.
4. Login as customer → browse products, add to cart, checkout, place order, open order and “Simulate Payment”.
5. Login as admin → open Admin → view orders, users, analytics.

## 8. Production Notes

- Set a strong `JWT_SECRET` and keep `.env` out of version control.
- Use HTTPS and secure cookies if you add session/cookie-based options later.
- Run frontend build: `npm run build` (in `frontend/`) and serve the `dist` folder with your preferred server or reverse proxy.
- Run backend with a process manager (e.g. PM2) and put a reverse proxy (e.g. Nginx) in front of both if needed.
# EECOMERCE-
