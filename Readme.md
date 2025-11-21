# Volunteer Resource Management System (VRMS)

![VRMS Logo](https://img.shields.io/badge/VRMS-Volunteer%20Management-blue?style=for-the-badge)

## Overview

The Volunteer Resource Management System (VRMS) is a comprehensive microservices-based platform designed to seamlessly connect volunteers, NGOs, corporates, and administrators. Our platform enables users to register, browse volunteer opportunities, manage postings, track volunteer activities, and access detailed analytics.

Each microservice operates independently and communicates through secure REST APIs using JWT authentication, ensuring scalability, maintainability, and security.

## Architecture

VRMS follows a microservices architecture pattern with the following core services:

- **User Service** - Authentication, registration, identity & role management
- **NGO Posting Service** - Manage NGO postings, volunteer slots, registrations
- **Volunteer Service** - Volunteer preferences, skills, activity tracking
- **Matching Service** - Intelligent recommendation engine
- **Analytics Service** - Aggregated admin insights
- **Frontend** - React-based web interface for all roles

## Services Overview

### 1. User Service
**Central Authentication & Identity Management**

The User Service manages user identity, authentication, and role-based access control, serving as the central authentication service in VRMS.

#### Responsibilities
- User registration (Volunteer, NGO, Corporate, Admin)
- JWT-based login and token refreshing
- Profile management
- Role-based access control (RBAC)

#### Key API Endpoints

**Registration**
- `POST /api/v1/users/register/volunteer` - Register a new volunteer
- `POST /api/v1/users/register/ngo` - Register a new NGO
- `POST /api/v1/users/register/corporate` - Register a corporate user

**Authentication**
- `POST /api/v1/users/login` - Authenticate user and return JWT tokens
- `POST /api/v1/users/refresh-token` - Refresh the access token

**Profile Management**
- `GET /api/v1/users/volunteers/{id}` - Get volunteer profile
- `GET /api/v1/users/ngos/{id}` - Get NGO profile
- `GET /api/v1/users` (Admin) - Get all users

### 2. NGO Posting Service
**Volunteer Opportunity Management**

The NGO Posting Service manages volunteer opportunities created by NGOs and handles volunteer registrations for these postings.

#### Responsibilities
- Posting creation, update, deletion
- Volunteer registration & slot tracking
- Location, domain, and status-based filtering
- Posting lifecycle: Active, Closed, Draft, Archived

#### Key API Endpoints

**Postings**
- `POST /api/v1/postings` - Create a new posting
- `GET /api/v1/postings` - Fetch all postings with filtering (domain, city, state, status)
- `GET /api/v1/postings/{id}` - Get posting details
- `PUT /api/v1/postings/{id}` - Update posting
- `DELETE /api/v1/postings/{id}` - Delete posting

**Volunteer Registration**
- `POST /api/v1/postings/{postingId}/register/{volunteerId}` - Register a volunteer for a posting

### 3. Volunteer Service
**Extended Volunteer Information Management**

The Volunteer Service stores comprehensive volunteer information used in analytics and matching algorithms.

#### Responsibilities
- Manage volunteer details (skills, interests, languages)
- Store activity history
- Track availability and volunteer engagement

#### Key API Endpoints
- `POST /api/v1/volunteers` - Create volunteer profile
- `GET /api/v1/volunteers/{userId}` - Fetch volunteer profile
- `PUT /api/v1/volunteers/{userId}` - Update volunteer preferences, availability, skills
- `GET /api/v1/volunteers/{userId}/activity` - Fetch volunteer's activity log

### 4. Matching Service
**Intelligent Recommendation Engine**

The Matching Service provides personalized volunteer opportunity recommendations based on skills, preferences, domain, location, and availability.

#### Responsibilities
- Fetch volunteer preference data
- Fetch posting metadata
- Compute similarity/matching score
- Exclude postings already registered by the volunteer
- Provide ranked recommendations

#### Key API Endpoints
- `GET /api/v1/matching/recommendations/{volunteerId}` - Get personalized recommendations for volunteers

### 5. Analytics Service
**Data Insights & Reporting**

The Analytics Service aggregates data from all microservices to provide comprehensive insights for Admin users.

#### Responsibilities
- User role distribution analysis
- Posting-level insights
- Volunteer participation statistics
- Cross-service data aggregation

#### Key API Endpoints
- `GET /api/v1/admin/analytics/user-insights` - Returns total users grouped by roles
- `GET /api/v1/admin/analytics/posting/{postingId}/volunteers` - Details of volunteers registered for a specific posting

### 6. Frontend (React)
**User Interface & Experience**

The React-based frontend provides an intuitive, responsive interface for all user roles with real-time interactions.

#### Features
- User login & registration
- Volunteer dashboard
- NGO posting management
- Browse & apply for opportunities
- Admin dashboard for analytics
- Real-time interactions and feedback

## Technology Stack

### Backend Services
- **Java/Spring Boot** - Microservices framework
- **JWT** - Authentication & authorization
- **MySQL** - Primary database
- **Docker** - Containerization
- **Kubernetes** - Orchestration

### Frontend
- **React** - Frontend framework
- **Vite** - Build tool
- **Tailwind CSS** - Styling
- **Axios** - HTTP client

### DevOps & Monitoring
- **Docker Compose** - Local development
- **Kubernetes** - Production deployment
- **Prometheus/Grafana** - Monitoring (if configured)

## Project Structure

```
VRMS/
├── User-Service/                 # Authentication & user management
├── NGO-Postings-Service/        # Posting management
├── Volunteer-Service/           # Volunteer profiles & tracking
├── Matching-Service/            # Recommendation engine
├── analytics-service/           # Analytics & reporting
├── VRMS_Frontend/              # React frontend application
└── README.md                   # This file
```

## Getting Started

### Prerequisites
- Java 11+
- Node.js 16+
- Docker & Docker Compose
- MySQL 8.0+

### Quick Start with Docker Compose

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd APIBP-20242YB-Team-06
   ```

2. **Start all services**
   ```bash
   docker-compose up -d
   ```

3. **Access the application**
   - Frontend: http://localhost:3000
   - User Service: http://localhost:8081
   - NGO Posting Service: http://localhost:8082
   - Other services on their respective ports

### Manual Setup

#### Backend Services
```bash
# Start each service individually
cd User-Service
mvn spring-boot:run

cd ../NGO-Postings-Service
mvn spring-boot:run

# Repeat for other services
```

#### Frontend
```bash
cd VRMS_Frontend/vrms-frontend
npm install
npm run dev
```

## Authentication Flow

1. **User Registration** - Users register through the User Service
2. **Login** - Authenticate and receive JWT tokens
3. **Token Usage** - Include JWT in Authorization header for API calls
4. **Token Refresh** - Use refresh token to get new access tokens

## 👥 User Roles & Permissions

| Role | Permissions |
|------|-------------|
| **Volunteer** | Browse opportunities, register for postings, view recommendations |
| **NGO** | Create/manage postings, view volunteer registrations |
| **Corporate** | Sponsor events, view corporate analytics |
| **Admin** | Full system access, analytics dashboard, user management |

## 📊 API Documentation

Each service provides OpenAPI documentation accessible at:
- User Service: http://localhost:8081/swagger-ui.html
- NGO Posting Service: http://localhost:8082/swagger-ui.html
- Additional services on their respective ports

## Docker Support

Each service includes:
- `Dockerfile` for containerization
- `docker-compose.yml` for local development
- Kubernetes deployment files

## Monitoring & Logging

- **Structured Logging** - Using Logback for consistent log formatting
- **Health Checks** - Spring Boot Actuator endpoints
- **Metrics** - Application metrics for monitoring

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request


## Support

For questions and support, please:
- Open an issue in the repository
- Contact the development team
- Check the documentation in each service's README

## Roadmap

- [ ] Enhanced matching algorithms
- [ ] Mobile application
- [ ] Real-time notifications
- [ ] Advanced analytics dashboard
- [ ] Integration with external volunteer platforms

---

**Built with ❤️ by the VRMS Development Team**
