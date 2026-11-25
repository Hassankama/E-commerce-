# Sprint 01 — Northstar Online Store

A bilingual-free (English UI) online store delivered as two independent applications:
- `backend`: Node.js + Express MVC + MongoDB (Mongoose), JWT and bcrypt.
- `frontend`: Angular standalone components, router, services, interfaces, guard and interceptor.

## Implemented scope

### Public customer pages
- Home page with categories and items.
- Item details page.
- Favorites stored in browser LocalStorage.
- Cart stored in browser LocalStorage.
- Buy / checkout page that submits an order with customer name, phone, address and optional notes.
- No customer account or online payment. The manager confirms and arranges delivery by phone.
- Item quantity is never shown on public pages.

### Manager
- JWT-protected manager login.
- Manager dashboard with Overview, Categories, Items and Orders.
- Create, update and delete categories.
- Create, update and delete items (quantity visible only in dashboard).
- View order details, customer details, opened/closed status and manager notes.
- Change order status between `opened` and `closed`.

## Requirements
- Node.js LTS and npm.
- MongoDB running locally or a MongoDB Atlas connection string.
- Angular CLI compatible with the Angular version in `frontend/package.json`.

## Setup — Windows CMD

### 1. Backend
Open CMD:

```cmd
cd sprint01-storeackend
npm install
copy .env.example .env
```

Open `backend\.env` and set a strong `JWT_SECRET_KEY`, MongoDB connection, manager username and password.

```cmd
npm run seed
npm run dev
```

API runs at `http://localhost:3001`.

### 2. Frontend
Open a second CMD window:

```cmd
cd sprint01-storerontend
npm install
npm start
```

Open `http://localhost:4200`.

Angular proxy forwards `/api` requests to `http://localhost:3001`.

## Default seeded manager
The username/password come from `.env`:
- `MANAGER_USERNAME`
- `MANAGER_PASSWORD`

Do not commit `.env` to GitHub. Change the seed password before using the app beyond local development.

## Main API endpoints

### Public
| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/v1.0/categories` | List categories |
| GET | `/api/v1.0/items` | List items (without stock quantity) |
| GET | `/api/v1.0/items/:id` | Item details (without stock quantity) |
| POST | `/api/v1.0/orders` | Submit customer order |
| POST | `/api/v1.0/signin` | Manager login |

### Manager — Bearer JWT required
| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/api/v1.0/categories` | Create category |
| PUT | `/api/v1.0/categories/:id` | Update category |
| DELETE | `/api/v1.0/categories/:id` | Delete category |
| POST | `/api/v1.0/items` | Create item |
| PUT | `/api/v1.0/items/:id` | Update item |
| DELETE | `/api/v1.0/items/:id` | Delete item |
| GET | `/api/v1.0/orders` | List all orders |
| GET | `/api/v1.0/orders/:id` | Order details |
| PUT | `/api/v1.0/orders/:id` | Update status and notes |

## Notes
- The API deliberately removes `quantity` from public item responses.
- Orders capture item names and prices at checkout so a later catalog edit does not rewrite old order details.
- Inventory is displayed and managed by the manager, but stock deduction / reservation is not part of the Sprint 01 workflow because the requirements did not specify inventory movement on order submission.
- Orders are created as `opened`. The manager can mark them `closed` after phone confirmation / delivery workflow.
- For a production deployment, add stronger input validation, rate limiting, logging, HTTPS, and a proper order stock-reservation policy.
