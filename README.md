# 🎓 University Student Management System (SMS)

This project is a RESTful API system for managing a university ecosystem (including students, courses, instructors, and enrollments). The application is built following a clean multi-layered architecture (Controller -> Service -> Repository) using a modern Java development stack.

## Tech Stack

- Language: Java 17
- Framework: Spring Boot 3.3.2
- Database: PostgreSQL 16 (running inside a Docker container)
- Build System: Gradle
- API Documentation: Springdoc-OpenAPI v2 (Swagger UI)
- Libraries: Lombok, Jackson
- Testing: JUnit 5, Mockito, MockMvc

## Local Setup and Installation

### 1. Clone the Repository

```sql
git clone https://github.com/TaranenkoKen/java-final-project.git
```

### 2. Run Main Database in Docker
Make sure your Docker Desktop is running, then execute the following command in your terminal to start the PostgreSQL instance:

```bash
docker compose up -d
```

### 3. Run the Spring Boot Application
Execute the Gradle command to start your local server:

```
./gradlew bootRun
```

Alternatively, you can open the project in IntelliJ IDEA, locate the SmsApplication.java file, and click the green Run button.

### 4. Open Swagger UI

```
http://localhost:8080/swagger-ui.html
```

## Running Tests

```bash
mvn ./gradlew test
```

## API Overview

### Students `/api/v1/students`

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/` | Create student |
| GET | `/` | List all (filter: `?status=ACTIVE`, `?year=2`) |
| GET | `/{id}` | Get by ID |
| PUT | `/{id}` | Update |
| DELETE | `/{id}` | Delete |
| GET | `/search?name=` | Search by name |
| GET | `/search?email=` | Search by email |
| GET | `/top?n=10` | Top-N students by GPA |

### Teachers `/api/v1/teachers`

Full CRUD (POST/GET/PUT/DELETE).

### Courses `/api/v1/courses`

Full CRUD + filter by `?teacherId=` and `?credits=`.

### Enrollments `/api/v1/enrollments`

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/` | Enroll student in course |
| GET | `/{id}` | Get enrollment |
| GET | `/student/{studentId}` | Enrollments for student |
| GET | `/course/{courseId}` | Enrollments for course |
| PATCH | `/{id}/grade` | Set grade |
| PATCH | `/{id}/paid` | Mark as paid |
| GET | `/unpaid` | All unpaid |
| GET | `/unpaid/student/{id}` | Unpaid for student |
| GET | `/reports/gpa-by-course` | Average GPA per course |
| GET | `/reports/gpa-by-semester?from=&to=` | Average GPA in date range |

## Example cURL

```bash
# Create teacher
curl -X POST http://localhost:8080/api/v1/teachers \
  -H 'Content-Type: application/json' \
  -d '{"firstName":"Ivan","lastName":"Petrenko","email":"ivan@uni.ua","department":"Computer Science"}'

# Create student
curl -X POST http://localhost:8080/api/v1/students \
  -H 'Content-Type: application/json' \
  -d '{"firstName":"Olena","lastName":"Kovalenko","email":"olena@uni.ua","enrollmentDate":"2023-09-01","status":"ACTIVE","year":1}'

# Enroll student (use IDs from previous responses)
curl -X POST http://localhost:8080/api/v1/enrollments \
  -H 'Content-Type: application/json' \
  -d '{"studentId":1,"courseId":1}'

# Set grade
curl -X PATCH http://localhost:8080/api/v1/enrollments/1/grade \
  -H 'Content-Type: application/json' \
  -d '{"grade":92.5}'

# Top 5 students by GPA
curl http://localhost:8080/api/v1/students/top?n=5
```

## Project Structure

```
src/main/java/ua/university/sms/
├── controller/    REST endpoints
├── service/       Business logic (interfaces + implementations)
├── repository/    Spring Data JPA interfaces
├── model/
│   ├── entity/    JPA entities
│   └── dto/       Request/Response records
├── mapper/        Entity ↔ DTO conversion
└── exception/     GlobalExceptionHandler + custom exceptions
```