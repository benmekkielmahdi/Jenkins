FROM eclipse-temurin:17-jre
WORKDIR /app
COPY target/*.jar client-service.jar
ENTRYPOINT ["java","-jar","client-service.jar"]