# Microservices

## Basic concepts:

* Common Applications
* Defined Goals
* A part of an ecosystem
* They are independent and autonomous (the hard part)
* They communicate every time

## Monolithic VS Microservice:

* Monolithic: 
  - The whole application
  - In general: One Technology
  - If the app fails, the whole system fails
  - The whole team is working in the same system
  
* Microservice:
  - Various Technologies: some challenges can be used with specific technology in only one microservice, as an example.
  - Each service is a different deploy
  - If one service fails, only it fails
  - spread the people that are working on the project

## When should we use the Microservice and Monolithic?

Microservice:

* Usually we begin a system with a Monolithic strategy, until we can recognize 100% of the domain. Start with microservice only if there is a lot of money invested involved and you know the whole market
* If you want to scale your team
* if the company has a maturity process (CI, tests, cluster configured, etc)
* Context well defined, business area
* technical maturity of the team
* when we have a need of scale of a part of my system
* When we need a specific technology in a part of the system

Monolithic:

* when we create a POC
* new projects that we don´t know the whole domain and there are a lot of things that can change
* when we need a simple governance about technologies that will be used
* Ease to contract people
* Ease to train the developers
* Everything is in one place
* Clear share of libraries

OBS: There is no better or worse. Depends of each context

## How can we initiate the migration process of a monolithic to a microservice architecture?

1) Separate Contexts (Domain Driven Design - DDD)
2) Avoid the high granularity: Separate in a few services in the beginning
3) check dependencies between microservices (avoid distributed monolithic)
4) Plan the migration process of Databases. There is no "one day to other" that will become a microservice. Little by little is the migration.
5) Think about events (asynchronous). Don't be afraid of data duplication.
6) Think about Eventual Inconsistency between data in different databases (usual). Sometimes there is a time to reflect
7) Maturity in CD/CD, Tests and Environments
8) Begin with "edges". (don't begin with separating the core)
9) Strangler Pattern

## 9 Features of microservices (By Martin Fowler):

1) Components with services (substitutive, upgradable and deploy apart)
2) Organization by Business Areas (think in business areas of the company, not think in the whole company. Each team has all the knowledge and is focused in one business)
3) Products not projects (instead of treat the software as a project, treat it as a product, keeping the same team maintaining it, even when the software "is done")
4) Smart endpoints and dumb pipes (use http or messaging system: rabbitM, kafka, etc. You send the message and receive the same message, differently as it was a few years ago with ESB that had conversions and used to generate accouplement)
5) decentralized governance (avoid patterns, such as: technologies. with microservices, each service can have a different technology. Each team will manage it. The service communications have to have defined contracts)
6) decentralized data (each component has its own data. Sometimes we have some inconsistency, delay to update all the places)
7) infrastructure automatized (all the services have to have his own tests, security, deploy process (ci/cd), how much cpu and memory available, etc.). A lot of work. We have to be very mature before thinking about microservices.
8) draw to fail (we have to think in resiliency. It has to think in the worst situation)
9) evolutionary design (everytime that we can update or substitute the service without affect anything or others services)

## Resilience

- In some moment, the system will fail. We have to mitigate the risk.
- A group of strategies to intentionally adapt a system when a failure occurs.
- Strategies:

1) Protect and be protected: 
- Auto preservation, guarantees when a lot of requests get into the system, all the system will answer in the same manner (same response time). So: 1 million requests respond to each request in 500ms. If 2 million requests come and the response time increases to 1s to all the requests, you have to block these beyond requests, guaranteeing quality.
- A system cannot receive retries and retries if the system is down. 
- Sometimes a slow system is worst than a system offline (it generates a chaining)

2) Health Check: 
- do no return only the health check of the system, but also check the database and other dependencies
- self healing: a system that is not health, there is a great opportunity to recover if there are no requests for them (do not stop the pod, just stop sending requests to it)

3) Rate Limiting:
- Protect the system based on what the system was proposed to accept (ex: 100 requests per second)
- custom protection per client (groups of preferences for rate limit per client)

4) Circuit Breaker: 
- protect the system to deny some requests
- Opened Circuit: all the request deliver normally
- Closed Circuit: requests are not delivered to the system.
- Half Opened Circuit: a limited request is allowed to verify if the system has a condition to come back online.
- The best way to implement is using Service Mesh (such as: Istio)

5) API Gateway
- One authentication for the rest of the system
- Rate Limit, Health check (return an error if the /health is down), etc
- avoid chaotic communications: when you have a request getting into a service A and this service send this request to a service B, change it and put the request directly into api gateway to the service B

6) Service Mesh
- Proxies: Service A sends the request to Proxy A and the proxy A sends the request to proxy B and the proxy B send the request to Service B
- Control the whole system, the whole communication
- protect the services with mTLS communication (secure)
- Circuit Breaker, retry, timeout, fault injection, etc.
- Instead of the developer implement it, give the responsibility to other person to control the whole system
- Ex: Istio

7) Async
- send the message to a broker and the broker, the broker keeps the message and when the destination service consumes the message in its time.
- Ex: Kafka, RabbitMQ, etc.
- There is not data lost
- The server can process the information in its time.

8) Retry
- Without Backoff: retry in the Same time every time
- Exponential Backoff: the time will increase each time in fails (with less requests)
- Exponential Backoff with Jitter: the time will increase each time in fails (with less requests), however, the time will change a little bit with a random number, this way all the services will do requests in a different time

9) Delivery Guarantee
- In Kafka, there are:
  * Ack 0 (None): No Confirmation (high performance)
  * Ack 1 (Leader): Confirmation that the Leader has get the message
  * Ack -1 (ALL): Confirmation that the Leader and the Followers got the message (less performance) 
  
10) Complex Situations:
  * What happens if a message broker goes down? 
  * Will it lose data? 
  * Will the whole system go offline?
  * How to guarantee resiliency?
- Transactional Outbox: Save the message in a Database before sending it to kafka, and, when sent and the kafka guarantee that it was received, delete it.

11) Receiving Guarantee (Resiliency)
- Never work with auto ack
  * Auto Ack: use it false and manual commit (first processed the message and just later we send a message to the broker that it can be deleted)
- Prefetch aligned with volumetry: receive the message as a batch, not one by one. However, it is good to do a stress test with volumetry, to see how much the system can support 

12) Idempotence and Fallback Policies
- Idempotence: how we can handle a duplicity message situation. We have to think in this situation. (ex: id)
- Fallback Policy: control the possible of wrong situations

13) Observability
- APM (Application Performance Monitor)
- Distributed Tracing
- Custom Metrics (system metrics and business metrics)
- Personalized Spans (each part of the system we can see details)
- Open Telemetry


## Choreography VS Orchestration
1) Choreography: 
  - Death Star: When a lot of microservice are communicating with each other generating a complex requests communication
  - ![](https://1.bp.blogspot.com/-pFb1j0gLUXw/W5_GpjLMdyI/AAAAAAAADVw/lo_L8SRLWlw3VDmlfQPJx2fk176Vv-n5QCLcBGAs/s1600/004_Death%2BStart%2BPitfall.png)
2) Orchestration: Guarantee an order between calls. Ex: a -> B -> C -> D
  - There is a mediator that controls all the flux of each communication (that can be async or sync) get success or fail, if fail, it generates a fallback plan (what happens when a failed communication occurs. As an example: undo all the things of past services did)
  - It is more complex than Choreography

## Strategy of Choreography:
- A way to group the micro services into contexts
- Especially if the communication is sync
- Create a "Micro" API Gateway to each Context. The communication occurs between api gateways for each context (It is not a service mesh)
- Ex: Microservice A and Microservice B belong to Context X - API Gateway X (/payment). 
- The other services (belonging to other contexts, other api gateways) that call these two services go to the API Gateway first, and the API Gateway knows where to redirect.
- This way we can organize better the Choreography

## Patterns

1) API Composition: Possibilities:
  - a) Service Composer: Instead of a Front-end (or other service) calls Service A and Service B to generate a report, as an example, it can call a service C, and the Service C calls Service A and B
  - b) API Gateway: The same example of above, but instead use a Service C, you can use an API Gateway
* Disadvantages:
  - You can have a unavailable problem
  - Data Consistency
  - High Complexity (create a service to read other services)
  - High Latency
  - Sync Way

2) Decompose By Business Capability
  - Think in DDD (Think in business contexts)
  - Ex: Decompose a monolithic system into microservices. Ex: each module will be a business area (microservice)

3) Strangler Application
  - Slowly (a long time), we decrease the monolithic, doing:
    * Each feature will be a microservice (new ones)
    * little parts of the monolithic system will be a microservice
  - Attention Points:
    * We have to think in the communication with the monolithic and the microservices while it is being decrease
    * The company has to have the devops culture
    * At the beginning, we can have a shared database (evaluate)
    * Tip: Use APM (Ex: New Relic / Data Dog): It will show all the queries that the system is doing
    * Each microservice will have its own APM
    * What metrics do you want? what happens differently the expected will have to have alarm. 

4) ACL (Anti Corruption Layer)
  - Create a microservice in the middle, between the microservice A and the microservices B,C,D, doing a "proxy/interface" to decide to which microservice it will communicate, this way we do not corrupt the microservice A, we do not modify it when we need to communicate to B, C or D
  - there are people that treat the "ACL" inside the application, but it is not the right way in this context.
  
5) API Gateway
- A very important pattern. Can be: Stateless and Stateful. Ex: Kong
  * Redirect to certain microservice
  * Rate Limit
  * Can modify the message
  * Authentication
  * Etc.
  
6) BFF (Backend for Frontend)
  - To each different client, we have a different payload (Ex: a BFF for mobile client, a BFF for TV client, a BFF for Desktop clients)
  - We create a microservice for that
  * There is a solution that can substitute the BFF: GraphQL (in GraphQL, who dominates the backend is the client. So, this way, the client can request only the information it needs). Sometimes it is not so simple to work with GraphQL
  
7) 
