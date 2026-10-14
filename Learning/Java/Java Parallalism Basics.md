## Fork/Join Framework 
`Fork`: method.fork() == async method.compute()
- compute() is any computation method 
- fork creates a compute task
`join`: method.join() --> waits until the computation by fork finishes. 
- join waits for a specific task to finish 
- must call join() on every task created in the finish scope. 

ForkJoinPool --> a pool of Java threads. 