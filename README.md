# HinkleSystem

Hinkle is the appointment and reporting application for Lave-Auto Kimber. The starter serves a web entry page and packages it with the Spring Boot backend in one executable JAR. Application code lives under `src/main/java/org/sltech/hinkle`, web assets under `src/main/resources/static`, and tests under `src/test/java`.

## Build and test

Install JDK 21. From the repository root, run `.\gradlew.bat clean build` on Windows or `./gradlew clean build` on macOS and Linux. The build runs the web and database smoke tests and produces `build/libs/hinkle.jar`. Local tests use an embedded H2 database by default. CI runs the same tests against PostgreSQL 17.

## Run locally

Install Docker with Compose and start its engine. In PowerShell, run the following in the same session. Choose a local development password when prompted.

```powershell
$env:HINKLE_DB_PASSWORD = Read-Host 'Local database password'
docker compose up -d --wait db
.\gradlew.bat bootRun --args='--spring.profiles.active=local'
```

Open http://localhost:8080. On macOS or Linux export `HINKLE_DB_PASSWORD` first and use `./gradlew`. Stop the app with Ctrl+C and the database with `docker compose down`. Compose retains database data and the original password in its named volume.

## Continuous integration and delivery

GitHub Actions builds every branch push and pull request with Java 21 and PostgreSQL. Each successful run uploads the runnable JAR. Publishing a GitHub Release rebuilds its tag, runs both smoke tests, and attaches the verified `hinkle.jar` to that release.

To run the JAR against a provisioned PostgreSQL database, set `SPRING_DATASOURCE_URL`, `SPRING_DATASOURCE_USERNAME`, and `SPRING_DATASOURCE_PASSWORD` in the host environment and run `java -jar hinkle.jar`.
