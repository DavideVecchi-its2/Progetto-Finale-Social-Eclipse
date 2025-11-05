# Usa una immagine base con OpenJDK per Spring Boot
FROM openjdk:17-jdk-slim AS build
# Installa Maven
RUN apt-get update && apt-get install -y maven

# Imposta il percorso di lavoro
WORKDIR /app

# Copia il file pom.xml (o build.gradle) e scarica le dipendenze
COPY pom.xml ./
RUN mvn dependency:go-offline -B

# Copia il codice sorgente dell'applicazione
COPY src ./src

# Compila l'applicazione Spring Boot
RUN mvn clean package -DskipTests

# Usa un'immagine base per eseguire l'applicazione
FROM openjdk:17-jdk-slim

# Imposta il percorso di lavoro
WORKDIR /app

# Copia il file JAR dalla fase di build
COPY --from=build /app/target/*.jar /app/myapp.jar

# Esponi la porta che Spring Boot userà 
EXPOSE 8081

# Esegui l'applicazione
ENTRYPOINT ["java", "-jar", "/app/myapp.jar"]