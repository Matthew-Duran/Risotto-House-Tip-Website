# Stage 1: Build the app using Maven + OpenJDK 24
FROM maven:3.9.0-openjdk-24 AS build

WORKDIR /app

# Copy pom.xml first for caching dependencies
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy source code
COPY src ./src

# Build the jar (skip tests to speed up)
RUN mvn package -DskipTests

# Stage 2: Run the app
FROM eclipse-temurin:24-jre

WORKDIR /app

# Copy the jar from the build stage
COPY --from=build /app/target/*.jar ./app.jar

# Expose the port your app uses
EXPOSE 8080

# Run the app
ENTRYPOINT ["java", "-jar", "app.jar"]
