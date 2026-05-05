<div align="center">

<img src="https://img.shields.io/badge/FinVault-Fintech%20Wallet%20Platform-4CAF50?style=for-the-badge&logo=wallet&logoColor=white" alt="FinVault"/>

# FinVault

**A production-grade fintech wallet platform for digital payments, expense management, KYC verification, and social bill splitting.**


---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Core Workflows](#core-workflows)
- [API Reference](#api-reference)
- [Getting Started](#getting-started)
- [Deployment](#deployment)
- [Roadmap](#roadmap)

---

## Overview

FinVault is a full-stack fintech application that simulates core banking and payment workflows in a clean, modern interface. Built with a modular architecture and secure JWT-based authentication, it handles everything from user onboarding and KYC verification to wallet transfers, expense tracking, and group bill settlements.

It is designed to reflect production-level engineering practices — including role-based access control, ledger-driven financial history, and KYC-gated transaction flows.

> **Live:** [https://fin-vault-zeta.vercel.app](https://fin-vault-zeta.vercel.app)

---
```
To approve your KYC (Login as admin): email- sumit@test.com
                                      password- 123456
```
## Features

| Module | Capabilities |
|---|---|
| **Authentication** | JWT-based registration & login, protected routes, role-aware navigation |
| **KYC Verification** | User KYC submission, admin approval/rejection, KYC-gated wallet access |
| **Wallet & Transfers** | Default wallet balance, send money to connections or any registered user |
| **Expenses** | Record external expenses, wallet deduction, category tracking |
| **Passbook** | Full debit/credit history with timestamps, categories, and human-readable descriptions |
| **Connections** | Send, accept, reject, withdraw, and delete user connections |
| **Split Bills** | Create splits with connected users, auto-invite non-connected members, settle shares |
| **Analytics** | Category-wise and monthly spending insights powered by ledger data |

---

## Tech Stack

```
Frontend    React 18 + Vite · Material UI · Pastel Green Fintech Theme
Backend     Java 17 · Spring Boot · Spring Security
Auth        JWT (JSON Web Tokens) · Role-Based Access Control
Database    MongoDB Atlas
Deployment  Vercel (Frontend) · Render (Backend)
Version     Git + GitHub
```

---

## Architecture

```
FinVault
├── finvault-backend
│   ├── config/              # Security config, CORS, JWT filter setup
│   ├── controller/          # REST controllers for each domain
│   ├── dto/                 # Request/response transfer objects
│   ├── model/               # MongoDB document models
│   ├── repository/          # Spring Data MongoDB repositories
│   ├── security/            # JWT provider, auth filter, user details service
│   ├── service/             # Business logic layer
│   └── resources/           # application.properties, env config
│
└── finvault-frontend
    └── src
        ├── api/             # Axios instance and endpoint definitions
        ├── components/      # Reusable UI components
        ├── layout/          # Sidebar, topbar, mobile navigation
        ├── pages/           # Route-level page components
        ├── services/        # Frontend service layer (auth, wallet, etc.)
        └── theme/           # MUI theme overrides, color palette
```

---

## Core Workflows

```
User registers / logs in
        ↓
User submits KYC documents
        ↓
Admin reviews and approves KYC
        ↓
Wallet features unlocked
        ↓
User sends money · records expenses · creates split bills
        ↓
Ledger entries auto-generated on every transaction
        ↓
Passbook and analytics reflect real-time ledger state
```

### Split Bill Flow

```
Creator adds members (connections or email)
        ↓
Non-connected members receive auto connection invite
        ↓
Full bill amount deducted from creator wallet
        ↓
Members settle their individual share
        ↓
Settlement amount credited back to creator
```

---

## API Reference

### Authentication
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/auth/register` | Register a new user |
| `POST` | `/api/auth/login` | Login and receive JWT |

### User
| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/user/profile` | Fetch authenticated user profile |
| `PUT` | `/api/user/profile` | Update user profile |

### KYC
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/kyc/submit` | Submit KYC documents |
| `GET` | `/api/kyc/status` | Check KYC status |

### Admin
| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/admin/kyc/pending` | List pending KYC submissions |
| `POST` | `/api/admin/kyc/approve` | Approve a KYC submission |
| `POST` | `/api/admin/kyc/reject` | Reject a KYC submission |

### Connections
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/connections/request` | Send connection request |
| `POST` | `/api/connections/{id}/accept` | Accept a connection request |
| `POST` | `/api/connections/{id}/reject` | Reject a connection request |
| `POST` | `/api/connections/{id}/withdraw` | Withdraw a sent request |
| `DELETE` | `/api/connections/{id}` | Remove an existing connection |
| `GET` | `/api/connections/my` | List all connections |

### Transfers & Payments
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/transfer/send` | Send money to a user |
| `POST` | `/api/payments/initiate` | Record an external expense |
| `GET` | `/api/payments/my` | List all expense payments |

### Passbook & Analytics
| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/ledger/passbook` | Full transaction passbook |
| `GET` | `/api/ledger/summary` | Balance summary |
| `GET` | `/api/ledger/category` | Category-wise spending |
| `GET` | `/api/ledger/monthly` | Monthly spending breakdown |

### Split Bills
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/split-bills` | Create a new split bill |
| `GET` | `/api/split-bills/my` | List active and settled split bills |
| `POST` | `/api/split-bills/{id}/settle` | Settle a member's share |

---

## Getting Started

### Prerequisites

- Java 17+
- Node.js 18+
- MongoDB Atlas account

### Backend Setup

```bash
cd finvault-backend
./mvnw spring-boot:run
```

Backend runs on `http://localhost:8080`

**Required configuration** (`application.properties` or environment):

```properties
spring.data.mongodb.uri=<your-mongodb-atlas-uri>
server.port=${PORT:8080}
```

### Frontend Setup

```bash
cd finvault-frontend
npm install
npm run dev
```

Frontend runs on `http://localhost:5173`

**Required `.env`:**

```env
# Local development
VITE_API_URL=http://localhost:8080/api

# Production
VITE_API_URL=https://your-render-backend-url/api
```

---

## Deployment

| Service | Platform | URL |
|---|---|---|
| Frontend | Vercel | [fin-vault-zeta.vercel.app](https://fin-vault-zeta.vercel.app) |
| Backend | Render | *(your Render URL)* |
| Database | MongoDB Atlas | *(managed cloud cluster)* |

A `Dockerfile` is included for containerized backend deployment.

---

## Roadmap

- [ ] Dedicated wallet management page
- [ ] In-app notifications system
- [ ] Cloud-based avatar upload
- [ ] Loan application and admin approval workflow
- [ ] Investment plans with admin-managed interest rates
- [ ] PDF export for passbook and analytics reports
- [ ] Transaction idempotency and duplicate prevention
- [ ] Fraud detection rules engine
- [ ] Advanced analytics dashboard

---

## Author

**Sumit Kumar** · Full-Stack Developer

[![GitHub](https://img.shields.io/badge/GitHub-sumit--kumar-181717?style=flat-square&logo=github)](https://github.com/developsumitkumar/)

---

<div align="center">
  <sub>Built with Spring Boot, React, and MongoDB Atlas</sub>
</div>
