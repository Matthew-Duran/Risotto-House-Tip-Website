# Use Maven + OpenJDK for building the app
FROM maven:3.9.1-openjdk-17 AS build

# Set working directory
WORKDIR /app

# Copy the project files
COPY pom.xml .
COPY src ./src

# Build the app and package as a jar
RUN mvn clean package -DskipTests

# Use a lightweight OpenJDK image to run the app
FROM openjdk:17-jdk-slim

WORKDIR /app

# Copy the jar from the build stage
COPY --from=build /app/target/TipApp.jar ./TipApp.jar

# Expose port if your app uses one (optional)
EXPOSE 8080

# Run the app
ENTRYPOINT ["java", "-jar", "TipApp.jar"]
