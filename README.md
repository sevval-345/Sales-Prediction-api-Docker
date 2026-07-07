# 📊 Sales Prediction API - Docker

A microservices-based sales prediction system built with Python, featuring machine learning predictions, web scraping capabilities, and a web dashboard. The entire application is containerized using Docker and Docker Compose for easy deployment.

## 🎯 Project Overview

This project implements a complete sales prediction pipeline with multiple services:

- **ML Service**: Machine learning model for sales predictions
- **Scraper Service**: Web scraping for data collection
- **Web Dashboard**: Flask-based web interface for visualization and analysis
- **Database**: PostgreSQL for persistent data storage
- **Cache**: Redis for caching and message queuing

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────┐
│         Web Dashboard (Flask)                   │
│         Port: 5000                              │
└──────────────┬──────────────────────────────────┘
               │
    ┌──────────┼──────────┐
    │          │          │
┌───▼───┐  ┌──▼───┐  ┌───▼────┐
│  ML   │  │Redis │  │Database│
│Service│  │Cache │  │(PG)    │
└───────┘  └──────┘  └────────┘
    │
┌───▼──────┐
│ Scraper  │
└──────────┘
```

## 📋 Services

### 1. **ML Service**
Machine learning service for sales prediction
- Depends on: Redis, PostgreSQL
- Handles prediction requests
- Caches results in Redis

### 2. **Scraper Service**
Web scraping module for data collection
- Depends on: Redis
- Collects data from web sources
- Stores data in cache and database

### 3. **Web Dashboard**
Flask-based web application for user interaction
- Port: **5000**
- Depends on: PostgreSQL
- Provides visualization and analysis interface
- User-friendly interface for predictions

### 4. **Database (PostgreSQL)**
Persistent data storage
- Image: `postgres:15`
- Database: `pricedb`
- Initialized with: `./db/init.sql`
- Persistent volume: `postgres_data`

### 5. **Redis Cache**
In-memory data store and message broker
- Image: `redis:alpine`
- Used for caching and service communication

## 🚀 Quick Start

### Prerequisites
- Docker
- Docker Compose

### Installation & Setup

1. **Clone the repository**
```bash
git clone https://github.com/sevval-345/Sales-Prediction-api-Docker.git
cd Sales-Prediction-api-Docker
```

2. **Build and start all services**
```bash
docker-compose up -d
```

3. **Check service status**
```bash
docker-compose ps
```

4. **Access the web dashboard**
Open your browser and navigate to:
```
http://localhost:5000
```

### Stopping Services

```bash
docker-compose down
```

To remove volumes as well:
```bash
docker-compose down -v
```

## 📁 Project Structure

```
Sales-Prediction-api-Docker/
├── docker-compose.yml          # Compose configuration for all services
├── requirements.txt            # Root dependencies
│
├── ml-service/                 # Machine Learning Service
│   ├── Dockerfile
│   └── requirements.txt
│
├── scraper/                    # Web Scraping Service
│   ├── Dockerfile
│   └── requirements.txt
│
├── web-dashboard/              # Flask Web Dashboard
│   ├── Dockerfile
│   ├── app.py
│   └── requirements.txt
│
└── db/                         # Database Configuration
    └── init.sql               # PostgreSQL initialization script
```

## 🔧 Configuration

### Environment Variables
Database configuration in `docker-compose.yml`:
- `POSTGRES_DB`: `pricedb`
- `POSTGRES_PASSWORD`: `password`

You can modify these variables in the compose file for production use.

### Database Initialization
Place your SQL initialization scripts in `./db/init.sql`. This file is automatically executed when PostgreSQL starts.

## 📦 Dependencies

### Core Requirements
- `redis` - Redis Python client for caching
- `requests` - HTTP library for API calls and scraping
- `psycopg2-binary` - PostgreSQL adapter for Python

### Services
- PostgreSQL 15
- Redis (Alpine)
- Python 3.x

## 🐳 Docker Commands

### View Logs
```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f web-dashboard
docker-compose logs -f ml-service
docker-compose logs -f scraper
```

### Rebuild Services
```bash
docker-compose build
docker-compose up -d
```

### Execute Commands in Container
```bash
# Web dashboard
docker-compose exec web-dashboard python app.py

# ML service
docker-compose exec ml-service python app.py

# Database
docker-compose exec db psql -U postgres -d pricedb
```

## 💻 Development

### Local Development Setup (Optional)
If you want to run services locally without Docker:

1. Install Python 3.8+
2. Install PostgreSQL 15
3. Install Redis
4. Create virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```
5. Install dependencies:
```bash
pip install -r requirements.txt
```

## 🔐 Security Considerations

⚠️ **Production Use**: 
- Change default PostgreSQL password
- Use environment variables for sensitive data
- Implement proper authentication in Flask
- Use Docker secrets for sensitive configurations
- Consider using `.env` files (excluded from version control)

## 📝 API & Usage

Detailed usage examples and API endpoints should be documented in each service directory:
- `ml-service/README.md` - ML predictions API
- `scraper/README.md` - Scraper configuration
- `web-dashboard/README.md` - Dashboard features

## 🐛 Troubleshooting

### Port 5000 Already in Use
Change the port mapping in `docker-compose.yml`:
```yaml
web-dashboard:
  ports:
    - "8000:5000"  # Access at localhost:8000
```

### Database Connection Issues
Verify PostgreSQL is running:
```bash
docker-compose logs db
```

### Redis Connection Issues
Check Redis service:
```bash
docker-compose logs redis
```

### Services Not Starting
Rebuild images:
```bash
docker-compose build --no-cache
docker-compose up -d
```

## 📊 Monitoring & Maintenance

### Check Database Size
```bash
docker-compose exec db psql -U postgres -d pricedb -c "SELECT pg_size_pretty(pg_database_size('pricedb'));"
```

### Backup Database
```bash
docker-compose exec db pg_dump -U postgres -d pricedb > backup.sql
```

### Restore Database
```bash
docker-compose exec -T db psql -U postgres -d pricedb < backup.sql
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request


**Last Updated**: July 2026

Happy Predicting! 🚀📈
