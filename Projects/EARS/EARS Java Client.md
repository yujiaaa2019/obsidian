EARS walkthrough 
- graphservice is not generic --> need to create POJO 
	- biggest gap --> dynamic 
- EARS is a Managed infra (not IB to graph nor Cinder to graph only)
- graphql java engine has lots of overhead 
	- large payload --> java engine that runs and validates process longer
	- graphqlservice is a lot more lightweight than graphql java 
- use graph search for backend call
	- map a graphaql service to a graphql execution 

graphql --> declarative schema def 