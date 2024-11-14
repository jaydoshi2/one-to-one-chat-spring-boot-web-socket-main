# 🗨️ One-to-One Chat Application

A powerful real-time one-to-one chat application built using **Spring Boot**, **WebSocket**, and **MongoDB**. This application facilitates seamless communication with instant message delivery, user management, and chat history storage.

## 📑 Project Structure

```plaintext
one-to-one-chat-spring-boot-websocket/
├── src/
│   ├── main/
│   │   ├── java/com/jd/websocket/
│   │   │   ├── chat/
│   │   │   ├── chatroom/
│   │   │   ├── config/
│   │   │   ├── user/
│   │   │   └── ChatApplication.java
│   │   └── resources/
│   │       ├── static/
│   │       │   ├── css/
│   │       │   ├── img/
│   │       │   ├── js/
│   │       │   └── index.html
│   │       └── application.yml
├── docker-compose.yml
├── Dockerfile
├── .gitignore
├── app-preview.png
└── mvnw
```

## 🛠️ Technologies & Concepts

- **Spring Boot**: Framework for building Java-based web applications.
- **WebSocket**: Real-time communication over a single TCP connection.
- **MongoDB**: NoSQL database for message and user data storage.
- **Docker**: Containerization platform for deployment.
- **Maven**: Build automation and dependency management.

## 🚀 Features

- **User Management**: Tracks user presence (online/offline).
- **One-to-One Chat**: Secure, private chat sessions.
- **Message History**: Stores chat history in MongoDB.
- **Real-time Updates**: Messages are delivered instantly using WebSocket.

![Chat Application Preview](https://github.com/jaydoshi2/one-to-one-chat-spring-boot-web-socket-main/blob/main/one_to_one_comm.png)

## 📦 Setup

### 1. Docker Compose

The `docker-compose.yml` file sets up the application and MongoDB.

```yaml
version: '3.8'

services:
  mongodb:
    image: mongo
    container_name: one-to-one-chat-spring-boot-web-socket-main
    ports:
      - 27017:27017
    volumes:
      - mongo:/data
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=admin

  mongo-express:
    image: mongo-express
    container_name: mongo-express
    restart: always
    ports:
      - 8081:8081
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD=admin
      - ME_CONFIG_MONGODB_SERVER=mongodb

volumes:
  mongo: {}
```

### 2. Dockerfile

To containerize the application:

```dockerfile
# Use the official Maven image as a build environment
FROM maven:3.8.4-openjdk-17 as build
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

# Use a minimal JRE to run the application
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8088
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### 3. Configuration (application.yml)

```yaml
spring:
  data:
    mongodb:
      host: localhost
      port: 27017
      database: chat_app
      authentication-database: admin
server:
  port: 8088
```

### 4. Steps to Run

1. **Build & Run with Docker**:
   - In the project root, run:
     ```sh
     docker-compose up
     ```
   - This starts both the MongoDB and chat application in containers.

2. **Local Development**:
   - Ensure Java 17 and MongoDB are installed.
   - Start the application:
     ```sh
     ./mvnw spring-boot:run
     ```

3. **Accessing the App**:
   - Once running, open `http://localhost:8088` in your browser for the main chat interface.
   - For database management, Mongo Express is accessible at `http://localhost:8081`.

## 🖼️ Preview

![Application Preview](https://github.com/jaydoshi2/one-to-one-chat-spring-boot-web-socket-main/blob/main/app-preview.png)

