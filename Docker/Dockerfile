# Etapa 1: Imagem base para compilar o projeto
FROM maven:3-eclipse-temurin-21-alpine AS builder

# Definir o diretório de trabalho no container
WORKDIR /app

# Instalar Git para capturar informações do commit
RUN apk add --no-cache git

# Copiar o arquivo pom.xml para o container
COPY ../pom.xml .

# Baixar as dependências
RUN mvn dependency:go-offline

# Agora, copiar todo o código-fonte do projeto para o container
COPY ../. .

# Compilar o projeto e gerar o jar
RUN mvn clean package -DskipTests

# Etapa 2: Imagem base para rodar a aplicação
FROM eclipse-temurin:21-jre-alpine

# Definir o diretório de trabalho
WORKDIR /app

# Copia o JAR gerado na etapa de build
COPY --from=builder /app/target/dslist-0.0.1-SNAPSHOT.jar /app/dslist.jar

# Comando para executar a aplicação
ENTRYPOINT ["java", "-jar", "/app/dslist.jar"]