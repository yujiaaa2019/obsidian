[Slides](https://docs.google.com/presentation/d/1jaXRXzlP6ZAQAWyDqYwEe1gI4jUh5VfugA2skxnd4jI/edit#slide=id.g75bcc282fd_0_77) 
How HollowSQL executes query: 
![[Screenshot 2023-10-16 at 10.03.24 AM.png]]
RowSet: contains the data through the query 

![[Screenshot 2023-10-16 at 10.09.03 AM.png]]
API center: QueryEngine
pre-execution: 
- make things non volatile for the subsequent query 
- execution need to be atomic volatile 
QueryContext: 
- an object that all the query executions could access 

<mark class="hltr-pink">Autocomplete</mark>
antlr4-c3 for predictive forward scanning for suggestions about lllexer keywords like "where" or "as" 
