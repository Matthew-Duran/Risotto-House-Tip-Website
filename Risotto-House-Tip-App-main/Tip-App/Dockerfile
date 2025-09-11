# Stage 1: Build with JDK 24 and Maven
FROM eclipse-temurin:24-jdk AS build

WORKDIR /app

# Install Maven
RUN apt-get update && \
    apt-get install -y maven && \
    rm -rf /var/lib/apt/lists/*

# Copy pom.xml first for dependency caching
COPY pom.xml .

# Download dependencies only (optional, speeds up rebuilds)
RUN mvn dependency:go-offline

# Copy the source code
COPY src ./src

# Build the app (skip tests to avoid issues)
RUN mvn package -DskipTests

# Stage 2: Run the app with just JRE
FROM eclipse-temurin:24-jre

WORKDIR /app

# Copy the built jar from build stage
COPY --from=build /app/target/*.jar ./app.jar

# Expose port (use your app port, default 8080)
EXPOSE 8080

# Run the app
ENTRYPOINT ["java", "-jar", "app.jar"]
