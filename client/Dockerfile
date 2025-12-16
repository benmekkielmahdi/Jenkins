FROM maven:3.9-eclipse-temurin-17 AS builder
WORKDIR /app
COPY ./src ./src
COPY ./pom.xml .
RUN mvn clean package -DskipTests

FROM eclipse-temurin:17-jre
VOLUME /tmp
ARG JAR_FILE=target/*.jar
COPY --from=builder /app/target/*.jar client-service.jar
ENTRYPOINT ["java","-jar","/client-service.jar"]