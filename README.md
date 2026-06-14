# Untitled Project

A production-ready REST API built with FastAPI and PostgreSQL, featuring JWT-based authentication and Docker containerization.

## Business Problem Solved

This project provides a scalable, secure, and maintainable backend API foundation that solves common enterprise requirements:

- **Secure Authentication**: JWT-based authentication with bcrypt password hashing ensures user credentials are protected with industry-standard security practices.
- **Database Reliability**: PostgreSQL with SQLAlchemy ORM and Alembic migrations provides a robust data layer with version-controlled schema changes.
- **Production Deployment Ready**: Docker and Docker Compose configuration enables seamless deployment to any containerized environment.
- **Clean Architecture**: Separation of concerns between routes, models, services, and schemas makes the codebase maintainable and testable.
- **Developer Experience**: Comprehensive logging, error handling, and CORS middleware simplify debugging and frontend integration.

## Tech Stack

| Component | Technology |
|-----------|------------|
| Framework | FastAPI 0.109.x |
| Language | Python 3.11+ |
| Database | PostgreSQL 15+ |
| ORM | SQLAlchemy 2.x |
| Migrations | Alembic |
| Authentication | JWT (python-jose) + bcrypt (passlib) |
| Container | Docker + Docker Compose |
| Testing | pytest + pytest-asyncio |

## Getting Started

### Prerequisites

- Python 3.11 or higher
- PostgreSQL 15+ (or Docker)
- Docker & Docker Compose (for containerized setup)

### Local Development Setup

1. **Clone the repository**
   