# Development Guide

## Prerequisites
- Java [TODO: version] (recommend SDKMAN for version management)
- [TODO: Maven / Gradle]
- Docker + Docker Compose
- [TODO: any other tools]

## Local Setup

### 1. Clone the repository
```bash
git clone [TODO: repo URL]
cd [TODO: project-dir]
```

### 2. Start infrastructure
```bash
# Start database, message broker, etc.
docker compose up -d
```

### 3. Configure environment
```bash
# Copy example environment file
cp .env.example .env
# Edit .env with your local settings
```

Key environment variables:
| Variable          | Default              | Description              |
|-------------------|----------------------|--------------------------|
| `DATABASE_URL`    | [TODO]               | PostgreSQL connection    |
| `DATABASE_USER`   | [TODO]               | DB username              |
| `DATABASE_PASS`   | [TODO]               | DB password              |
| [TODO]            | [TODO]               | [TODO]                   |

### 4. Run database migrations
```bash
[TODO: mvn flyway:migrate / ./gradlew flywayMigrate]
```

### 5. Start the application
```bash
[TODO: mvn spring-boot:run / ./gradlew quarkusDev]
```

Application is available at: `http://localhost:[TODO: port]`

## Build

```bash
# Full build with tests
[TODO: mvn clean verify / ./gradlew build]

# Build without tests (fast)
[TODO: mvn clean package -DskipTests / ./gradlew build -x test]
```

## Test

```bash
# Unit tests only
[TODO: mvn test / ./gradlew test]

# Integration tests
[TODO: mvn verify / ./gradlew integrationTest]

# Single test class
[TODO: mvn test -Dtest=MyClassTest / ./gradlew test --tests MyClassTest]
```

## Common Tasks

### Generate a new database migration
```bash
[TODO: describe how to create a new migration file]
```

### Update dependencies
```bash
[TODO: mvn versions:display-dependency-updates / ./gradlew dependencyUpdates]
```

### Docker build
```bash
[TODO: docker build command or Jib/Buildpacks]
```

## Troubleshooting

### Port already in use
```bash
lsof -i :[PORT] | grep LISTEN
kill -9 [PID]
```

### Database connection refused
- Ensure Docker is running: `docker compose ps`
- Check DB logs: `docker compose logs db`

### [TODO: Add project-specific issues as they come up]
