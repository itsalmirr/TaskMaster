# TaskMaster

A to-do REST API written to learn reactive Spring: Spring WebFlux functional endpoints over R2DBC and PostgreSQL.

**Stack:** Java 17 · Spring Boot 3.0 · Spring WebFlux · Spring Data R2DBC · PostgreSQL · Lombok

## API

| Method | Path | Description |
|---|---|---|
| GET | `/api/v1/tasks` | List tasks |
| POST | `/api/v1/tasks` | Create a task |
| GET | `/api/v1/tasks/{id}` | Get one task |
| PATCH | `/api/v1/tasks/{id}` | Update a task |
| DELETE | `/api/v1/tasks/{id}` | Delete a task |

The routes are a `RouterFunction` in `RestfulSpringApplication`, the handlers are in `TaskController`, and `GlobalExceptionHandler` maps a missing task to 404 and bad input to 400. Creation and update times are filled in by R2DBC auditing.

Request body for `POST` and `PATCH`:

```json
{
  "title": "Task title",
  "taskDescription": "Task description",
  "completed": false
}
```

## Run locally

You need JDK 17 and a PostgreSQL database with a `tasks` table matching `models/Task.java`. Point R2DBC at it, for example in `src/main/resources/application.properties`:

```properties
spring.r2dbc.url=r2dbc:postgresql://localhost:5432/taskmaster
spring.r2dbc.username=postgres
spring.r2dbc.password=postgres
```

Then:

```sh
./mvnw spring-boot:run   # http://localhost:8080
./mvnw test
```

Spring Security is on the classpath with its default configuration, so requests need the generated password that Spring Boot prints at startup (user `user`).
