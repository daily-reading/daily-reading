# Taming Service-Oriented Architecture using a Data-Oriented Service Mesh

##1. Introduction
* The main goal is to improve modularity of Airbnb's architecture.
* Service-Oriented architecture results in large number of small microservices with complex dependency graph structure. (some one has to do a topology sort at some point. The idea of ViaDuct is that, dependecy information should be handled by the architecture rather than consumers.)
* The author thinks complex dependency graph is conceptually similar to spaghetti code, and Aribnb's remedy to this spaghetti SOA is a concept called "data-oriented service mesh"

##2. Data-Oriented Service Mesh
* ViaDuct is Aribnb's solution. It uses GraphQL and the "schema" consists of Types (data), Queries, and Mutations.
* Type is essentially an "interface" for data, and consumer of a given Type should not care about where the data comes from. 
* The "central schema" then could become the only place where all microservice dependency/API lives, (which hopefully drastically improves the modularity, and reduce telepathic/hidden assumption/dependencies. )
* Once "central schema" is well-defined (particularly if they are "typesafe"), a lot of the "derived-data" services becomes functional calls, or "cloud functions" as the author calls them. This means that they could be refactor into stateless containers.