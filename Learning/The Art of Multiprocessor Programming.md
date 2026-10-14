# Introduction 
## 1.2 Multual exclusion protocol 
Multual exclusion protocol has 2 properties: 
1. mutual exclusion 
2. deadlock-freedom 

1. mutual exclusion -- safety property 
	1. a must 
2. deadlock-freedom -- liveness property 
	1. liveness: a good thing to have 
3. starvation-freedom / lockout-freedom 
	1. will the satisfaction eventually be met?
4. waiting 
	1. what happens if one side fails and the other side have to wait for a long time 

## 1.2.2 Producer-Consumer protocol 
Producer-consumer has 3 properties: 
1. mutual exclusion 
2. starvation-freedom 
3. producer-consumer
Two types of communications: 
1. Transient communication: requires both parties to participate at the same time 
2. Persistent communication: allows the sender and receiver to participate at different times 

