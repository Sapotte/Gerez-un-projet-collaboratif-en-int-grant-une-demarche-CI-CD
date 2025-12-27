FROM maven:3.9-eclipse-temurin-11 AS build
WORKDIR /workspace

COPY pom.xml .
COPY src ./src
RUN mvn -B -ntp clean package -DskipTests

FROM amazoncorretto:11
COPY --from=build --chown=10001:0 /workspace/target/*.jar /app/app.jar

USER 10001
EXPOSE 8080
ENTRYPOINT ["java", "-jar","app.jar"]
