# 🦷 DentalCare - Healthcare & Dental Clinic Booking Platform

A modern, full-stack healthcare and dental clinic booking platform built with microservices architecture, real-time scheduling, integrated payments, and comprehensive observability.

## 📋 Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [API Documentation](#api-documentation)
- [Development](#development)
- [Testing](#testing)
- [Deployment](#deployment)
- [Contributing](#contributing)

---

## 🎯 Overview

DentalCare is an enterprise-grade booking platform designed for healthcare and dental clinics. It provides patients with an intuitive interface to browse clinics, check availability, and book appointments. Dentists and clinic administrators get a comprehensive management dashboard with scheduling, financial tracking, and patient communication tools.

**Current Version:** 0.0.0  
**Created:** June 2026  
**License:** MIT

---

## ✨ Key Features

### Frontend
- 🎨 **Modern UI** - Built with React 19 and Tailwind CSS
- 🗺️ **Google Maps Integration** - Real-time clinic location discovery
- ⚡ **Fast Performance** - Optimized with Vite and HMR (Hot Module Replacement)
- 🧭 **Client-side Routing** - Seamless navigation with React Router v6
- 📱 **Responsive Design** - Mobile-first approach with Tailwind CSS

### Backend
- 🏛️ **Microservices Architecture** - Independent, scalable services
- 🔐 **Authentication & Authorization** - Centralized via BFF layer
- 💳 **Payment Integration** - Braintree payment processing
- 🔗 **gRPC Communication** - High-performance inter-service communication
- 📊 **Real-time Scheduling** - Advanced booking engine
- 📈 **Observability Stack**
  - Spring Boot Actuator for health checks & metrics
  - Prometheus integration for monitoring
  - Distributed tracing capabilities

### Database & Data
- 🐘 **PostgreSQL** - Reliable, ACID-compliant relational database
- 📦 **Connection Pooling** - Optimized performance

### Quality Assurance
- ✅ **JUnit Tests** - Comprehensive unit testing
- 🐳 **Testcontainers** - Containerized integration testing
- 🔍 **ESLint** - Frontend code quality
- 📐 **Type Safety** - TypeScript support (dev)

---

## 🛠️ Tech Stack

### Frontend
| Technology | Version | Purpose |
|-----------|---------|---------|
| React | 19.0.0 | UI Framework |
| Vite | 6.2.0 | Build tool & dev server |
| Tailwind CSS | 3.4.0 | Styling |
| React Router | 6.22.3 | Client-side routing |
| React Icons | 5.0.1 | Icon library |
| Google Maps API | 2.19.3 | Location services |
| TypeScript | 19.0.10 | Type safety (dev) |
| ESLint | 9.21.0 | Code linting |
| PostCSS | 8.4.32 | CSS preprocessing |
| Autoprefixer | 10.4.16 | CSS vendor prefixes |

### Backend
| Technology | Version | Purpose |
|-----------|---------|---------|
| Spring Boot | 3.x | Framework |
| Java | 21 | Language |
| PostgreSQL | Latest | Database |
| gRPC | Latest | Service communication |
| Braintree | Latest | Payment processing |
| Spring Actuator | Latest | Monitoring/metrics |
| Prometheus | Latest | Metrics collection |
| JUnit | 5.x | Testing |
| Testcontainers | Latest | Integration testing |

---

## 🏗️ Architecture

DentalCare follows a **microservices architecture** with a layered design:

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         CLIENT LAYER                                    │
│                                                                           │
│   ┌──────────────────────────────────────────────────────────────────┐  │
│   │  React Frontend (localhost:5173)                                 │  │
│   │  ├─ Pages (HomePage, FindDispensaries, DispensaryDetails)       │  │
│   │  ├─ Components (AuthModal, Header, SearchBar, Maps)             │  │
│   │  ├─ React Router v6                                             │  │
│   │  └─ Google Maps API Integration                                 │  │
│   └──────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                    ┌───────────────┴───────────────┐
                    │                               │
         ┌──────────▼────────────┐    ┌────────────▼──────────────┐
         │   BFF SERVICE         │    │  MAIN BACKEND SERVICE     │
         │  (localhost:8005)     │    │   (localhost:8080)        │
         │                       │    │                           │
         │ ┌─────────────────┐   │    │ ┌─────────────────────┐   │
         │ │ AuthController  │   │    │ │   Controllers       │   │
         │ │ - Register      │   │    │ ├─ Dispensary        │   │
         │ │ - Login         │   │    │ ├─ Doctor            │   │
         │ └─────────────────┘   │    │ ├─ Patient           │   │
         │       │               │    │ └─ Payment           │   │
         │       ▼               │    │                       │   │
         │ ┌─────────────────┐   │    │ ┌─────────────────────┐   │
         │ │ AuthService     │   │    │ │   Services          │   │
         │ ├─ JwtService     │   │    │ ├─ Dispensary        │   │
         │ ├─ BCrypt         │   │    │ ├─ Doctor            │   │
         │ └─ UserRepository │   │    │ ├─ Patient           │   │
         │                   │    │    │ ├─ Booking           │   │
         │                   │    │    │ ├─ Slot              │   │
         │                   │    │    │ ├─ Invitation        │   │
         │                   │    │    │ ├─ Review            │   │
         │                   │    │    │ ├─ Report            │   │
         │                   │    │    │ ├─ Payment           │   │
         │                   │    │    │ ├─ Email             │   │
         │                   │    │    │ └─ Notification      │   │
         │                   │    │    │                       │   │
         │ ┌─────────────────┐   │    │ ┌─────────────────────┐   │
         │ │  Databases      │   │    │ │  Repositories       │   │
         │ │  - auth_db      │   │    │ ├─ Dispensary        │   │
         │ │  - users        │   │    │ ├─ Doctor            │   │
         │ └─────────────────┘   │    │ ├─ Patient           │   │
         │                       │    │ ├─ Booking           │   │
         │                       │    │ ├─ Schedule          │   │
         │                       │    │ ├─ Invitation        │   │
         │                       │    │ ├─ Review            │   │
         │                       │    │ ├─ Report            │   │
         │                       │    │ └─ ReportShare       │   │
         │                       │    │                       │   │
         │                       │    │ ┌─────────────────────┐   │
         │                       │    │ │ Actuator/Prometheus │   │
         │                       │    │ │ - /health           │   │
         │                       │    │ │ - /metrics          │   │
         │                       │    │ │ - /info             │   │
         │                       │    │ └─────────────────────┘   │
         └──────────────────────┘    └────────────────────────────┘
                    │                               │
                    │                               │
         ┌──────────▼───────────────────────────────▼──────────┐
         │         DATABASES (PostgreSQL :5432)                │
         │                                                      │
         │  ┌─────────────────────────────────────────────┐   │
         │  │ auth_db                                     │   │
         │  │ └─ users                                    │   │
         │  └─────────────────────────────────────────────┘   │
         │                                                      │
         │  ┌─────────────────────────────────────────────┐   │
         │  │ dispensary_db                               │   │
         │  │ ├─ dispensary                               │   │
         │  │ ├─ doctor                                   │   │
         │  │ ├─ patient                                  │   │
         │  │ ├─ doctor_schedule                          │   │
         │  │ ├─ doctor_invitation                        │   │
         │  │ ├─ booking                                  │   │
         │  │ ├─ report / report_chunk                    │   │
         │  │ ├─ report_share                             │   │
         │  │ ├─ doctor_review                            │   │
         │  │ └─ dispensary_review                        │   │
         │  └─────────────────────────────────────────────┘   │
         └──────────────────────────────────────────────────────┘
                           │
         ┌─────────────────┼─────────────────┬──────────────┐
         │                 │                 │              │
    ┌────▼────┐    ┌──────▼──────┐  ┌──────▼─────┐  ┌─────▼──────┐
    │Braintree │    │  Mailtrap   │  │ Google Maps│  │ Prometheus │
    │ Payment  │    │    SMTP     │  │    API     │  │  Metrics   │
    │ Gateway  │    │             │  │            │  │            │
    └──────────┘    └─────────────┘  └────────────┘  └────────────┘
    (Sandbox Mode)   (smtp.mailtrap. (Geolocation)   (Monitoring)
                     io:2525)
```

### Service Responsibilities

- **Frontend (React + Vite)** - User interface, routing, and client-side state management
- **BFF (Backend for Frontend)** - Authentication, authorization, request routing, JWT validation
- **Main Backend (Spring Boot)** - Business logic, data persistence, payment processing, notifications
- **Databases** - Persistent storage for user, clinic, booking, and transaction data
- **External Services** - Payments (Braintree), Email (Mailtrap), Maps (Google), Monitoring (Prometheus)

### Data Flow

1. **Authentication**: User registers/logs in via Frontend → BFF → Auth DB
2. **Clinic Browse**: Frontend queries Main Backend → Dispensary Service → Data retrieved from dispensary_db
3. **Booking**: Patient creates booking → Booking Service → Payment Service → Braintree → Email notification via Mailtrap
4. **Reports**: Doctor uploads reports → Report Service with AES-GCM encryption → Storage in report_db
5. **Monitoring**: Main Backend exposes metrics → Prometheus scrapes → Dashboards visualize health/performance

---

## 📂 Project Structure

```
DentalCare/
├── Frontend/                      # React + Vite frontend
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── package.json
│   └── vite.config.js
│
├── BFF/                           # Backend for Frontend (Spring Boot)
│   ├── src/
│   │   ├── controller/
│   │   │   └── AuthController.java
│   │   ├── service/
│   │   │   ├── AuthService.java
│   │   │   └── JwtService.java
│   │   ├── config/
│   │   │   └── SecurityConfig.java
│   │   └── filter/
│   │       └── JwtAuthenticationFilter.java
│   ├── pom.xml
│   └── application.yml
│
├── Backend/                       # Main Backend (Spring Boot)
│   ├── src/
│   │   ├── controller/
│   │   │   ├── DispensaryController.java
│   │   │   ├── DoctorController.java
│   │   │   ├── PatientController.java
│   │   │   └── PaymentController.java
│   │   ├── service/
│   │   │   ├── DispensaryService.java
│   │   │   ├── DoctorService.java
│   │   │   ├── PatientService.java
│   │   │   ├── BookingService.java
│   │   │   ├── SlotService.java
│   │   │   ├── InvitationService.java
│   │   │   ├── ReviewService.java
│   │   │   ├── ReportService.java
│   │   │   ├── PaymentService.java
│   │   │   ├── EmailService.java
│   │   │   └── NotificationService.java
│   │   ├── repository/
│   │   ├── entity/
│   │   ├── dto/
│   │   ├── util/
│   │   │   └── CryptoUtil.java
│   │   └── Application.java
│   ├── pom.xml
│   └── application.yml
│
├── .docker/                       # Docker configuration
├── docker-compose.yml
├── README.md
└── LICENSE
```

---

## 🚀 Getting Started

### Prerequisites
- **Node.js** 18+ & npm (Frontend)
- **Java** 21+ & Maven (Backend)
- **Docker** & Docker Compose (Databases & external services)
- **PostgreSQL** (or use Docker)

### Installation

#### 1. Clone Repository
```bash
git clone https://github.com/JClerve/DentalCare.git
cd DentalCare
```

#### 2. Start Databases & External Services
```bash
docker-compose up -d
```
This starts:
- PostgreSQL (port 5432)
- Prometheus (port 9090)
- Mailtrap (mock SMTP)

#### 3. Start BFF Service
```bash
cd BFF
mvn spring-boot:run
```
Runs on: `http://localhost:8005`

#### 4. Start Main Backend Service
```bash
cd Backend
mvn spring-boot:run
```
Runs on: `http://localhost:8080`

#### 5. Start Frontend
```bash
cd Frontend
npm install
npm run dev
```
Runs on: `http://localhost:5173`

---

## 📡 API Documentation

### Authentication Endpoints (BFF - Port 8005)

```
POST /api/auth/register
Content-Type: multipart/form-data
- email: string
- password: string
- name: string
- certificate: file (optional)

POST /api/auth/login
Content-Type: application/json
- email: string
- password: string

Returns: { token, user }
```

### Main Backend Endpoints (Port 8080)

All endpoints require:
- `Authorization: Bearer <JWT_TOKEN>`
- `X-User-Email: <user-email>` (extracted from token)

#### Dispensary
```
GET /api/dispensary - List all dispensaries
GET /api/dispensary/{id} - Get dispensary details
POST /api/dispensary - Create dispensary
PUT /api/dispensary/{id} - Update dispensary
GET /api/dispensary/{id}/doctors - Get doctors in dispensary
```

#### Doctor
```
GET /api/doctor - List doctors
GET /api/doctor/{id} - Get doctor details
GET /api/doctor/{id}/schedule - Get available slots
POST /api/doctor/{id}/schedule - Add schedule
```

#### Booking
```
POST /api/patient/booking - Create booking
GET /api/patient/bookings - Get user bookings
PUT /api/patient/booking/{id}/cancel - Cancel booking
```

#### Payment
```
POST /api/payment/charge - Process payment via Braintree
POST /api/payment/refund - Refund payment
```

---

## 🔧 Development

### Frontend Development
```bash
cd Frontend
npm run dev          # Start dev server with HMR
npm run build        # Build for production
npm run lint         # Run ESLint
npm run preview      # Preview production build
```

### Backend Development
```bash
cd Backend
mvn clean install    # Build project
mvn test            # Run tests
mvn spring-boot:run # Run application
```

### Database Migrations
PostgreSQL migrations are managed via application startup. Ensure `spring.jpa.hibernate.ddl-auto=update` is set.

---

## ✅ Testing

### Frontend Tests
```bash
cd Frontend
npm run test         # Run unit tests
npm run test:e2e    # Run end-to-end tests
```

### Backend Tests
```bash
cd Backend
mvn test            # Run JUnit tests
mvn verify          # Run integration tests with Testcontainers
```

### Coverage
```bash
mvn clean test jacoco:report   # Generate coverage report
```

---

## 🐳 Deployment

### Docker Build
```bash
docker build -f Frontend.Dockerfile -t dentalcare-frontend .
docker build -f BFF.Dockerfile -t dentalcare-bff .
docker build -f Backend.Dockerfile -t dentalcare-backend .
```

### Docker Compose (Development)
```bash
docker-compose up
```

### Production Deployment
1. Build and push images to registry
2. Update Kubernetes manifests or deployment configs
3. Deploy with environment-specific configurations
4. Monitor via Prometheus & alerting

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Code Standards
- **Frontend**: Follow ESLint rules, use Prettier for formatting
- **Backend**: Use Google Java Style Guide, add unit tests for new features
- **Commits**: Use conventional commits (`feat:`, `fix:`, `docs:`, etc.)

---

## 📝 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

---

## 👥 Team & Support

For issues, feature requests, or contributions, please:
- 📧 Open an issue on GitHub
- 💬 Start a discussion for questions
- 📢 Join our community

---

**Last Updated:** June 2026  
**Version:** 0.0.0
