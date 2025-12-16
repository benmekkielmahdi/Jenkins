FROM eclipse-temurin:17-jre
WORKDIR /app
COPY target/*.jar eureka-server.jar
ENTRYPOINT ["java", "-jar", "eureka-server.jar"]