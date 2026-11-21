# Expense Tracker — React Native + Express

A cross-platform expense-tracking app built with Expo and a small Express API. It helps a signed-in user record income and expenses and review their balance and transaction history.

> **Project scope:** This is an expense tracker, not a bank account, stored-value wallet, UPI app, or money-transfer service. It does not hold or move real funds.

## Features

- Email sign-up and sign-in with Clerk
- Home screen with balance and transaction history
- Add income and expense entries
- Delete transactions
- Pull-to-refresh transaction data
- Express API with PostgreSQL persistence
- API rate limiting with Upstash Redis
- Expo Router navigation for the mobile app

## Tech Stack

**Mobile**
- React Native 0.79
- Expo SDK 53
- Expo Router
- Clerk Expo
- React Navigation

**Backend**
- Node.js
- Express 4
- Neon serverless PostgreSQL
- Upstash Redis and rate limiting
- dotenv

## Repository Structure

```text
backend/
  src/
    config/       Database, Redis, and scheduled job setup
    controllers/  Transaction API handlers
    middleware/   API rate limiting
    routes/       Transaction routes
    server.js     Express application entry point
mobile/
  app/            Expo Router screens and layouts
  components/     Reusable mobile UI
  hooks/          Transaction data hook
  constants/      API configuration and UI constants
  assets/         App assets
```

## Prerequisites

- Node.js LTS and npm
- Expo Go or an Android emulator/device
- Clerk project
- Neon PostgreSQL database
- Upstash Redis database

## Environment Setup

### Backend

Create `backend/.env`:

```env
PORT=5001
NODE_ENV=development
DATABASE_URL=your_neon_postgres_connection_string
UPSTASH_REDIS_REST_URL=your_upstash_redis_rest_url
UPSTASH_REDIS_REST_TOKEN=your_upstash_redis_rest_token
```

### Mobile

Create `mobile/.env`:

```env
EXPO_PUBLIC_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
EXPO_PUBLIC_API_URL=https://your-deployed-api.example.com/api
```

Never commit real environment files, database connection strings, Redis tokens, or Clerk secret keys.

## Run Locally

### 1. Start the backend

```bash
cd backend
npm install
npm run dev
```

The API defaults to port `5001`. Health check: `GET /api/health`.

### 2. Start the mobile app

Open a second terminal:

```bash
cd mobile
npm install
npx expo start
```

Use the Expo terminal options to open the app on a device or emulator. Make sure the mobile API URL points to an address reachable from that device.

## API Overview

- `GET /api/health` — basic service health check
- `GET /api/transactions/:userId` — list a user's transactions
- `POST /api/transactions` — create a transaction
- `DELETE /api/transactions/:id` — delete a transaction
- `GET /api/transactions/summary/:userId` — retrieve balance, income, and expense totals

## Important Security Work Before Production

The current backend handlers use user IDs supplied in request paths/bodies. Before real users or sensitive data are involved:
- Verify Clerk session tokens on the backend.
- Derive the user identity from the verified token; do not trust a client-supplied `user_id`.
- Ensure every read/delete operation checks ownership.
- Validate transaction amounts and request payloads on the server.
- Add tests for authentication, authorization, and transaction ownership.
- Keep database and Redis credentials only in server-side environment variables.

## Project Status

Portfolio / learning project. Security hardening and end-to-end testing are required before production use.

## Author

**Bharath Simha Reddy** — [GitHub](https://github.com/Bharath170)
