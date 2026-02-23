# Build Stage
FROM maven:3.9.6-eclipse-temurin-21 AS build
WORKDIR /build

COPY pom.xml .
RUN mvn dependency:go-offline

COPY src ./src
RUN mvn clean package -DskipTests


# Runtime Stage
FROM eclipse-temurin:21-jre

ARG PROFILE=dev
ENV ACTIVE_PROFILE=${PROFILE}

WORKDIR /app

# Renommer le jar → app.jar
COPY --from=build /build/target/book-network-*.jar /app/app.jar

EXPOSE 8088

ENV DB_URL=jdbc:postgresql://postgres-sql-bsn:5432/book_social_network

CMD java -jar \
-Dspring.profiles.active=${ACTIVE_PROFILE} \
-Dspring.datasource.url=${DB_URL} \
app.jar
