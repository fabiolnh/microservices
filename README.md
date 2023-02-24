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

## When we should use the Microservice and Monolithic?

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
4) Plan the migration process of Databases. There is no "one day to other" will become microservice. Little by little is the migration.
5) Think about events (asynchronous). Don´t be afraid of data dupplication.
6) Think about Eventual Inconsistency between data in a different databases (usual). Sometimes there is a time to reflect
7) Maturity in CD/CD, Tests and Environments
8) Begin with "edges". (don't begin with separating the core)
9) Strangler Pattern
