
# Spring AI PGVector Retrieval Demo

This project demonstrates how to retrieve vectorized data stored in a PostgreSQL database using the **PGVector** extension. The project is a continuation of the [SpringAI PGVector RAG Store Demo](https://github.com/LegPro/springai-pgvector-rag-store-demo.git), where data is stored as vectors in a PostgreSQL database. Here, we focus on retrieving this vectorized data using a RESTful API.

## Overview

In this demo, we use **Spring Boot** and **PGVector** to retrieve vectorized data from a PostgreSQL database. The stored vector data is searched based on similarity using a query, and the results are returned as a comma-separated list of relevant documents. The similarity search is based on a vector search using **OpenAI** embeddings stored in PostgreSQL via the **PGVector** extension.

## Prerequisites

- **Java 21** installed on your system.
- **Maven** installed for building and running the project.
- **Docker** installed for running PostgreSQL with the PGVector extension.
- The PostgreSQL instance with the vectorized data stored (refer to the [SpringAI PGVector RAG Store Demo](https://github.com/LegPro/springai-pgvector-rag-store-demo.git) to populate your PostgreSQL database with vectorized data).

## Related Project

Before running this demo, ensure that you have completed the setup of the [SpringAI PGVector RAG Store Demo](https://github.com/LegPro/springai-pgvector-rag-store-demo.git). This demo relies on the vectorized data stored in that project to perform the similarity search.

## Setup and Installation

### Step 1: Clone the Repository

Clone this repository to your local machine:

```bash
git clone https://github.com/LegPro/spring-ai-pgvector-retrieval-demo.git
cd spring-ai-pgvector-retrieval-demo
```

### Step 2: Set Up PostgreSQL with PGVector

If you haven't already set up PostgreSQL with PGVector (from the previous project), run the following Docker command:

```bash
docker run -it --name postgres -p 5432:5432 -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres pgvector/pgvector:0.7.4-pg16
```

This will start a PostgreSQL instance with the **PGVector** extension installed.

Make sure the vector data is already stored in your PostgreSQL instance (refer to the [SpringAI PGVector RAG Store Demo](https://github.com/LegPro/springai-pgvector-rag-store-demo.git)).

### Step 3: Configure the Database Connection

Ensure that your `application.properties` file in `src/main/resources` contains the correct connection settings for PostgreSQL. You can update these settings as needed.

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/postgres
spring.datasource.username=postgres
spring.datasource.password=postgres
spring.datasource.driver-class-name=org.postgresql.Driver
```

### Step 4: Build and Run the Application

You can build and run the Spring Boot application using Maven:

```bash
mvn clean install
mvn spring-boot:run
```

The application will start on `http://localhost:8080`.

## API Endpoints

### 1. `/chat`

- **Method**: `GET`
- **Description**: Retrieves the top 10 similar documents from the vectorized data in the PostgreSQL database based on a user query.
- **Query Parameter**:
    - `message`: The query to search for similar documents. (Default: "smartwatch with features like fitness tracking and health monitoring").
- **Example**:
    ```bash
    curl "http://localhost:8080/chat?message=smartwatch with features like fitness tracking and health monitoring"
    ```

- **Response**: A comma-separated list of relevant documents based on the similarity search using vectorized embeddings.

Example Response:
```
"Document 1 content, Document 2 content, Document 3 content, ..."
```

## How It Works

- The **`VectorStore`** interacts with the PostgreSQL database to perform a similarity search on the vectorized data stored previously.
- The **`SearchRequest`** contains the query which is transformed into a vector that is then used to find the top `K` (in this case, 10) closest matches from the database.
- The retrieved documents are returned in a comma-separated string format.

## Project Structure

```bash
spring-ai-pgvector-retrieval-demo/
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com
│   │   │       └── example
│   │   │           └── springaipgvectorretrieval
│   │   │               └── controller
│   │   │                   └── ChatController.java
│   └── resources
│       └── application.properties
├── pom.xml
└── README.md
```

- **ChatController.java**: Contains the REST API for retrieving the vectorized documents.
- **application.properties**: Configuration file for database and application properties.

## Maven Dependencies

The project includes the following key dependencies:

```xml
<dependencies>
    <!-- Spring Boot Starter Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Boot Data JPA for PostgreSQL -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- PostgreSQL Driver -->
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <scope>runtime</scope>
    </dependency>

    <!-- OpenAI Integration and Vector Store -->
    <dependency>
        <groupId>org.springframework.ai</groupId>
        <artifactId>spring-ai-openai</artifactId>
        <version>1.0.0</version>
    </dependency>

    <!-- Spring Boot Starter Test -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

## Additional Configuration

- You can adjust the **Top-K** parameter in the **`chat()`** method to retrieve more or fewer results by modifying `withTopK(10)`.

