Microservice or Monolith?
A: depends on project complexity.

1. domain driven design
Note: the key is tto split application correctly into modules. Otherwise, it will have the disadvantags of monolith and complexity of microservice (do it right, otherwise it's worse than not doing anything). Need to read the reference for more details. Ideally, microservice should be mapped to domains.

2. database per microservice.
Note: IT DEPENDS. Sharding DB is against the principal of microservices but each microservice has its own DB will cause a huge issue for syncing data. Author doesnt give an ultimate answer here, but the author's opinion is to assign DB to each microservice.

3. micro frotend
Note: I remember there was an article about this earlier. Several implementations.

4. Contoinuous Delivery
Note: CD. Nothing fancy, everyone knows.

5. Observability
Note: My understanding is logging. Microservice would be super difficult for debugging and investigation. It would be good to get prepared and have good logging system.

6. Unified Tech Stack
Note: Use the right choice of tech stack. Really depends on experience.

7. Asynchronous Communication
Note: Communication between microservices should be async to avoid complexity. e.g. I am using service bus.

8. Microservice first
Note: Do the right thing from the beginning

9. Infra over Lib
Note: Unless the projects are super similiar, it would be good to start with what is really needed.

10. Org considerations
Note: Conway's law