Q:why?
A:to support GraphQl in Java ecosystem (Spring Boot). and it *happens* to be a good candicate to open source :-D

Schema-first development
1. schema first
2. code first
note: in code first, schema is generated during runtime. Eventually, it's still schema first.

pros of schema first
1. better experience of developer. Nobody wants schema generated during runtime. Hard to debug, unstable, low perf.
2. easy to consume the schema
3. backward compatiable.

Framework in action
1. annotation based
2. easy testing, no http required.

Federation
1. gateway generates a query plan, and calls underlying services to construct the return value.

Architecture
1. as the picture describes