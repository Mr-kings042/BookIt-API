# BookIt API

![Python](https://img.shields.io/badge/Python-3.13-blue.svg)
![FastAPI](https://img.shields.io/badge/FastAPI-0.104-green.svg)
![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-2.0-red.svg)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Latest-blue.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)

A comprehensive REST API for a modern booking platform called **BookIt**. This API enables users to browse services, make bookings, leave reviews, and provides full administrative capabilities for managing the platform.

## 🚀 Features

### Core Functionality
- **User Management**: Registration, authentication, profile management
- **Service Management**: Browse, search, and manage service offerings
- **Booking System**: Complete booking lifecycle with status management
- **Review System**: Rate and review services after booking completion
- **Admin Panel**: Full administrative control over users, services, and bookings

### Key Highlights
- **JWT Authentication**: Secure token-based authentication
- **Role-Based Access Control**: User and Admin roles with appropriate permissions
- **Database Migrations**: Alembic integration for schema versioning
- **Comprehensive Testing**: Full test coverage with pytest
- **Professional Logging**: Structured logging throughout the application
- **API Documentation**: Auto-generated OpenAPI/Swagger documentation
- **Data Validation**: Robust input validation with Pydantic schemas

## 🏗️ Architecture

### Tech Stack
- **Framework**: FastAPI (Python 3.13)
- **Database**: PostgreSQL (SQLite for testing)
- **ORM**: SQLAlchemy 2.0
- **Authentication**: JWT tokens with bcrypt password hashing
- **Testing**: pytest with test fixtures and factories
- **Migration**: Alembic
- **Documentation**: OpenAPI/Swagger UI

### Project Structure
```
BookIt-API/
├── alembic/                 # Database migrations
├── crud/                    # CRUD operations
│   ├── booking.py
│   ├── review.py
│   ├── service.py
│   └── user.py
├── database/                # Database configuration
├── middleware/              # Custom middleware
├── models/                  # SQLAlchemy models
│   ├── booking.py
│   ├── review.py
│   ├── service.py
│   ├── token_blacklist.py
│   └── user.py
├── routers/                 # API route handlers
│   ├── booking.py
│   ├── review.py
│   ├── service.py
│   └── user.py
├── schemas/                 # Pydantic schemas
├── security/                # Authentication & authorization
├── services/                # Business logic services
├── tests/                   # Comprehensive test suite
└── main.py                  # Application entry point
```

## 📋 Prerequisites

- Python 3.13+
- PostgreSQL (for production)
- pip or pipenv for dependency management

## ⚡ Quick Start

### 1. Clone the Repository
```bash
git clone <repository-url>
cd BookIt-API
```

### 2. Set Up Virtual Environment
```bash
# Create virtual environment
python -m venv menv

# Activate virtual environment
# On Windows:
menv\Scripts\activate
# On macOS/Linux:
source menv/bin/activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Configure Environment
```bash
# Copy example configuration
cp config.example.py config.py

# Edit config.py with your database settings
```

### 5. Set Up Database
```bash
# Run migrations
alembic upgrade head
```

### 6. Run the Application
```bash
# Development server
uvicorn main:app --reload --host 0.0.0.0 --port 8000

# Production server
uvicorn main:app --host 0.0.0.0 --port 8000
```

### 7. Access the API
- **API Base URL**: `http://localhost:8000`
- **Interactive Docs**: `http://localhost:8000/docs`
- **ReDoc**: `http://localhost:8000/redoc`

## 🔧 Configuration

Create a `config.py` file in the project root:

```python
import os
from typing import Optional

class Settings:
    # Database
    DATABASE_URL: str = os.getenv("DATABASE_URL", "postgresql://user:password@localhost/bookit")
    TEST_DATABASE_URL: str = "sqlite:///./test.db"
    
    # Security
    SECRET_KEY: str = os.getenv("SECRET_KEY", "your-secret-key-here")
    ACCESS_TOKEN_EXPIRE_MINUTES: int = 30
    
    # Application
    API_V1_STR: str = "/api"
    PROJECT_NAME: str = "BookIt API"
    
    class Config:
        case_sensitive = True

settings = Settings()
```

## 📚 API Documentation

### Authentication
All protected endpoints require a JWT token in the Authorization header:
```
Authorization: Bearer <your-jwt-token>
```

### Core Endpoints

#### Authentication & Users
- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login
- `POST /api/auth/logout` - User logout
- `GET /api/users/me` - Get current user profile
- `PATCH /api/users/me` - Update user profile
- `GET /api/admin/users` - List all users (Admin only)

#### Services
- `GET /api/services` - List all active services
- `GET /api/services/{service_id}` - Get service details
- `POST /api/services` - Create new service (Admin only)
- `PATCH /api/services/{service_id}` - Update service (Admin only)
- `DELETE /api/services/{service_id}` - Delete service (Admin only)

#### Bookings
- `POST /api/bookings` - Create new booking
- `GET /api/bookings` - Get user's bookings
- `GET /api/bookings/{booking_id}` - Get booking details
- `PATCH /api/bookings/{booking_id}` - Update booking
- `DELETE /api/bookings/{booking_id}` - Cancel booking
- `GET /api/admin/bookings` - List all bookings (Admin only)

#### Reviews
- `POST /api/reviews` - Create review (for completed bookings)
- `GET /api/services/{service_id}/reviews` - Get service reviews
- `GET /api/reviews/{review_id}` - Get review details
- `PATCH /api/reviews/{review_id}` - Update review
- `DELETE /api/reviews/{review_id}` - Delete review
- `GET /api/services/{service_id}/reviews/stats` - Get review statistics

### Data Models

#### User
```json
{
  "id": "uuid",
  "name": "John Doe",
  "email": "john@example.com",
  "role": "user|admin",
  "is_active": true,
  "status": "active",
  "created_at": "2025-09-27T00:00:00Z"
}
```

#### Service
```json
{
  "id": "uuid",
  "title": "House Cleaning",
  "description": "Professional house cleaning service",
  "price": 99.99,
  "duration_minutes": 120,
  "is_active": true,
  "owner_id": "uuid",
  "created_at": "2025-09-27T00:00:00Z"
}
```

#### Booking
```json
{
  "id": "uuid",
  "user_id": "uuid",
  "service_id": "uuid",
  "start_time": "2025-09-28T10:00:00Z",
  "end_time": "2025-09-28T12:00:00Z",
  "status": "pending|confirmed|cancelled|completed",
  "created_at": "2025-09-27T00:00:00Z"
}
```

#### Review
```json
{
  "id": "uuid",
  "booking_id": "uuid",
  "rating": 5,
  "comment": "Excellent service!",
  "created_at": "2025-09-27T00:00:00Z"
}
```

## 🧪 Testing

### Run Tests
```bash
# Run all tests
pytest

# Run specific test file
pytest tests/user_test.py

# Run with coverage
pytest --cov=. --cov-report=html

# Run tests in verbose mode
pytest -v
```

### Test Coverage
The project includes comprehensive tests for:
- User authentication and management
- Service CRUD operations
- Booking lifecycle management
- Review system functionality
- Admin operations
- Edge cases and error handling

## 🔒 Security Features

### Authentication & Authorization
- **JWT Tokens**: Secure, stateless authentication
- **Password Hashing**: bcrypt for secure password storage
- **Token Blacklisting**: Logout functionality with token invalidation
- **Role-Based Access**: User and Admin roles with appropriate permissions

### Data Validation
- **Input Validation**: Pydantic schemas validate all input data
- **SQL Injection Protection**: SQLAlchemy ORM prevents SQL injection
- **UUID Usage**: UUIDs for all primary keys enhance security

### Business Logic Security
- **Booking Ownership**: Users can only access their own bookings
- **Review Authorization**: Only booking creators can review services
- **Time Validation**: Bookings must be in the future
- **Status Validation**: Proper booking status transitions

## 🚀 Deployment

### Environment Variables
```bash
export DATABASE_URL="postgresql://user:password@localhost/bookit"
export SECRET_KEY="your-secure-secret-key"
export ACCESS_TOKEN_EXPIRE_MINUTES=30
```

### Docker Deployment (Optional)
```dockerfile
FROM python:3.13-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .
EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### Production Considerations
- Use PostgreSQL for production database
- Set up proper environment variables
- Configure proper logging levels
- Use HTTPS in production
- Set up monitoring and health checks
- Configure CORS for frontend integration

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines
- Follow PEP 8 style guidelines
- Write tests for new features
- Update documentation for API changes
- Use meaningful commit messages

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👥 Authors

- **Your Name** - *Initial work* - [Your GitHub](https://github.com/yourusername)

## 🙏 Acknowledgments

- FastAPI community for the excellent framework
- SQLAlchemy team for the powerful ORM
- Pydantic for robust data validation
- The Python community for amazing libraries

## 📞 Support

For support and questions:
- Create an issue on GitHub
- Email: your-email@example.com
- Documentation: `http://localhost:8000/docs`

---

**BookIt API** - Professional booking platform REST API built with FastAPI, SQLAlchemy, and PostgreSQL.