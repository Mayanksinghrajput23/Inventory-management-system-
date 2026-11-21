# Inventory and Data Management System

A REST API built with **Node.js, Express.js, MongoDB and Mongoose** to manage a store's product catalogue. You can create products, adjust stock, filter/search/paginate products, and generate inventory reports using aggregation.

## Tech Stack

- Node.js
- Express.js
- MongoDB + Mongoose
- dotenv
- nodemon (dev)

## Project Structure

```
inventory-management-system/
├── config/
│   └── db.js                  # MongoDB connection
├── controllers/
│   └── productController.js   # Business logic
├── middleware/
│   ├── asyncHandler.js        # Async error wrapper
│   └── errorHandler.js        # Central error handling + 404
├── models/
│   └── Product.js             # Mongoose schema + validation
├── routes/
│   └── productRoutes.js       # API routes
├── .env.example               # Environment variable template
├── package.json
├── server.js                  # Entry point
└── README.md
```

## Requirements

- [VS Code](https://code.visualstudio.com/)
- [Node.js](https://nodejs.org/) (LTS)
- MongoDB (local installation or MongoDB Atlas)

## How to Run in VS Code

1. Clone the repository:
   ```bash
   git clone <your-repo-url>
   ```
2. Open the project folder in VS Code (**File → Open Folder**).
3. Open the integrated terminal with `Ctrl + ``.
4. Install dependencies:
   ```bash
   npm install
   ```
5. Create a `.env` file from the template:
   ```bash
   # macOS / Linux / Git Bash
   cp .env.example .env

   # Windows
   copy .env.example .env
   ```
6. Make sure MongoDB is running.
7. Start the server:
   ```bash
   npm run dev
   ```
8. Open http://localhost:5000 in your browser. You should see:
   `Inventory and Data Management System API is running...`

Use `npm start` instead of `npm run dev` if you don't want auto-restart.

## Environment Variables

```env
PORT=5000
MONGO_URI=mongodb://127.0.0.1:27017/inventory_db
```

For MongoDB Atlas, use your own connection string:

```env
MONGO_URI=mongodb+srv://<username>:<password>@cluster0.mongodb.net/inventory_db
```

## Recommended VS Code Extensions

- **MongoDB for VS Code**: browse your database inside VS Code
- **REST Client** or **Thunder Client**: test API requests inside VS Code
- **DotENV**: syntax highlighting for `.env` files

## API Endpoints

Base URL: `http://localhost:5000/api/products`

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST   | `/api/products` | Create a new product |
| GET    | `/api/products` | Get all products (filter, sort, paginate) |
| GET    | `/api/products/low-stock` | Products at or below reorder level |
| GET    | `/api/products/summary` | Category-wise inventory summary |
| GET    | `/api/products/:id` | Get a single product |
| PUT    | `/api/products/:id` | Update a product |
| PATCH  | `/api/products/:id/stock` | Restock or record a sale |
| DELETE | `/api/products/:id` | Delete a product |

### Create Product

`POST /api/products`

```json
{
  "name": "Gel Pen Pack of 15",
  "sku": "STNY-PN-713",
  "category": "Stationery",
  "price": 120,
  "quantity": 40,
  "reorderLevel": 20,
  "supplier": "Cello Pens"
}
```

### Get All Products

`GET /api/products` supports these query parameters:

| Parameter | Description | Example |
|-----------|-------------|---------|
| `category` | Filter by category | `?category=Furniture` |
| `supplier` | Filter by supplier | `?supplier=Cello Pens` |
| `minPrice`, `maxPrice` | Price range | `?minPrice=100&maxPrice=5000` |
| `inStock` | `true` or `false` | `?inStock=true` |
| `search` | Search by name or SKU | `?search=table` |
| `sort` | Sort field, `-` for descending | `?sort=-price` |
| `page`, `limit` | Pagination | `?page=2&limit=5` |

Example:

```
GET /api/products?category=Furniture&sort=-price&page=1&limit=10
```

### Adjust Stock

`PATCH /api/products/:id/stock`

```json
{ "change": 10 }
```

Use a positive number to restock and a negative number to record a sale. Quantity can never go below 0.

### Category Summary

`GET /api/products/summary` returns, for each category: `totalItems`, `totalQuantity`, `totalStockValue` and `avgPrice`.

## Error Response Format

```json
{
  "success": false,
  "message": "Description of what went wrong"
}
```

## Troubleshooting

- **`ECONNREFUSED 127.0.0.1:27017`**: MongoDB is not running. Start it and restart the server.
- **`EADDRINUSE: address already in use`**: Port 5000 is busy. Change `PORT` in `.env`.
- **`MONGO_URI` undefined**: Make sure `.env` is in the project root and restart the server.
- **`nodemon is not recognized`**: Run `npm install` first.

## Concepts Covered

Mongoose schema design, field-level validation, virtual fields, REST CRUD, filtering, sorting, pagination, aggregation pipelines, middleware, centralized error handling, dotenv.
