
**VProfile** is a Java-based web application built as a WAR package and designed for deployment on a servlet container such as Apache Tomcat. It uses Spring MVC, Spring Security, Spring Data JPA, Hibernate, RabbitMQ, Elasticsearch, and MySQL.

## Key Features

- Web-based user profile management UI using JSP views
- Spring MVC + Spring Security for authentication and authorization
- JDBC / JPA persistence with MySQL
- RabbitMQ messaging integration
- Elasticsearch search support
- Memcached caching configuration
- Docker multi-stage build support
- CI/CD workflow using GitHub Actions with SonarQube and AWS ECR deployment

## Technology Stack

- Java 17 / JDK 21 (CI uses JDK 21)
- Maven
- Spring Framework 6 / Spring Boot 3.x libraries
- Spring Security 6
- Spring Data JPA
- Hibernate ORM
- MySQL Connector/J
- Elasticsearch Java client
- Spring Rabbit / RabbitMQ
- JSP + Servlet API
- Tomcat runtime
- JUnit 4, JUnit 5, Mockito

## Repository Structure

- `pom.xml` — Maven project definition and dependencies
- `src/main/java` — Java source code
- `src/main/resources` — application properties and SQL resources
- `src/main/webapp` — JSP views, static assets, and Spring config XML files
- `Docker-files` — Docker image definitions and multi-stage build files
- `.github/workflows/ci.yml` — GitHub Actions pipeline for build, tests, SonarQube, and AWS ECR

## Prerequisites

- Java JDK 17 or newer
- Maven 3.9+
- MySQL database
- RabbitMQ server
- Elasticsearch cluster or node
- Memcached server
- Docker (optional, for container builds)
- AWS credentials if using GitHub Actions ECR deployment

## Local Build

From the project root:

```bash
mvn clean package
```

This generates a WAR file under `target/vprofile-v2.war`.

## Run in a Servlet Container

Deploy `target/vprofile-v2.war` to an Apache Tomcat or compatible servlet container.

## Docker Build

The multi-stage Dockerfile is located at `Docker-files/app/multistage/Dockerfile`.

To build the Docker image:

```bash
docker build -f Docker-files/app/multistage/Dockerfile -t vprofile-app .
```

To run the container:

```bash
docker run -p 8080:8080 --name vprofile-app vprofile-app
```

> Note: the current Dockerfile clones a remote repository and builds the WAR from that source. If you want to use the local project sources directly, update the Dockerfile accordingly.

## Configuration

Application configuration is defined in `src/main/resources/application.properties`.

Important settings include:

- `jdbc.url`, `jdbc.username`, `jdbc.password`
- `memcached.active.host`, `memcached.active.port`
- `rabbitmq.address`, `rabbitmq.port`, `rabbitmq.username`, `rabbitmq.password`
- `elasticsearch.host`, `elasticsearch.port`, `elasticsearch.cluster`
- `spring.servlet.multipart.max-file-size`
- `spring.mvc.view.prefix` / `spring.mvc.view.suffix`

### Default property values

- MySQL: `jdbc:mysql://vprodb:3306/accounts`
- RabbitMQ: `vpromq01:5672`
- Elasticsearch: `localhost:9300`
- Memcached active host: `vprocache01`

Adjust these values to match your local or container network environment.

## Testing

Run the test suite with Maven:

```bash
mvn test
```

The project uses JUnit 4, JUnit 5, and Mockito for unit testing.

## CI/CD

The GitHub Actions workflow at `.github/workflows/ci.yml` performs:

- checkout
- JDK 21 setup
- Maven dependency caching
- `mvn clean verify checkstyle:checkstyle`
- SonarQube scan and quality gate on pull requests
- Docker build and AWS ECR push on `main` branch pushes
- Helm values update in a separate repository after image push

## Notes

- SQL schema files are available in `src/main/resources/accountsdb.sql` and `src/main/resources/db_backup.sql`.
- Web application configuration XMLs are under `src/main/webapp/WEB-INF/`.
- Static UI assets are stored inside `src/main/webapp/resources/`.

## Contribution

To contribute or extend this project:

1. Fork the repository
2. Build and verify locally using Maven
3. Update configuration for your environment
4. Create a feature branch and submit a pull request

---

If you need help running the app locally, update the database and service host names in `src/main/resources/application.properties` before starting the container or deploying the WAR.