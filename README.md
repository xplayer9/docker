# Docker for Spring Boot

## Spring Web Service
- To declare skip maven test in pom file
- In Eclipse, use Maven install to generate jar file

```Html
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <skipTests>true</skipTests>
    </configuration>
</plugin>
```

## Dockerfile
- Edit Dockerfile to build jar file into docker image
- "FROM": OS version
- "ADD": copy source jar file into docker image 
- "ENTRYPOINT": start commands used in image
- "EXPOSE": expose port
- Build docker image command: "docker build -t shopapp.jar . "

```Java
FROM openjdk:11
ADD target/shopApp.jar shopApp.jar
ENTRYPOINT ["java", "-jar","shopApp.jar"]
EXPOSE 8080
```

## docker-compose.yml
- Edit docker-compose.yml for multiple images execution
- Run: "docker-compose up -d"
- Stop: "docker-compose stop"

```Java
version: '3.1'
services:
  API:
    image: 'shopapp.jar'
    ports:
      - "8080:8080"
    depends_on:
      PostgreSQL:
        condition: service_healthy
    environment:
      - SPRING_DATASOURCE_URL=jdbc:postgresql://PostgreSQL:5432/postgres
      - SPRING_DATASOURCE_USERNAME=postgres
      - SPRING_DATASOURCE_PASSWORD=password
      - SPRING_JPA_HIBERNATE_DDL_AUTO=update

  PostgreSQL:
    image: postgres
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_PASSWORD=password
      - POSTGRES_USER=postgres
      - POSTGRES_DB=postgres
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5
```
