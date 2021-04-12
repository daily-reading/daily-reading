# 提交说明
- 编号：NO.14@20210409
- 文章：[The Uber Engineering Tech Stack](https://eng.uber.com/tech-stack-part-one-foundation/)

# 读书笔记
## Bottom
- hybrid cloud model
  - > Our business runs on a hybrid cloud model, using a mix of cloud providers and multiple active data centers. If one data center fails, trips (and all the services associated with trips) fail over to another one.
- storage
  - > Schemaless instances replace individual MySQL and Postgres instances
  - > Cassandra replaces Riak for speed and performance
  - > For distributed storage and analytics for complex data, we use a Hadoop warehouse
  - > Beyond these databases, our Seattle engineers focus on building a new real-time data platform.
  - Redis
    - > We use Redis for both caching and queuing
    - > Twemproxy provides scalability of the caching layer without sacrificing cache hit rate via its consistent hashing algorithm
- log
  - Kafka
  - ELK
- App Provisioning
  - Docker 
  - mesos/aurora
- Routing and Service Discovery
  - HAProxy
  - Hyperbahn: is part of a collection of open source software developed at Uber: Ringpop, TChannel, and Hyperbahn all share a common mission to add automation, intelligence, and performance to a network of services.
  - nginx
  - thrift protobuf
  - HTTP/2 SPDY
- Development and Deploy
  - Phabricator: powers a lot of internal operations, from code review to documentation to process automation
  - OpenGrok
  - internal deployment system
  - jenkins
  - We combined Packer, Vagrant, Boto, and Unison to create tools for building, managing, and developing on virtual machines.
  - We use Clusto for inventory management in development
  - Puppet manages system configuration
  - We constantly work to build and maintain stable communication channels, not just for our services but also for our engineers
  - We use an in-house documentation site that autobuilds docs from repositories using Sphinx
- Languages
  - Java/Go: Java takes advantage of the open source ecosystem and integrates with external technologies, like Hadoop and other analytics tools. Go gives us efficiency, simplicity, and runtime speed.
- Testing
  - Hailstorm drives integration tests and simulates peak load during off-peak times
  - uDestroy intentionally breaks things so we can get better at handling unexpected failures.
- Reliability
  - We use Nagios alerting for monitoring, tied to an alerting system for notifications.
  - SRE
- Observability
  - a collection of systems acts as the eyes, ears, and immune system of Uber Engineering around the world.
  - Grafana
  - Argos, our in-house anomaly detection tool
  - Acting on Metrics: Uber’s μMonitor tool enables engineers to view this information and thresholding (either static or Argos’s smart thresholding) and take action on it. 
- Using Data Creatively
  - storm/spark
  - visualization

## Middle
- Uber’s core trip execution engine
  - > Uber’s core trip execution engine was originally written in Node.js because of its asynchronous primitives and simple, single-threaded processing. (In fact, we were one of the first two companies to deploy Node.js in production.) Node.js gives us the ability to manage large quantities of concurrent connections. We’ve now written many services in Go, and this number continues to increase. We like Go for its concurrency, efficiency, and type-safe operations.
- The Edge
  - > Engineers on this team use the open source module logtron to log to disk and Kafka. We generate stats using the uber-statsd-client module (the Node.js client for statsd), which talks to our in-house M3 (previously described).
- Highly Available, Self-Healing, Persistent
  - >  These teams use Ringpop and Sevnup for cooperation and shifting of object ownership when a node in a hashring goes down, or when another node takes ownership of the keyspace.

## Top
### web
- language
  - > The core of our web tech stack is built on top of Node.js, which has a large and vibrant community of web engineers. Node.js allows us to share JavaScript code between client and server to create universal (isomorphic) web applications. 
  - Browserify
- Web Server
  -  Express.js
        - > which has a set of default middleware to provide security, internationalization, and other Uber-specific pieces that handle infrastructure integration.
  - Atreyu
    - > Our in-house service communication layer, Atreyu, handles communication with backend services and integrates with Bedrock.
- Rendering, State Handling, and Building
  - > We use React.js and standard Flux for our application rendering and state handling, though a few teams have been experimenting with Redux as our future state container. 
  - > Our build system, Core Tasks, is a standard set of scripts to compile and version front-end assets, created on top of Gulp.js, that publishes to the file storage web service, allowing us to take advantage of a CDN service.
  - > Finally, we use an internal NPM registry to access the huge collection of public-registry packages as well as publish internal-only packages.
### mobile
- development
  - > Mobile development is one hundred percent trunk development and train releases.
  - > The platform uses feature flags to enable and disable code pass from the server side. We launch dark, turn it on, and monitor the rollout closely.
  - > Engineers don’t need to think about the build train; they just land incrementally behind the feature flag. Rather than a manual QA process, we invest heavily in automation and monitoring. Our continuous integration and enforcement keeps us scaling fast, and monitoring lets auto-responsive systems catch and correct any flawed commits.


# 参考链接
- [SOA架构](https://eng.uber.com/service-oriented-architecture/)
- [Scalable Datastore Using MySQL](https://eng.uber.com/schemaless-part-one-mysql-datastore/)
- [Terraform](https://www.terraform.io/) : Terraform is an open-source infrastructure as code software tool that provides a consistent CLI workflow to manage hundreds of cloud services
- [Data-Migration](https://eng.uber.com/mezzanine-codebase-data-migration/): migrate hundreds of millions of rows of data and more than 100 services over several weeks
- [encoding-compression](https://eng.uber.com/trip-data-squeeze-json-encoding-compression/): Uber Engineering’s comprehensive encoding protocol and compression algorithm
- [postgresql](https://www.postgresql.org/)
- [mesos](http://mesos.apache.org/): Program against your datacenter like it’s a single pool of resources
- [aurora](http://aurora.apache.org/): Aurora is a Mesos framework for long-running services and cron jobs.
- [phabricator](https://github.com/phacility/phabricator): Phabricator is a collection of web applications which help software companies build better software
- [uber's internal deployment system](https://eng.uber.com/micro-deploy-code/)
- [go in uber](https://eng.uber.com/go-geofence-highest-query-per-second-service/)
- [Rewriting Uber Engineering: The Opportunities Microservices Provide](https://eng.uber.com/building-tincup-microservice-implementation/)
- [chaos-monkey](https://blog.codinghorror.com/working-with-the-chaos-monkey/)
- [stack-history](https://stackshare.io/stack-history-timeline-uber-tech-stack-evolution)
- [nagios](https://www.nagios.org/)
- [uber SRE](https://eng.uber.com/site-reliability-engineering-talks-feb-2016/)
- [the history of Uber Site Reliability Engineering](https://www.youtube.com/watch?v=qJnS-EfIIIE&t=4s) : A February 2016 tech talk delineates the history of Uber Site Reliability Engineering.
- [GrafanaCon 2015: Scaling Monitoring at Uber](https://www.youtube.com/watch?v=89H48IwFeV4)
- [Argos, uber in-house anomaly detection tool,](https://eng.uber.com/argos-real-time-alerts/)
- [flask](https://flask.palletsprojects.com/en/1.1.x/)
- [uWSGI](https://uwsgi-docs.readthedocs.io/en/latest/index.html#)
- [Browserify](https://en.wikipedia.org/wiki/Browserify): Browserify is an open-source JavaScript tool that allows developers to write Node.js-style modules that compile for use in the browser
- [expressjs](http://expressjs.com/)
- [relay](https://github.com/facebook/relay)
- [falcor](https://github.com/Netflix/falcor)
- [mvc-vs-flux-vs-redux](https://www.clariontech.com/blog/mvc-vs-flux-vs-redux-the-real-differences)
- [feature-flag](https://featureflags.io/feature-flags/)
- []()

