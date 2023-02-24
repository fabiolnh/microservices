# Microservices

## Basic concepts:

* Common Applications
* Defined Goals
* A part of an ecosystem
* They are independent and autonomous (the hard part)
* They communicate every time

## Monolytic VS Microservice:

* Monolytic: 
  - The whole application
  - In general: One Technology
  - If the app fails, the whole system fails
  - The whole team is working in the same system
  
* Microservice:
  - Various Technologies: some challenges can be used with specific technology in only one microservice, as an example.
  - Each service is a different deploy
  - If one service fails, only it fails
  - spread the people that are working on the project

## When we should use the Microservice and Monolytic?

Microservice:

* Usually we begin a system with a monolytic strategy, until we can recognize 100% of the domain. Start with microservice only if there is a lot of money invested involved and you know the whole market
* If you want to scale your team
* if the company has a maturity process (CI, tests, cluster configured, etc)
* Context well defined, business area
* technical maturity of the team
* when we have a need of scale of a part of my system
