#### Stage 1: Build
FROM maven:3.9.6-eclipse-temurin-11 AS build

WORKDIR /app

COPY pom.xml .
RUN mvn dependency:go-offline -B

COPY src src
RUN mvn package -DskipTests

RUN mkdir -p target/dependency \
    && (cd target/dependency && jar -xf ../*.jar)

#### Stage 2: Runtime
FROM eclipse-temurin:11-jre-alpine

ENV db_username=default_user \
    db_password=default_password \
    db_host=localhost

ARG DEPENDENCY=/app/target/dependency

COPY --from=build ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=build ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=build ${DEPENDENCY}/BOOT-INF/classes /app

ENTRYPOINT ["java","-cp","app:app/lib/*","kl.socialnetwork.SocialNetworkApplication"]
