# Build 
covers all the steps required to create a "deliverable" of your project. 
1. Generate sources (sometimes)
2. Compile sources 
3. Compile test sources 
4. Execute tests 
5. Packaging (jar)
6. Run health checks 
7. Generate Project

# Compile 
Java compiler checks for syntax errors and type correctness but does not verify the runtime configuration of Spring beans. 

# Error Handling 
### Static Error
Errors during compile time 

# Layered Architecture
1. Presentation Layer (API Layer)
	1. This layer is responsible for handling HTTP connections from clients and is used to handle GraphQL and gRPC endpoints. It receives user requests and sends responses. This layer will only handle communication and not have any business logic.
2. Business Layer (Service Layer)
	1. This layer contains the core business logic. This is where you consolidate similar operations that are shared between gRPC and GraphQL requests. This could include validation, request processing, data transformation etc. If there is any action that is needed by both gRPC and GraphQL requests, keep it in this layer and not inside your API layer. This will allow your APIs to call these shared functions in a convenient manner.
3. Data Access Layer (DAL)
	1. This layer deals with persistent storage, i.e., interaction with your database. This includes operations like create, read, update, and delete (CRUD operations). Here, you'll define your data entities as per the underlying database schema and write methods to operate on these entities.
