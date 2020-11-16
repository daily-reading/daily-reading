[Article](https://medium.com/airbnb-engineering/taming-service-oriented-architecture-using-a-data-oriented-service-mesh-da771a841344) Taming Service-Oriented Architecture Using A Data-Oriented Service Mesh

### Why service mesh?

- Ease the complexity for API updates and breaking changes
- Increase maintainability
- Hide more invocation details and let engineers focus more on function development 
- Gain clearer insight on how microservices cooperate with each other

Current industry standard for service meshes is to organize exclusively around remote procedure invocations without knowing anything about the data that makes up the application architecture

### Why data-oriented service mesh?

To replace these procedure-oriented service meshes with service meshes organized around **data**

- Integrate multiple provider-consumer dependencies among microservices to one common dependency to data-oriented service mesh
- Increase data agility that a change to a database schema would not need to reflect its change through layers of architecture and layers of microservices. Propagating change like this will only need a single update by exploiting the data-oriented service mesh

After reading this article, it is hard to imagine their architecture. May be they have a completely different layout that their microservices are build around data as well. Otherwise, I cannot figure out a way to put the data-oriented service mesh over microservices splitted according to the DDD principle (like in our company).

