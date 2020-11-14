### 2. Introducing Microserve

2.1 What are microservices?

Microservies is an architechtural style where autonomouse, independently deployable services collaborate to form an aplication

Microservies architectures are often contrasted with monoliths. Monoliths is software application that typically has all of its code in a single code base which all developer collaborate together on that same source code repository.
"Monoliths"
Single codebase
Single process
Single host
Single database
Consistent technology

2.2 **Monolith Benefits**

- Simplicit
- One codebase
  - Easy to find things
- Deployment
  - One application to replicate
- Monoliths are not "wrong"

**The problem of Scale**

**Monolith Problems**

- Difficult to deploy
  - Risky (rủi ro)
  - Requires downtime
- Difficult to scale
  - Horizantal scaling often not possible
  - Vertical scaling is expensive
  - Whole application must be scaled
- Wedded to legacy technologiy
  - Reduces agility

**Distributed Monoliths**

2.3 **Benefits of Microservies**

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
- Agility - Adapt rapidly - Easier reuse

  2.4 **The challenges of Microservices**

- Developer Productivity
  - How to we make it easy for developers to be productive working on the system
- Complex interactions
  - Take care to avoid inefficient, chatty communications between microservices
- Deployment
  - You will need to automate the process
- Monitoring - we need a centralized place to check logs and monitor for problems

  2.5 [Source Code](https://github.com/dotnet-architecture/eShopOnContainers)

### 3. Architecting Microservices

3.1 **Evolving Towards Microservices(Hướng phát triển microservies)**

- Augment a monolith
  - Add new microservies
- Decompose(phân tích ra) a monolith
  - Extract microservies
- Don't need to start with microservices
  - It's hard to get servies boundaries(danh giới) right
- Defining microservies responsibilities - public interface

  3.2 **Microservices own their own data (Microservies sở hữu dữ liệu của chính nó)**

- **Avoid** sharing a data store

**Mitigating data Ownership Limiteations**

- Define servie boundaries well
  - Minimize the need to aggregate data
- Caching
  - Improved performance
  - Improved availability
- Identity "seams" in the database

  3.3 **Component of Microservice**

  3.4 **Microservice Contracts**

- Make additive changes
  - New endpoints
  - New properties on DTOs
- Introduce version 2 API
  - Version 1 clients must still be supported
- Easily forgotten in development

  3.5 **Avoiding upgrade Issues**

- Team ownership of microservices
  - First, add a new capability
  - Then deploy updated microservice
  - Later, update clients
- Create automated tests
  - Ensure that older clients are supported
  - Run as part of a CL build process
- Beware of shared code - Can result in tight coupling

  3.6 **Indentifying Microservies Boundaries**

- Getting it wrong can be costly
  - Poor performance
  - Hard to change
- Start from an existing system
  - Identify loosely coupled components (xác định các thành phần liên kết lỏng lẻo)
  - Identify database seams (Xác định đường nối cơ sở dữ liệu)

**Domain Driven Design**

- Identify "bounded contexts"
  - Break up large domains
  - "Ubiquitous language (Phổ cập)"
  - Microservies don't share models
- Sketch your ideas on a whiteboard
  - Run them through real-world use cases
  - Indentify potentical problems

**Microservie Boundary pitfalls (cạm bẫy)**

- Don't turn every noun into a microservices
  - Anemic CRUD microservices
  - Thin wrappers around database
  - Logic distributed elsewhere
- Avoid circular dependencies
- Avoid chatty communications

**Summary**

- Evolving towards microservices
- Microservices own their data
- Microservices may consist of multiple processes
- Microservices should be independently deployable
- Avoid breaking changes
- Indentify "bounded contexts" for microservice boundaries

#### Buiding Microservies

##### Microsoft Template

    - Logging
    - Health checks
    - Configuration
    - Authentication
    - Build scripts

    Benefits of Service Template
        - Reduced time to create a new microservice
        - Consistent tooling
        - Increased developer productivity
        - Ability to run the microservice in isolation
        - Run in context of full application

**Summary**

- Hosting microservices
- Using containers
- Source control and build
- Automated tests
  - Unit tests
  - Service level tests
  - End-to-end tests
- Standardizing(chuẩn hóa) microservices
  - Service template
