## Notes on Apache Kafta Course

### What is a Microservice? 
* Small autonomous Application
* Responsible for specific functionality (search, email notification, SMS notification). Can be a small standalone application or a very small building block of a larger application.
* Loosely coupled, designated to scale horizontally and work in the cloud. 

### Microservice VS Monolithic Application
* Monolithic Application: A single stream of application with multiple controls. Each controller is responsible for a certain domain.
* The controllers are part of one single application.
* Also works within one database
* If there are any changes made to a controller, you will need to re-build and re-deploy the whole application.
* Communication between the controllers is possible as they are all under one application.
  * Users Controller -> working with users
  * Products Controller -> working with products
  * Orders Controller -> working with orders

* Microservices
* Multiple smaller applications
* Each controller is responsible for its own functionality.
* Can work on different servers.
* Works within it's own database (database service pattern).
  * Users Microservice -> working with users resource (edit user data)
  * Products Microservice -> working with products 
  * Orders Microservice -> working with orders
*  If a microservice needs to get restarted, the products and orders microservice will not be affected and could still run.
*  In order to communicate between two microservices is by sending HTTP requests.
