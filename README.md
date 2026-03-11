ALL COMMANDS:

------------------------------------------------------------------------
practical 10: ARGO CD
[POWERSHELL]
fork GitHub argocd
minikube start --driver=docker
minikube status
kubectl status
kubectl get nodes
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl get pods -n argocd

kubectl port-forward svc/argo-server -n argocd 8080:443 --address 0.0.0.0

<h5>kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("SjZFWGdhN0lUOWp1ZlBxdw=="))</h5>
------------------------------------------------------------------------------------
practical 9: Kafka Demo
[POWERSHELL]
cd desktop
mkdir kafa-demo
[create Docker-compose.yml file in kafka-demo folder]
Docker-compose.yml:
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

ls
docker compose up -d
docker compose -f docker-compose.yml up -d
docker ps
docker exec -it kafka kafka-topics --create --topic quick-demo --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1

open another powershell:
docker exec -it kafka kafka-topics --describe --topic quick-demo --bootstrap-server localhost:9092
docker exec -it kafka kafka-console-producer --topic quick-demo --bootstrap-server localhost:9092 .

previous powershell:
docker exec -it kafka kafka-console-consumer --topic quick-demo --bootstrap-server localhost:9092 .
-----------------------------------------------------------------------------------------------------------
practical 8:kubernetes working & installation
[POWERSHELL]
minikube start --driver=docker
minikube status
kubectl get nodes
kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
kubectl expose deployment hello-minikube --type=NodePort --port=8080
minikube service hello-minikube --url
-----------------------------------------------------------------------------
assignment 
Build an image installing git and apache2 via Dockerfile

create a folder soa
2 file:
index.html
Dockerfile:
FROM ubuntu:22.04
RUN sed -i 's/archive.ubuntu.com/mirrors.edge.kernel.org/g'
/etc/apt/sources.list
RUN apt-get update && apt-get install -y \
    git \
    apache2 \
    && apt-get clean
COPY index.html /var/www/html/index.html
EXPOSE 80
CMD ["apachectl", "-D","FOREGROUND"]

open soa in powershell:
docker build -t apache-docker .
docker run -d -p 8090:80 apache-docker
- now we can access the website using `http://localhost:8090`
- once this is done we can now put this up on kubernetes cluster using minikube and kubectl
- we have add the image to minikube using `minikube image load apache-docker`
- we have to create a deployment for the apache server using `kubectl create deployment apache-deployment --image=apache-docker --image-pull-policy=Never`
- we have to expose the deployment using `minikube service apache-deployment`
--------------------------------------------------------------------------------
practical 6: Create a Restful API Microservice and Create a Docker Image of the Microservice

create a web app on netbeans > microservice
create pattern: microservice-jaxrs
resource package: com.example

public String hello(){
        return "Jax-RS Microservice -DockerReady";
    }
   
    @GET
    @Path("status")
    @Produces(MediaType.APPLICATION_JSON)
    public String status(){
        return "{\"status\":\"healthy\",\"service\":\"microservice\"}";
    }

.war file will be created in ur project > dist 
create a Dockerfile for tomcat:
FROM tomcat:8.5-jdk8
COPY microservice-jaxrs.war /usr/local/tomcat/webapps/ROOT.war
EXPOSE 8080
CMD ["catalina.sh", "run"]

go to powershell in dist location
docker build -t microservice .
docker run microservice
docker run -d -p 9090:8080 --name jax-rs microservice
go to google > localhost:9090
----------------------------------------------------------------------------
practical 5: Use of docker with sql in cmd

create a folder:
create 2 files:
hello.py:
print("hello from python")

Dockerfile:
FROM python:3.12-slim
WORKDIR /app
COPY hello.py .
CMD ["python","hello.py]

go to powershell in that folder:
docker build -t py-hello .
docker run py-hello
docker pull MySQL:latest
docker run -d --name mydemo -p 3306:3306 -e MYSQL_ROOT_PASSWORD=student123 mysql:latest
docker exec -it mydemo mysql -uroot -pstudent123
----------------------------------------------------------------------------
Prac4 : Jax RS Client For Hello World and String Conversion RS Services

java app>JAXRS_Client
click right > properties > add library
new java class:
name:restClient.java
package: default

public static void main(String[] args){
        Client c = ClientBuilder.newClient();
        WebTarget t = c.target("http://localhost:8080/restPatterns/webresources/strings/upper").
                queryParam("text","hello shelly");
        String res = t.request(MediaType.TEXT_PLAIN).get(String.class);
        System.out.println("RESPONSE: "+res);
        c.close();
    }
----------------------------------------------------------------------------
Prac 3: Jax Rs Demo and Prac

java web: UpperLower
add patterns:
name:stringRS
package name: stringrs

@GET
    @Path("lower")
    @Produces(MediaType.TEXT_PLAIN)
    public String toLower(@QueryParam("text") String text) {
        return text.toLowerCase();
    }
    
@GET
    @Path("upper")
    @Consumes(MediaType.TEXT_PLAIN)
    public String toUpper(@QueryParam("text") String text){
        return text.toUpperCase();
    }

java web: helloworld:

@GET
    @Produces(MediaType.TEXT_HTML)
    public String getXml() {
        return "<html><body>HELLO WORLD</body></html>";
    }

    @PUT
    @Path("{id}")
    @Produces(MediaType.TEXT_PLAIN)
    @Consumes(MediaType.APPLICATION_JSON)
    
    public Response updateMessage(@PathParam("id") String id, String message) 
    {
        String result = "Update message " + id + " with " + message;
        return Response.ok(result).build();
    }
----------------------------------------------------------------------------
Prac2: Web Service Jax-ws (naac)

crate web app: NaacRate
add webservice: 
name: NaacService
package: client

@WebMethod(operationName = "rate")
    public String rate(@WebParam(name = "naac") String naac) {
        if ("jai hind".equals(naac)){
            return "A";
        }
        else if("kc".equals(naac)){
        return "B";
        }
        return "invild";
    }


create java app: NaacClient
web service client : paste WSDL url

add java class:
name: NaacClient
package: naacclient

public static void main(String[] args) {

        client.NaacService_Service Service = new client.NaacService_Service();
        client.NaacService port = Service.getNaacServicePort();
        
        String collegeName = "kc";
        String result = port.rate(collegeName);
        
        System.out.println("RATING: " + result);
    }

run java file

create a new web app: TestClient
add web service client 
paste WSDL url

add java class:
name: NaacServlet
package: naac.web

@WebServlet("/naac")
public class NaacServlet extends HttpServlet {

    protected void processRequest(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        
        String collegeName = request.getParameter("college");
        
        client.NaacService_Service Service = new client.NaacService_Service();
        client.NaacService port = Service.getNaacServicePort();
        
        String result = port.rate(collegeName);
        
        request.setAttribute("rating", result);
        request.getRequestDispatcher("result.jsp").forward(request, response);
    }

add two jsp file inside webpages:
index.jsp:
<body>
        <h1>College Rating</h1>
        <form action="naac" method="post">
            College Name:
            <input type="text" name="college" required/>
            <br>
            <input type="submit" value="Get Rating"/>
        </form>
</body>

result.jsp:
<body>
        <h1>Naac Rating</h1>
        <p>Rating: ${rating}</p>
        <br><br>
        
        <a href="index.jsp">back</a>
    </body>
----------------------------------------------------------------------
JAX_WS

SOAP Service – Currency Conversion:
Create Project
Java Web → Web Application
Project Name: CurrencySOAP

Create Web Service
Right click Source Packages
New → Web Service
Name: CurrencyService
Package: com.soap

Add Method:

package com.soap;

import javax.jws.WebService;
import javax.jws.WebMethod;

@WebService
public class CurrencyService {

    @WebMethod
    public double convertUSDToINR(double usd) {
        double rate = 83.0;
        return usd * rate;
    }
}

Test Service
http://localhost:8080/CurrencySOAP/CurrencyService?wsdl
----------------------------------------------------------
SOAP Service – Student Details by ID
Web Service
Name: StudentService

Code:
package com.soap;

import javax.jws.WebService;
import javax.jws.WebMethod;

@WebService
public class StudentService {

    @WebMethod
    public String getStudentDetails(int id) {

        if(id == 1)
            return "ID:1 Name:Rahul Course:IT";

        if(id == 2)
            return "ID:2 Name:Anita Course:CS";

        return "Student Not Found";
    }
}
Step 3: Run
http://localhost:8080/CurrencySOAP/StudentService?wsdl
-----------------------------------------------------------
SOAP Service – Power(a,b)

Create Web Service > PowerService
Code:

package com.soap;

import javax.jws.WebService;
import javax.jws.WebMethod;

@WebService
public class PowerService {

    @WebMethod
    public double power(int a, int b) {
        return Math.pow(a, b);
    }
}
Step 3: Run
http://localhost:8080/CurrencySOAP/PowerService?wsdl
-------------------------------------------------------
SOAP Service – Temperature Conversion

Create Web Service > TemperatureService
Code:

package com.soap;

import javax.jws.WebService;
import javax.jws.WebMethod;

@WebService
public class TemperatureService {

    @WebMethod
    public double celsiusToFahrenheit(double c) {
        return (c * 9/5) + 32;
    }
}
Step 3: Run
http://localhost:8080/CurrencySOAP/TemperatureService?wsdl

-------------------------------------------------------------------
JAX-RS

Create REST API that returns JSON Student Object

Java Web → Web Application
Project Name: RestServices

Right click Project
New → RESTful Web Services from Patterns
Select Simple Root Resource Class

Class Name: StudentService
Package: com.rest

Create Student Class
Create new class: Student.java
package com.rest;

public class Student {

    public int id;
    public String name;
    public String course;

    public Student(int id, String name, String course) {
        this.id = id;
        this.name = name;
        this.course = course;
    }
}

Resource Class
package com.rest;

import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;
import javax.ws.rs.core.MediaType;

@Path("student")
public class StudentService {

    @GET
    @Produces(MediaType.APPLICATION_JSON)
    public Student getStudent() {

        Student s = new Student(1,"Rahul","IT");
        return s;
    }
}

Open browser:
http://localhost:8080/RestServices/webresources/student
------------------------------------------------------------
REST API that returns System Status in JSON
Create Resource: SystemService.java
package com.rest;

import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;
import javax.ws.rs.core.MediaType;

@Path("status")
public class SystemService {

    @GET
    @Produces(MediaType.APPLICATION_JSON)
    public String systemStatus(){

        return "{\"status\":\"Server Running\"}";
    }
}
Run
http://localhost:8080/RestServices/webresources/status
------------------------------------------------------------
REST API that returns HTML Response
Create Resource: HtmlService.java
package com.rest;

import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;
import javax.ws.rs.core.MediaType;

@Path("html")
public class HtmlService {

    @GET
    @Produces(MediaType.TEXT_HTML)
    public String getHTML(){

        return "<h1>Welcome to REST Service</h1>";
    }
}
Run
http://localhost:8080/RestServices/webresources/html
-----------------------------------------------------------------
create a REST CRUD Library API (GET + POST) using JAX-RS.

Java Web → Web Application
Project Name: LibraryREST

Configure REST
Right click Project
RESTful Web Services from Patterns

Class Name: LibraryService
Package: com.rest

Create Book Class
Create new class: Book.java
package com.rest;

public class Book {

    public int id;
    public String name;
    public String author;

    public Book(){}

    public Book(int id,String name,String author){
        this.id=id;
        this.name=name;
        this.author=author;
    }
}

Create REST Service

Replace LibraryService.java code with:

package com.rest;

import java.util.ArrayList;
import java.util.List;
import javax.ws.rs.GET;
import javax.ws.rs.POST;
import javax.ws.rs.Path;
import javax.ws.rs.Consumes;
import javax.ws.rs.Produces;
import javax.ws.rs.core.MediaType;

@Path("library")
public class LibraryService {

    static List<Book> books = new ArrayList<>();

    static{
        books.add(new Book(1,"Java","James Gosling"));
        books.add(new Book(2,"Python","Guido"));
    }

    @GET
    @Produces(MediaType.APPLICATION_JSON)
    public List<Book> getBooks(){
        return books;
    }

    @POST
    @Consumes(MediaType.APPLICATION_JSON)
    public String addBook(Book b){
        books.add(b);
        return "Book Added";
    }
}
Right click project → Run

Test GET (Fetch Books)

Open browser:

http://localhost:8080/LibraryREST/webresources/library

Output:

[
 {id:1,name:"Java",author:"James Gosling"},
 {id:2,name:"Python",author:"Guido"}
]

Test POST (Add Book)
POST
http://localhost:8080/LibraryREST/webresources/library
