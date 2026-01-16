# Use official Maven image to build the project
FROM maven:3.9.4-eclipse-temurin-20 AS build

WORKDIR /app

# Copy pom.xml and download dependencies
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy source code
COPY src ./src

# Build the jar
RUN mvn package -DskipTests

# Use a lightweight Java runtime to run the app
FROM eclipse-temurin:20-jdk-alpine

WORKDIR /app

# Copy the jar from the build stage
COPY --from=build /app/target/*.jar app.jar

# Expose the port Render will use
EXPOSE 10000

# Use Render's PORT environment variable
ENV PORT=10000

# Run the jar
ENTRYPOINT ["sh", "-c", "java -jar app.jar --server.port=$PORT"]
