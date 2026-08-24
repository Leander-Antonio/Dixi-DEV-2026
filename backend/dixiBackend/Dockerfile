# ---- Etapa 1: build da aplicação com Maven ----
FROM eclipse-temurin:17-jdk AS build
WORKDIR /app

# Copia primeiro os arquivos de build para aproveitar o cache de camadas do Docker
COPY .mvn/ .mvn/
COPY mvnw pom.xml ./
RUN chmod +x mvnw && ./mvnw dependency:go-offline -B

# Copia o restante do código-fonte e gera o jar
COPY src ./src
RUN ./mvnw clean package -DskipTests -B

# ---- Etapa 2: imagem final, apenas com o runtime ----
FROM eclipse-temurin:17-jre
WORKDIR /app

COPY --from=build /app/target/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
