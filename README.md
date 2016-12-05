# springboot-poc
Springboot microservices for sapient exercise

Steps to be followed to execute the product catalogue and pricing services.

Technologies and Databsed used.

SpringBootH2 in memory database
EhcacheJson (Jackson-bind) 

              <dependency>

                        <groupId>org.springframework.boot</groupId>

                        <artifactId>spring-boot-starter-actuator</artifactId>

                        <version>1.4.2.RELEASE</version>

                    </dependency>
To create HealthCheck of microservice

ProductCatalogue MicroService:
Try
mvn clean install from project root : product-catalogue-app

Build
will get success. If you get below failure , 

[WARNING] COMPILATION WARNING : 

[INFO]
-------------------------------------------------------------

[WARNING]
/C:/Gopinath/Microservices/product-catalogue-app/src/main/java/gopi/springframework/controllers/ProductController.java:
Some input files use unchecked or unsafe operations.

[WARNING]
/C:/Gopinath/Microservices/product-catalogue-app/src/main/java/gopi/springframework/controllers/ProductController.java:
Recompile with -Xlint:unchecked for details.

[INFO] 2 warnings 

[INFO]
-------------------------------------------------------------

[INFO]
-------------------------------------------------------------

[ERROR] COMPILATION ERROR : 

[INFO]
-------------------------------------------------------------

[ERROR] /C:/Gopinath/Microservices/product-catalogue-app/src/main/java/gopi/springframework/configuration/WebConfiguration.java:[3,25]
package org.h2.server.web does not exist

[ERROR]
/C:/Gopinath/Microservices/product-catalogue-app/src/main/java/gopi/springframework/configuration/WebConfiguration.java:[12,85]
cannot find symbol

 
symbol:   class WebServlet

 
location: class gopi.springframework.configuration.WebConfiguration

[INFO] 2 errors

 

please
right click the project and select maven and give update project (check force
update of snapshots/releases) . Try for couple of time , sometimes maven wont
load jars for the first time. I faced this issue.

 

Run
SpringBootProductCatalogueApplication.java as Java Application

Assumptions:

 

I have implemented a Cache with time
interval of 30 seconds. In real time scenario, assuming that the product
addition will not happen frequently. If it is happening frequently, we can go
for write-through cache where all write operations will happen through Cache
and so DB will be in sync with Cached data.Assuming Product Table will have
different relational entities/tables like Product_Type nad having association
with Product tableAssuming products will always be unique
and will have different namesAssuming no update operation required for
this exerciseAssuming productname in Catalogue DB
will be Unique and so added Unique Constraint in DB Table .  If you try to add same product again , you will get ConstraintViolationException
as a response to indicate that this productname column is unique.















 

Monitoring :

Added a HealthCheck Service to see whether the service is up and running or
not.

http://localhost:8081/v1/api/health

 



 

A)
Add / Create Products in Catalogue DB (HTTP Method :  POST))

 

http://localhost:8080/v1/products/product/add  

 

Sample Request

{

    "name": "TV",

    "description" : " Samsung
LED",

    "price": 20000,

    "productType" :
"Electronics"

    

}

Response
: Product with id :1 added successfully in catalogue

 

2.

{

    "name": "Mobile",

    "description" : "
Sony",

    "price": 11000,

    "productType" :
"Electronics"

    

}

Response
: Product with id :2 added successfully in catalogue

 

 

3.

{

    "name": "Shirt",

    "description" : " White Full
Sleeve",

    "price": 2000,

    "productType" :
"Apparels"

    

}

Response
: Product with id :3 added successfully in catalogue

 

 

4.

{

    "name": "Jewel",

    "description" : "
Gold",

    "price": 39000,

    "productType" : "Jewellery"

    

}

 

Response
: Product with id :4 added successfully in catalogue

 

5.

{

    "name": "Football",

    "description" : " test",

    "price": 1100,

    "productType" : "Sports"

    

}

 

Response
: Product with id :5 added successfully in catalogue

 

To
view H2 databse console : 

Please
refer the url and JDBC url in the below image:

 

 

 

 



 

 

B) List all available products (HTTP
Method : GET)

http://localhost:8080/v1/products/list

 

 

Response
:

 

[

  {

    "id": 1,

    "description": " Samsung
LED",

    "name": "TV",

    "price": 20000,

    "productType":
"Electronics",

    "productId": 1

  },

  {

    "id": 2,

    "description": " Sony",

    "name": "Mobile",

    "price": 11000,

    "productType":
"Electronics",

    "productId": 2

  },

  {

    "id": 3,

    "description": " White Full
Sleeve",

    "name": "Shirt",

    "price": 2000,

    "productType":
"Apparels",

    "productId": 3

  },

  {

    "id": 4,

    "description": " Gold",

    "name": "Jewel",

    "price": 39000,

    "productType":
"Jewellery",

    "productId": 4

  },

  {

    "id": 5,

    "description": " test",

    "name": "Football",

    "price": 1100,

    "productType":
"Sports",

    "productId": 5

  }

]

 

 

Delete Operation :

C)
Delete a product from Catalogye DB ( HTTP Method : DELETE))

http://localhost:8080/v1/products/product/delete/3

 

Response
: product with id : 3 has been deleted successfully

 

 

Since
I have used ehCaching ,if you try http://localhost:8080/v1/products/list

It
will not reflect immediately as I explained on top .I did not use write-through
Cache.

 

Wait
for 30 seconds to reflect the deleted record when you query the list.

 

D)
Display products by type (HTTP
Method : GET)

 

http://localhost:8080/v1/products/show/Electronics

 

[

  {

    "id": 1,

    "description": " Samsung
LED",

    "name": "TV",

    "price": 20000,

    "productType":
"Electronics",

    "productId": 1

  },

  {

    "id": 2,

    "description": " Sony",

    "name": "Mobile",

    "price": 11000,

    "productType":
"Electronics",

    "productId": 2

  }

]

 

 

http://localhost:8080/v1/products/show/Apparels 

 

[

  {

    "id": 3,

    "description": " White Full
Sleeve",

    "name": "Shirt",

    "price": 2000,

    "productType":
"Apparels",

    "productId": 3

  }

]

 

Implemented
Metrics for only product-catalogue micro services

 

http://localhost:8080/metrics 

{

  "mem": 614400,

  "mem.free": 253313,

  "processors": 4,

  "instance.uptime": 220722,

  "uptime": 228114,

  "systemload.average": -1,

  "heap.committed": 614400,

  "heap.init": 262144,

  "heap.used": 361086,

  "heap": 3702784,

  "threads.peak": 19,

  "threads.daemon": 16,

  "threads": 19,

  "classes": 9470,

  "classes.loaded": 9470,

  "classes.unloaded": 0,

  "gc.ps_scavenge.count": 6,

  "gc.ps_scavenge.time": 614,

  "gc.ps_marksweep.count": 2,

  "gc.ps_marksweep.time": 128,

  "httpsessions.max": -1,

  "httpsessions.active": 0,

  "datasource.primary.active": 0,

  "datasource.primary.usage": 0,

  "counter.status.200.metrics": 1,

  "counter.status.404.star-star": 2,

  "gauge.response.metrics": 89,

  "gauge.response.star-star": 11

}

=========================================================

II
) Pricing MicroService:

 

Try
mvn clean install from project root : pricingservice-app

Build
will get success.

 

Run
SpringBootPricingApplication.java as
Java Application

 

Assumptions:
 
Pricing service will have its own pricing
strategy for each product in Catalogue.Assuming price details for the
corresponding product available in Pricing database table already .(As
mentioned in exercise document , there is no sync up required between product
and Pricing services and databases)Fetching productname  from Catalogue service and using the same to
retrieve the price from pricing database I have used Spring security to protect but have not added
any specific roles for this POC. Also, I have created healthCheckService for ProductCatalogue
as well as for Pricing microservices. If we access pricing service without running the product
catalogue , then the healthCheckService will check the status and will throw
user defined message like ‘Product Catalogue
Service is Down and it is required to get the Price for the product ‘





        

 Steps to invoke
pricing service

Monitoring :

Added a HealthCheck Service to see
whether the service is up and running or not.

http://localhost:8081/v1/api/pricing/health

 



Note : Pricing
service runs on port number 8081

To run in different
port -> Open application.properties and change server.port to point to
your new port and run the Java program again

http://localhost:8081/v1/price/product/TV


Response :   price=20000.0

http://localhost:8081/v1/price/product/Mobile


Response :    price=11000.0

