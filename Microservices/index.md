### 2.  Introducing Microserve

2.1 What are microservices?

Microservies is an architechtural style where autonomouse, independently deployable services collaborate to form an aplication

Microservies architectures are often contrasted with monoliths. Monoliths is software application that typically has all of its code in a single code base which all developer collaborate together on that same source code repository.
"Monoliths"
Single codebase
Single process
Single host
Single database
Consistent technology

2.2 __Monolith Benefits__
- Simplicit
- One codebase
    - Easy to find things
- Deployment
    - One application to replicate
- Monoliths are not "wrong"

__The problem of Scale__

__Monolith Problems__
- Difficult to deploy
    - Risky (rủi ro)
    - Requires downtime
- Difficult to scale
    - Horizantal scaling often not possible
    - Vertical scaling is expensive
    - Whole application must be scaled
- Wedded to legacy technologiy
    - Reduces agility 

__Distributed Monoliths__

2.3 __Benefits of Microservies__
- Small service
    - Can be owned by a team
    - Easier to understand
    - Can be rewritten
- Technology Choise
    - Adopt new technology
    - Use the right tool
    - Standardize where it make sense
- Individual Deployment
    - Lower rick
    - Minimize downtime
    - Frequent updates
- Scaling
    - Scale servies individually
    - Cost effective
- Agility
    - Adapt rapidly
    - Easier reuse

2.4  __The challenges of Microservices__

- Developer Productivity
    - How to we make it easy for developers to be productive working on the system
- Complex interactions
    - Take care to avoid inefficient, chatty communications between microservices
- Deployment
    - You will need to automate the process
- Monitoring
    - we need a centralized place to check logs and monitor for problems

2.5 [Source Code](https://github.com/dotnet-architecture/eShopOnContainers)






