## Overview
Studio Edge is a cross-functional project aiming to provide Content Engineering a platform for content entity retrieval.

![[Screenshot 2023-08-02 at 3.12.37 PM.png]]

## DGS
Domain Graph Service 
#### Ongoing Exploration: Connecting DSE Entities onto StudioEdge
- DSE *produces* data in DataWarehouse that are of interest to many studio apps

## Federation 
allows one to `extend` or `reference` existing types in a graph. 
You DGS fulfills a part of the query based on the schema that is `owned` by your DGS, the the gateway is responsible for fetching data from other DGSs
- example: 
	- CISLContentSubRegion is `owned` by contentinsights DGS
	- in cindercloudservice_contentinsights DGS, we could federate CISLContentSubRegion by doing this: 
```
type CISLContentSubRegion @key(fields: "id") @key(fields: "code") @extends {  
id: ID! @external  
code: String! @external  
}		
```
- what happens now is that, the query will run through cindercloudservice_contentinsights DGS first, and the gateway will hydrate the `description` field in CISLContentSubRegion through contentinsights DGS. 



### @key

### @extend

### @override
