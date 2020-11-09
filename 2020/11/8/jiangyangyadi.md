[Article](https://aws.amazon.com/blogs/architecture/architecting-for-reliable-scalability/) Architecting for Reliable Scalability

#### “Build Today with Tomorrow in Mind” 

I really need to keep this in mind. Not only for cloud solution architects, any ambitious engineer, I reckon, should have these words as the principle in desigin. After all, software engineering is all about to be resilient against the changing tides of time. We always need to face evolutions and have to reconstruct to bring into changes and tolerate the consequent complexity time to time.

#### What we don't want

- Additional complexity
- Degraded performance
- Reduced security
- Unstabel up time

#### What might help

- Modularity is one of the key to isolate system risks by loose coupling. It is througn pre-defined commin APIs to integrate units that are less complcated and easier to scale, secure and manage. Unlike in the monolithic architecture or tight-coupled code pieces where they might have troublesome chained risks, modularity ensures that risks can be classified into different levels and we have different levels of testing to manage differnet lecel of risk. 
- Horizontal scaling, AKA scale-out, is to handle an increase / decrease in load in a automatic and distributed manner. To support seamless scale-out, software needs to support a stateless scaling model which means to have its state information stored and requested independently from its instances. It is also a matter of modularity.
- Taking into security requirements into consideration in the intial solution design is essential. This will allow you to add more security capabilities and features as the solution grows, instead of compromising with suboptimal solutions or making major changes at later stages.
- Reliability comes from the resilience of the system, the capability to recover from infrastructure or service disruption. When design the structure of a service, we need to think about how to manage the impact magnitude of all kinds to failure so to have some functions still work when one or more components fail.

