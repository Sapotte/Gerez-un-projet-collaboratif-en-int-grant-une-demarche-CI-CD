FROM maven:3.6.3-jdk-11-slim AS build
RUN mkdir -p /workspace
WORKDIR /workspace
COPY pom.xml /workspace
COPY src /workspace/src
RUN mvn -B -f pom.xml clean package -DskipTests
FROM amazoncorretto:11
RUN useradd --system --uid 10001 --create-home appuser
COPY --from=build /workspace/target/*.jar app.jar
RUN chown -R appuser:appuser /app
USER 10001
EXPOSE 8080
ENTRYPOINT ["java", "-jar","app.jar"]
