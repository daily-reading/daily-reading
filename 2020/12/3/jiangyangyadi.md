[Artical](https://medium.com/airbnb-engineering/introducing-deploy-pipelines-to-airbnb-fc804ac2a157) Introducing Pipelines to Airbnb’s Deployment Process

Many reasons could cause a downtime of applications. As almost all services are shipped to the production throught the deployment service, the role of deployment pipeline, or said the "CI/CD" procedure, is the **last line of defense** against the occurence of downtimes. 

### We Want Less Mistakes

#### Intuitive Design

The deployment system has to be intuitive that makes it easy to use and reduces potential misuses as many as possible. However, it is sometimes hard to reach this point that oftenly what seems intuitive to the developer of the system is less thant intuitive for the users.

- Gaowei posted a comment with a similar point of view under the RFC about our branch workflow model, saying "An counter-intuitive design of a system would make user more prone to error, needless to say human beings are already apt to make mistakes".

#### Minimize Manual Operations

We cannot rely on the exprierence or familiarity of our engineers over the deployment procedure, taking in consideration of how crucial this stage is. We want to ensure all parts collaborating well by carefully designed, standardized, visualized and simple system, that minimizes manual operations as many as possible.

### Difficulty

To Accommodate Differences into Standard Interface while Keep Flexibility

- Each service has its own set of "environments".
- Different deployment procedures are required by teams or set of services.
- However, freedom could rise problems, for examples when users directly push their commits to the production or when users are trying to deploy a service they do not realy familiar with.

### Solution

1. Develop a specification on how service owners to define their deployment pipelines, with madatory / optional options.
   - Consider "Configuration As Code"
2. Save pipeline configurations in a database.
3. Validate pipeline configurations
   - Should be a DAG that has no circular dependency
4. Disable targets that shouldn’t be deployed to
5. A beautiful, ordered, and intuitive UI. **IMPORTANT**
