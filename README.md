---
# ALL COMMANDS
---

## Practical 10: ARGO CD

**[POWERSHELL]**

```
fork GitHub argocd
minikube start --driver=docker
minikube status
kubectl status
kubectl get nodes
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl get pods -n argocd

kubectl port-forward svc/argo-server -n argocd 8080:443 --address 0.0.0.0
```

```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"

[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("SjZFWGdhN0lUOWp1ZlBxdw=="))
```

---

# Practical 9: Kafka Demo

**[POWERSHELL]**

```
cd desktop
mkdir kafa-demo
```

Create **Docker-compose.yml** file inside kafka-demo folder

```
version: "3.8"

services:

  zookeeper:
    image: confluentinc/cp-zookeeper:7.6.0
    container_name: zookeeper
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
    ports:
      - "2181:2181"

  kafka:
    image: confluentinc/cp-kafka:7.6.0
    container_name: kafka
    depends_on:
      - zookeeper
    ports:
      - "9092:9092"
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:9092
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
```

```
ls
docker compose up -d
docker compose -f docker-compose.yml up -d
docker ps
```

Create Topic

```
docker exec -it kafka kafka-topics --create --topic quick-demo --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
```

Open another PowerShell

```
docker exec -it kafka kafka-topics --describe --topic quick-demo --bootstrap-server localhost:9092
docker exec -it kafka kafka-console-producer --topic quick-demo --bootstrap-server localhost:9092 .
```

Previous PowerShell

```
docker exec -it kafka kafka-console-consumer --topic quick-demo --bootstrap-server localhost:9092 .
```

---

# Practical 8: Kubernetes Working & Installation

**[POWERSHELL]**

```
minikube start --driver=docker
minikube status
kubectl get nodes

kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0

kubectl expose deployment hello-minikube --type=NodePort --port=8080

minikube service hello-minikube --url
```

---

# Assignment

### Build an image installing git and apache2 via Dockerfile

Create folder **soa**

Create 2 files

```
index.html
Dockerfile
```

Dockerfile

```
FROM ubuntu:22.04

RUN sed -i 's/archive.ubuntu.com/mirrors.edge.kernel.org/g' /etc/apt/sources.list

RUN apt-get update && apt-get install -y \
    git \
    apache2 \
    && apt-get clean

COPY index.html /var/www/html/index.html

EXPOSE 80

CMD ["apachectl", "-D","FOREGROUND"]
```

Open **soa folder in PowerShell**

```
docker build -t apache-docker .
docker run -d -p 8090:80 apache-docker
```

Access Website

```
http://localhost:8090
```

Deploy to Kubernetes

```
minikube image load apache-docker

kubectl create deployment apache-deployment --image=apache-docker --image-pull-policy=Never

minikube service apache-deployment
```

---

# Practical 6: RESTful API Microservice + Docker Image

Create Web App in NetBeans

```
microservice
pattern: microservice-jaxrs
resource package: com.example
```

```
public String hello(){
    return "Jax-RS Microservice -DockerReady";
}

@GET
@Path("status")
@Produces(MediaType.APPLICATION_JSON)
public String status(){
    return "{\"status\":\"healthy\",\"service\":\"microservice\"}";
}
```

WAR file generated in:

```
project > dist
```

Dockerfile

```
FROM tomcat:8.5-jdk8

COPY microservice-jaxrs.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080

CMD ["catalina.sh", "run"]
```

Commands

```
docker build -t microservice .
docker run microservice

docker run -d -p 9090:8080 --name jax-rs microservice
```

Open Browser

```
localhost:9090
```

---

# Practical 5: Docker with SQL

Create Folder

Files

```
hello.py
Dockerfile
```

hello.py

```
print("hello from python")
```

Dockerfile

```
FROM python:3.12-slim

WORKDIR /app

COPY hello.py .

CMD ["python","hello.py"]
```

Commands

```
docker build -t py-hello .

docker run py-hello

docker pull mysql:latest

docker run -d --name mydemo -p 3306:3306 -e MYSQL_ROOT_PASSWORD=student123 mysql:latest

docker exec -it mydemo mysql -uroot -pstudent123
```

---

# Practical 4: JAX-RS Client

Create Java Application

```
JAXRS_Client
```

Add Library → JAXRS

Create Class

```
restClient.java
```

```
public static void main(String[] args){

    Client c = ClientBuilder.newClient();

    WebTarget t = c.target("http://localhost:8080/restPatterns/webresources/strings/upper")
            .queryParam("text","hello shelly");

    String res = t.request(MediaType.TEXT_PLAIN).get(String.class);

    System.out.println("RESPONSE: "+res);

    c.close();
}
```

---

# Practical 3: JAX-RS Demo

Project

```
UpperLower
```

Resource

```
stringRS
package: stringrs
```

```
@GET
@Path("lower")
@Produces(MediaType.TEXT_PLAIN)
public String toLower(@QueryParam("text") String text){
    return text.toLowerCase();
}

@GET
@Path("upper")
@Consumes(MediaType.TEXT_PLAIN)
public String toUpper(@QueryParam("text") String text){
    return text.toUpperCase();
}
```

HelloWorld Service

```
@GET
@Produces(MediaType.TEXT_HTML)
public String getXml(){
    return "<html><body>HELLO WORLD</body></html>";
}

@PUT
@Path("{id}")
@Produces(MediaType.TEXT_PLAIN)
@Consumes(MediaType.APPLICATION_JSON)

public Response updateMessage(@PathParam("id") String id, String message){

    String result = "Update message " + id + " with " + message;

    return Response.ok(result).build();
}
```

---

# Practical 2: JAX-WS (NAAC Web Service)

Create Web App

```
NaacRate
```

Web Service

```
NaacService
package: client
```

```
@WebMethod(operationName = "rate")
public String rate(@WebParam(name = "naac") String naac){

    if("jai hind".equals(naac)){
        return "A";
    }

    else if("kc".equals(naac)){
        return "B";
    }

    return "invalid";
}
```

Client

```
public static void main(String[] args){

    client.NaacService_Service Service = new client.NaacService_Service();

    client.NaacService port = Service.getNaacServicePort();

    String collegeName = "kc";

    String result = port.rate(collegeName);

    System.out.println("RATING: " + result);
}
```

---

# JAX-WS SOAP Services

### Currency Conversion

```
convertUSDToINR(double usd)
rate = 83
return usd * rate
```

WSDL

```
http://localhost:8080/CurrencySOAP/CurrencyService?wsdl
```

---

### Student Details

```
getStudentDetails(int id)
```

```
if(id == 1) Rahul IT
if(id == 2) Anita CS
else Student Not Found
```

---

### Power Service

```
power(a,b) = Math.pow(a,b)
```

---

### Temperature Conversion

```
celsiusToFahrenheit(c)

return (c * 9/5) + 32
```

---

# JAX-RS REST APIs

### Student JSON API

```
GET /student
```

URL

```
http://localhost:8080/RestServices/webresources/student
```

---

### System Status JSON

```
GET /status
```

URL

```
http://localhost:8080/RestServices/webresources/status
```

---

### HTML Response

```
GET /html
```

URL

```
http://localhost:8080/RestServices/webresources/html
```

---

# REST CRUD Library API

Project

```
LibraryREST
```

Book Class

```
id
name
author
```

GET Books

```
GET /library
```

POST Book

```
POST /library
```

Example Output

```
[
 {id:1,name:"Java",author:"James Gosling"},
 {id:2,name:"Python",author:"Guido"}
]
```

````md
# LibraryREST (JAX-RS + MySQL)

# Book Fields

Each book contains:

- id (long)
- title (String)
- author (String)
- isbn (String)

---

# Technologies Used

- Java
- JAX-RS (Jersey)
- MySQL Database
- JDBC
- REST API
- JSON

---

# Database Setup

Create the database and table.

```sql
CREATE DATABASE librarydb;

USE librarydb;

CREATE TABLE books (
    id BIGINT PRIMARY KEY,
    title VARCHAR(255),
    author VARCHAR(255),
    isbn VARCHAR(50)
);
```
````

---

# Project Structure

```
LibraryREST
│
├── src/main/java
│   ├── model
│   │   └── Book.java
│   │
│   ├── dao
│   │   └── BookDAO.java
│   │
│   ├── service
│   │   └── LibraryService.java
│   │
│   └── config
│       └── ApplicationConfig.java
```

---

# Book Model

```java
package model;

public class Book {

    private long id;
    private String title;
    private String author;
    private String isbn;

    public Book(){}

    public Book(long id, String title, String author, String isbn) {
        this.id = id;
        this.title = title;
        this.author = author;
        this.isbn = isbn;
    }

    public long getId() { return id; }

    public void setId(long id) { this.id = id; }

    public String getTitle() { return title; }

    public void setTitle(String title) { this.title = title; }

    public String getAuthor() { return author; }

    public void setAuthor(String author) { this.author = author; }

    public String getIsbn() { return isbn; }

    public void setIsbn(String isbn) { this.isbn = isbn; }
}
```

---

# Database Connection (DAO)

```java
package dao;

import model.Book;
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public class BookDAO {

    private Connection getConnection() throws Exception {
        String url = "jdbc:mysql://localhost:3306/librarydb";
        String user = "root";
        String password = "root";

        Class.forName("com.mysql.cj.jdbc.Driver");
        return DriverManager.getConnection(url, user, password);
    }

    public void addBook(Book book) throws Exception {
        Connection con = getConnection();
        String query = "INSERT INTO books VALUES (?,?,?,?)";

        PreparedStatement ps = con.prepareStatement(query);
        ps.setLong(1, book.getId());
        ps.setString(2, book.getTitle());
        ps.setString(3, book.getAuthor());
        ps.setString(4, book.getIsbn());

        ps.executeUpdate();
        con.close();
    }

    public List<Book> getBooks() throws Exception {
        Connection con = getConnection();
        List<Book> list = new ArrayList<>();

        Statement st = con.createStatement();
        ResultSet rs = st.executeQuery("SELECT * FROM books");

        while(rs.next()){
            Book b = new Book(
                rs.getLong("id"),
                rs.getString("title"),
                rs.getString("author"),
                rs.getString("isbn")
            );
            list.add(b);
        }

        con.close();
        return list;
    }
}
```

---

# REST Service

```java
package service;

import dao.BookDAO;
import model.Book;

import jakarta.ws.rs.*;
import jakarta.ws.rs.core.MediaType;
import java.util.List;

@Path("/books")
public class LibraryService {

    BookDAO dao = new BookDAO();

    @GET
    @Produces(MediaType.APPLICATION_JSON)
    public List<Book> getBooks() throws Exception {
        return dao.getBooks();
    }

    @POST
    @Consumes(MediaType.APPLICATION_JSON)
    @Produces(MediaType.APPLICATION_JSON)
    public Book addBook(Book book) throws Exception {
        dao.addBook(book);
        return book;
    }
}
```

---

# Application Configuration

```java
package config;

import jakarta.ws.rs.ApplicationPath;
import jakarta.ws.rs.core.Application;

@ApplicationPath("/api")
public class ApplicationConfig extends Application {
}
```

---

# API Endpoints

## Get All Books

```
GET /api/books
```

---

## Add Book

```
POST /api/books
```

Example JSON:

```json
{
  "id": 1,
  "title": "Java Programming",
  "author": "James Gosling",
  "isbn": "12345"
}
```
