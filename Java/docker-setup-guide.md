# Docker Setup Guide for Java Microservices

## Install Docker Desktop

1. Download: https://www.docker.com/products/docker-desktop
2. Install and restart your computer
3. Verify: Open Docker Desktop (should show "Running")

## Docker Compose Setup (Recommended)

Once Docker is installed, we'll use Docker Compose to run all services locally:

### Services You'll Need

1. **PostgreSQL** - Main database
2. **Redis** - Caching layer
3. **MySQL** (optional) - If you want to try both databases
4. **Kafka** (optional, for Week 9) - Message broker

### Quick Test Commands

After Docker Desktop is running:

```bash
# Test Docker is working
docker --version

# Test Docker Compose
docker-compose --version

# Pull and test PostgreSQL
docker run --name test-postgres -e POSTGRES_PASSWORD=testpass -d postgres:15
docker stop test-postgres
docker rm test-postgres

# If all works, Docker is ready!
```

## Next Steps After Docker Installation

We'll create a `docker-compose.yml` file in Week 5 when you start your project.

### Template Structure (Preview)

```yaml
version: '3.8'
services:
  postgres:
    image: postgres:15
    environment:
      POSTGRES_DB: ecommerce_db
      POSTGRES_USER: app_user
      POSTGRES_PASSWORD: app_password
    ports:
      - "5432:5432"
  
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
```

---

## Current Status

- ✅ Java 17
- ✅ Maven
- ✅ IntelliJ IDEA
- ✅ Git
- ✅ Postman
- ⏳ Docker Desktop (install now)

**After Docker install:** Update `phase/Phase-0-Actions.md` and continue to Week 1!

---

## Why Docker?

- **Consistent environment** across development/staging/production
- **Easy database setup** - no manual installation needed
- **Microservices practice** - each service in its own container
- **Portfolio showcase** - Docker knowledge is valuable
- **Industry standard** - used everywhere

