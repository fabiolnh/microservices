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
- now only return 200 of the system, but also check the database and other dependencies
- self healing: a system that is not health, that is a great opportunity to recover if there are no requests for them (do not stop the pod, just stop to send requests to it)

3) Rate Limit:
- 
- 
