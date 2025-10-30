# Toss Backend

A TypeScript/Node.js backend service for a coin flip game with streak tracking and wallet integration.

## 📋 Overview

Toss Backend is a RESTful API server that powers a coin toss game where players can:
- Generate unique game sessions
- Play coin flip games and build winning streaks
- Submit and track high scores linked to wallet addresses
- Compete for the highest streak records

## 🚀 Features

- **Session Management**: Generate unique hash identifiers for game sessions
- **Coin Flip Game Logic**: Fair random coin toss with streak tracking
- **Wallet Integration**: Store and retrieve player scores by wallet address
- **High Score Tracking**: Maintain leaderboard with highest streaks per wallet
- **CORS Support**: Configurable cross-origin resource sharing
- **MongoDB Integration**: Persistent data storage for game history
- **Static Frontend Serving**: Serves built frontend application

## 🛠️ Tech Stack

- **Runtime**: Node.js
- **Language**: TypeScript
- **Framework**: Express.js
- **Database**: MongoDB (Mongoose ODM)
- **Authentication**: JWT (JSON Web Tokens)
- **Blockchain**: Web3.js integration
- **Validation**: Joi with express-joi-validation

## 📦 Installation

1. Clone the repository:
```bash
git clone https://github.com/superdev947/toss-backend.git
cd toss-backend
```

2. Install dependencies:
```bash
npm install
```

3. Create a `.env` file in the root directory:
```env
PORT=8000
MONGOURL=mongodb://localhost:27017/tossgame
JWTSECRET=your_jwt_secret_key
JWTEXPIRATION=24h
WHITE_LIST=["http://localhost:3000"]
LIVE_TIME=3600
```

4. Ensure MongoDB is running on your system or update the `MONGOURL` with your MongoDB connection string.

## 🚦 Usage

### Development Mode
```bash
npm run dev
```
Runs the server with nodemon and ts-node for hot reloading.

### Build
```bash
npm run build
```
Compiles TypeScript to JavaScript in the `dist` folder.

### Production Mode
```bash
npm start
```
Runs the compiled JavaScript from the build.

### Watch Mode
```bash
npm run server
```
Concurrently runs TypeScript compiler in watch mode and nodemon.

## 🔌 API Endpoints

### Coinflip Routes (`/api/coinflip`)

#### `GET /api/coinflip/`
Generate a new unique hash for game session.

**Response:**
```json
{
  "data": {
    "userhash": "AbCdEf1234...",
    "streaks": 0
  }
}
```

#### `POST /api/coinflip/play`
Play a coin flip game.

**Request Body:**
```json
{
  "userhash": "string",
  "id": "hh|ht|th|tt"
}
```

**Response:**
```json
{
  "result": "hh|ht|th|tt",
  "streaks": 5
}
```

#### `POST /api/coinflip/submit`
Submit game results to wallet address.

**Request Body:**
```json
{
  "userhash": "string",
  "address": "0x..."
}
```

**Response:**
```json
{
  "data": {
    "wallet_address": "0x...",
    "streaks": 10
  }
}
```

#### `POST /api/coinflip/ps`
Store custom hash (for testing/admin purposes).

**Request Body:**
```json
{
  "userhash": "string"
}
```

## 📁 Project Structure

```
toss-backend/
├── config/
│   └── database.ts          # MongoDB connection configuration
├── src/
│   ├── build/               # Built frontend static files
│   ├── controllers/
│   │   └── coinflip.ts      # Game logic controllers
│   ├── middleware/
│   │   ├── auth.ts          # Authentication middleware
│   │   └── validation.ts    # Request validation
│   ├── models/
│   │   ├── UserHashes.ts    # Game session schema
│   │   └── Wallets.ts       # Wallet/score schema
│   ├── routes/
│   │   ├── coinflip.ts      # Coinflip routes
│   │   └── index.ts         # Main router
│   ├── types/
│   │   └── index.ts         # TypeScript type definitions
│   ├── config.ts            # Environment configuration
│   └── index.ts             # Application entry point
├── package.json
├── tsconfig.json
├── tslint.json
└── README.md
```

## 🎮 Game Logic

The coin flip game uses a two-coin system:
- First coin: Random flip (heads or tails)
- Second coin: Depends on first coin
  - If first is heads: second can be heads or tails
  - If first is tails: second must be tails
- Possible outcomes: `hh`, `ht`, `tt`
- Players guess the combination to maintain their streak
- Wrong guess resets streak to 0

## 🔒 Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `PORT` | Server port | `8000` |
| `MONGOURL` | MongoDB connection string | `mongodb://0.0.0.0:27017/tossgaem` |
| `JWTSECRET` | Secret key for JWT signing | - |
| `JWTEXPIRATION` | JWT token expiration time | - |
| `WHITE_LIST` | CORS allowed origins (JSON array) | - |
| `LIVE_TIME` | Session live time in seconds | - |

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
