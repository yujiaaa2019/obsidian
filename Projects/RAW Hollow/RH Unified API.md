Writer API: 
1. Instantiation over the entire dataset 
2. from user perspective: 
	1. give a similar interface for development to interact with POJOs 
3. why can't use readerAPI 
	1. reader API has no info on Hollow Data Schema

Reader API: 
1. PerfAPI --> ordinal based lookup 
2. return type is an interface (either perfImpl or FlatRecordImpl)

proposal
1. toPojo() 

after reads (as perf/fr) --> call toPojo(Pojo.class) to supply to writer

Definition


shard is the spin app that houses multiple namespaces 
deployment: for a namespace, need the name of the namespace