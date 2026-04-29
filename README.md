# Student Service

A Spring Boot REST API handling student registration, authentication, course management, and graduation eligibility checks. Part of a three-service microservices architecture built for the Software Engineering for Service Computing module at Leeds Beckett University.

The service acts as the core backend for a student portal, integrating with a Finance Service to validate invoice status before allowing graduation, and with a Library Service to handle fine creation when books are returned late. A basic HTML/JS frontend is bundled and served at `/` for quick testing without a separate client.

## Features

- Student registration and login (email + password)
- Course listing and enrolment tracking
- Student profile read and update
- Graduation eligibility check — validates no outstanding invoices via the Finance Service
- Library fine endpoint — accepts fine creation requests from the Library Service
- Student verification endpoint — used by the Library Service to confirm a student exists

## Tech stack

- Java 17
- Spring Boot 3.4
- Maven
- H2 (file-based, persists between restarts)
- Spring Security
- Spring Data JPA

## Getting started

No database setup needed — H2 runs embedded and persists to a local file.

```bash
git clone https://github.com/nandakishore-78/student-service.git
cd student-service
mvn spring-boot:run
```

The service starts on port `8080`. An H2 console is available at `/h2-console` during development (JDBC URL: `jdbc:h2:file:./data/studentdb`).

## API endpoints

### Student

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/student/register` | Register a new student |
| POST | `/student/login` | Login with email and password |
| GET | `/student/{id}` | Get student profile |
| PUT | `/student/{id}` | Update name and surname |
| DELETE | `/student/delete/{id}` | Delete a student |
| GET | `/student/graduate/{id}` | Check graduation eligibility |

### Courses

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/student/courses` | List all available courses |

### Inter-service endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/student/verify` | Confirm a student exists (called by Library Service) |
| POST | `/student/fines` | Create a library fine for a student |

## Configuration

The service uses H2 file-based storage by default — no config changes needed for local development. To switch to a production database, update `application.properties`:

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/studentdb
spring.datasource.driver-class-name=org.postgresql.Driver
spring.datasource.username=<your_user>
spring.datasource.password=<your_password>
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
```

For integration with the Finance Service, make sure it is running on port `8081` before testing graduation eligibility.

## Architecture

This is one of three services in the system:

```
Student Service (this)  ──►  Finance Service  (graduation check, fine invoices)
Library Service         ──►  Student Service  (student verification, fine creation)
```

## Module

Software Engineering for Service Computing — Leeds Beckett University
