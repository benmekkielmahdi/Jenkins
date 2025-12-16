FROM eclipse-temurin:17-jre
WORKDIR /app
COPY target/*.jar gateway-service.jar
ENTRYPOINT ["java", "-jar", "gateway-service.jar"]