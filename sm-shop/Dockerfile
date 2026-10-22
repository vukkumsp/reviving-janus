# Run "docker build -f sm-shop/Dockerfile -t shopizer . " from root folder
# docker run --rm --name shopizer -p 8080:8080 shopizer
# Stage 1: Build Shopizer
FROM maven:3.8.8-eclipse-temurin-11 AS builder

WORKDIR /app

# Copy the complete repository into the build context
COPY . .

# Build all modules from the root POM
RUN mvn -f pom.xml clean package -DskipTests

# Stage 2: Run Shopizer
FROM adoptopenjdk/openjdk11-openj9:alpine

WORKDIR /opt/app

RUN mkdir -p /files

COPY --from=builder /app/sm-shop/target/shopizer.jar /opt/app/shopizer.jar
COPY sm-shop/SALESMANAGER.h2.db /
COPY sm-shop/files /files

CMD ["java", "-jar", "/opt/app/shopizer.jar"]
