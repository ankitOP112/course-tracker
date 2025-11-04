# MERN Stack Application with Next.js

A full-stack application template using MongoDB, Express, Next.js, and Node.js.

## Project Structure

```
mern-app/
├── frontend/          # Next.js application
├── backend/           # Express.js API server
├── package.json       # Root package.json with scripts
└── README.md          # This file
```

## Prerequisites

- Node.js (v18 or higher)
- npm or yarn
- MongoDB (local or MongoDB Atlas)

## Getting Started

### 1. Install Dependencies

```bash
npm run install-all
```

Or install manually:

```bash
npm install
cd backend && npm install
cd ../frontend && npm install
```

### 2. Environment Setup

#### Backend Environment Variables

Create a `.env` file in the `backend` directory:

```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/mern-app
JWT_SECRET=your_jwt_secret_key
NODE_ENV=development
```

#### Frontend Environment Variables

Create a `.env.local` file in the `frontend` directory:

```env
NEXT_PUBLIC_API_URL=http://localhost:5000/api
```

### 3. Start MongoDB

Make sure MongoDB is running on your system or update the connection string to use MongoDB Atlas.

### 4. Run the Application

Run both frontend and backend concurrently:

```bash
npm run dev
```

Or run them separately:

```bash
# Terminal 1 - Backend
npm run server

# Terminal 2 - Frontend
npm run client
```

## Available Scripts

- `npm run dev` - Run both frontend and backend in development mode
- `npm run server` - Run backend server only
- `npm run client` - Run frontend development server only
- `npm run build` - Build the Next.js application for production
- `npm start` - Start production server

## Tech Stack

### Frontend

- **Next.js 14** - React framework with App Router
- **TypeScript** - Type safety
- **Tailwind CSS** - Utility-first CSS framework

### Backend

- **Express.js** - Web framework
- **MongoDB** - Database
- **Mongoose** - MongoDB object modeling
- **JWT** - Authentication

## Features

- ✅ Modern Next.js App Router
- ✅ Express.js RESTful API
- ✅ MongoDB connection with Mongoose
- ✅ Authentication setup (JWT)
- ✅ CORS configuration
- ✅ Error handling middleware
- ✅ Environment variable configuration
- ✅ Responsive UI with Tailwind CSS

## API Endpoints

The backend API runs on `http://localhost:3001/api`

Example endpoints:

- `GET /api/users` - Get all users
- `POST /api/users` - Create a user
- `GET /api/users/:id` - Get user by ID

## License

MIT
