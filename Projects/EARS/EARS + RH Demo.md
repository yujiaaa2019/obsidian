
portarying the Managed aspect of the tools
focus on lowcode 

have repo 
1. Generate API, wiring, 

Add custom things live
also supports federation 
- use Actor 

-------
set up:
1. cbp-demo-programming-category 
2. cbp-demo-movie (reset from oscar_entity namespace that has Movie and MovieType)
3. Programming Category Data Type
demo:
assignProgrammingCategoryToMovie(movieId, pcId) 
1. create Hollow Data Type (ProgrammingCategoryMovieMap)
2. run GenerateAPI



For implementation details: we could go offline 

-----
Scripts: 
Today we will showcase how we could create transactional APIs using EARS and RAW Hollow. Say for example a team would like to have Programming Category exposed on graph, perform basic read, write, delete operations on it, and associate with specified Movie entites. To achieve that, we can create a new EARS usecase. 

A EARS usecase consists of the GraphQL schema definition files, a config file for RAW Hollow namespace declaration, and if needed, some code for business logic. 

I forked a starter repo called ears-demo, and 