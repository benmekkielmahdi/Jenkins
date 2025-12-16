FROM eclipse-temurin:17-jre
WORKDIR /app
COPY target/*.jar car-service.jar
ENTRYPOINT ["java", "-jar", "car-service.jar"]