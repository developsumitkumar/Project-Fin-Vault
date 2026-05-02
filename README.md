

# FinVault

### A full-stack fintech platform simulating real-world financial workflows —
### KYC onboarding, payment processing, and personal finance tracking.

<br/>

</div>

---

## Overview

FinVault is a production-style fintech application built from scratch to demonstrate end-to-end backend architecture, compliance-aware design, and modern full-stack development. It mirrors the core workflows found in real financial products — secure onboarding, KYC verification, payment lifecycle management, and a passbook-style expense ledger with group split-bill settlement.

This project is being developed incrementally, with each module building on the last to reflect how fintech systems are actually structured in the industry.

---

## System Flow

```
User Registration & Login
        ↓
  Submit KYC Documents
        ↓
  Admin Review & Approval
        ↓
  Payment Initiated (with category tag)
        ↓
  Ledger Entry created automatically
        ↓
  Category-wise Analytics & Summary
        ↓
  Split Bill → Settlement → Payment → Ledger
```

---

## Architecture

```
React Frontend (Vite)
        │
        ▼
Spring Boot API Gateway  ←  JWT Authentication + Role-Based Access
        │
   ┌────┼────────────────┐
   ▼    ▼                ▼
 KYC  Payments        Finance
Module  Module         Module
   │    │                │
   │  (gates)       (ledger + splits)
   └────┴────────────────┘
              │
              ▼
        MongoDB Atlas
  (users · kyc_docs · payments
   ledger_entries · split_bills)
```

**Module dependency chain:**
KYC verification → gates payment access → every payment auto-creates a ledger entry → ledger entries power category analytics and split-bill settlement.

---

## Feature Progress

### Auth & Security
| Feature | Status |
|---|---|
| JWT login / register | ✅ Done |
| Role-based access (USER / ADMIN) | ✅ Done |
| Token refresh & expiry handling | ⏳ Planned |

### KYC Module
| Feature | Status |
|---|---|
| KYC document submission | ✅ Done |
| Admin approval / rejection workflow | ✅ Done |
| Status states: SUBMITTED → IN REVIEW → VERIFIED / REJECTED | ✅ Done |
| KYC gating on payment access | ✅ Done |

### Payments Module
| Feature | Status |
|---|---|
| Initiate & process mock transactions | ✅ Done |
| Payment category tagging | ✅ Done |
| Payment states: INITIATED → PROCESSING → SUCCESS / FAILED / FLAGGED | 🚧 In Progress |
| Idempotency key enforcement | ⏳ Planned |
| Rule-based fraud flagging | ⏳ Planned |
| Reconciliation report | ⏳ Planned |

### Finance & Ledger Module
| Feature | Status |
|---|---|
| Single-entry expense ledger (passbook model) | ✅ Done |
| Category-wise expense tracking | ✅ Done |
| Group expense splitting (multi-user settlement) | ✅ Done |
| Debt settlement with payment record | ✅ Done |
| Monthly category reports & summary | ✅ Done |

### Frontend (React + Vite)
| Feature | Status |
|---|---|
| Project scaffold (Vite + React) | ✅ Done |
| Auth screens (login / register) | ⏳ Planned |
| KYC submission form | ⏳ Planned |
| Admin dashboard | ⏳ Planned |
| Transaction history & ledger view | ⏳ Planned |
| Spending analytics dashboard | ⏳ Planned |

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Java 17 |
| Backend framework | Spring Boot 3 |
| Security | Spring Security + JWT |
| Database | MongoDB Atlas |
| Frontend | React 18 + Vite |
| UI components | Material UI (MUI) |
| API style | RESTful JSON APIs |
| Build tool | Maven (mvnw) |
| Version control | Git + GitHub |

---

## Project Structure
```
FinVault/
│
├── finvault-backend/
│   ├── src/
│   │   ├── main/
│   │   │   └── java/com/finvault/
│   │   │       ├── config/
│   │   │       │   └── SecurityConfig.java              # Spring Security + JWT filter chain
│   │   │       ├── controller/
│   │   │       │   ├── AdminController.java              # Admin: KYC approval/rejection
│   │   │       │   ├── AuthController.java               # Register, login endpoints
│   │   │       │   ├── KycController.java                # KYC submission & status
│   │   │       │   ├── LedgerController.java             # Passbook, summary & category views
│   │   │       │   ├── PaymentController.java            # Initiate payments, payment history
│   │   │       │   ├── SplitBillController.java          # Group splits & settlement
│   │   │       │   ├── UserController.java               # User profile endpoints
│   │   │       │   └── TestController.java               # Dev/test endpoint
│   │   │       ├── dto/
│   │   │       │   ├── AuthResponse.java                 # JWT token response wrapper
│   │   │       │   ├── LoginRequest.java                 # Login payload
│   │   │       │   └── RegisterRequest.java              # Registration payload
│   │   │       ├── model/
│   │   │       │   ├── Kyc.java                          # KYC document (status, timestamps)
│   │   │       │   ├── LedgerEntry.java                  # Passbook entry (debit/credit, category)
│   │   │       │   ├── Payment.java                      # Payment record (amount, status, category)
│   │   │       │   ├── SplitBill.java                    # Group expense document
│   │   │       │   ├── SplitBillMember.java              # Per-member share & settlement status
│   │   │       │   └── User.java                         # User document (roles, KYC status)
│   │   │       ├── repository/
│   │   │       │   ├── KycRepository.java                # MongoDB KYC queries
│   │   │       │   ├── LedgerRepository.java             # MongoDB ledger entry queries
│   │   │       │   ├── PaymentRepository.java            # MongoDB payment queries
│   │   │       │   ├── SplitBillRepository.java          # MongoDB split bill queries
│   │   │       │   └── UserRepository.java               # MongoDB user queries
│   │   │       ├── security/
│   │   │       │   ├── CustomUserDetailsService.java     # Spring Security user loading
│   │   │       │   ├── JwtAuthenticationFilter.java      # Request-level JWT filter
│   │   │       │   └── JwtService.java                   # Token generation & validation
│   │   │       ├── service/
│   │   │       │   ├── KycService.java                   # KYC workflow & status transitions
│   │   │       │   ├── LedgerService.java                # Ledger entry creation & queries
│   │   │       │   ├── PaymentService.java               # Payment processing & KYC gating
│   │   │       │   ├── SplitBillService.java             # Split logic & settlement flow
│   │   │       │   └── UserService.java                  # User business logic
│   │   │       └── FinvaultBackendApplication.java       # Application entry point
│   │   ├── resources/
│   │   └── test/
│   ├── pom.xml
│   ├── mvnw / mvnw.cmd
│   └── HELP.md
│
├── finvault-frontend/
│   ├── public/
│   ├── src/
│   │   ├── assets/
│   │   ├── App.jsx                                       # Root component & routing
│   │   ├── App.css
│   │   ├── main.jsx                                      # React DOM entry point
│   │   └── index.css
│   ├── index.html
│   ├── package.json
│   ├── eslint.config.js
│   └── .gitignore
│
└── .gitignore
```
---

## Getting Started

### Prerequisites

- Java 17+
- Maven (or use the included `mvnw` wrapper)
- MongoDB Atlas account (or local MongoDB)
- Node.js 18+ *(for frontend)*

### Backend Setup

```bash
# Clone the repository
git clone https://github.com/developsumitkumar/FinVault.git
cd FinVault/finvault-backend

# Configure MongoDB URI in src/main/resources/application.yml
# spring.data.mongodb.uri=mongodb+srv://<user>:<pass>@cluster.mongodb.net/finvault

# Run the backend
./mvnw spring-boot:run
```

The API will be available at `http://localhost:8080`.

### Frontend Setup

```bash
cd FinVault/finvault-frontend
npm install
npm run dev
```

The frontend will be available at `http://localhost:5173`.

---

## API Reference

### Auth
```
POST   /api/auth/register               Register a new user
POST   /api/auth/login                  Login and receive JWT
```

### KYC
```
POST   /api/kyc/submit                  Submit KYC documents (USER)
GET    /api/kyc/status                  Get own KYC status (USER)
```

### Admin
```
POST   /api/admin/kyc/approve           Approve a KYC application (ADMIN)
POST   /api/admin/kyc/reject            Reject a KYC application (ADMIN)
```

### Payments
```
POST   /api/payments/initiate           Initiate a payment (KYC-gated)
GET    /api/payments/my                 View own payment history
```

### Ledger
```
GET    /api/ledger/my                   View full passbook / ledger
GET    /api/ledger/summary              Total credits & debits summary
GET    /api/ledger/category             Category-wise expense breakdown
```

### Split Bills
```
POST   /api/split-bills                 Create a group split expense
GET    /api/split-bills/my             View all splits involving you
POST   /api/split-bills/{id}/settle    Settle your share (creates payment + ledger entry)
```

---

## Key Concepts Demonstrated

- **JWT authentication** — stateless auth with role claims embedded in the token, validated on every request via `JwtAuthenticationFilter`
- **Role-based access control** — `USER` and `ADMIN` roles enforced via Spring Security at the controller level
- **KYC compliance workflow** — status state machine (SUBMITTED → IN REVIEW → VERIFIED / REJECTED) with admin-controlled transitions
- **KYC gating** — payment APIs blocked for users whose KYC is not verified
- **Passbook-style ledger** — every payment automatically generates a ledger entry for full transaction history
- **Category-based analytics** — expenses tagged by category and queryable via summary and breakdown endpoints
- **Split-bill settlement** — multi-user expense splitting where settlement triggers a real payment and ledger entry
- **Layered architecture** — clean separation of controller → service → repository → model across all modules

---

## Roadmap

```
[✅] Phase 1 — Auth & JWT (login, register, role-based access)
[✅] Phase 2 — KYC module (submission, admin approval, gating)
[✅] Phase 3 — Payments module + ledger + split-bill settlement
[🚧] Phase 4 — Advanced analytics + frontend (React + MUI)
[⏳] Phase 5 — Fraud detection & idempotency
[⏳] Phase 6 — Deployment (Railway / Render)
```

---

## Author

**Sumit Kumar**
Full-Stack Engineer · NIIT StackRoute Certified
