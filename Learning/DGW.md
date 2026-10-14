Data Gateway === API Gateway
`A platform for running Data Abstraction Layers (DALs)` 
![[Screenshot 2024-02-29 at 2.43.09 PM.png]]
## DAL 
Allows applications to choose to couple with using gRPC/REST data interface and with other Datastore APIs (like SQL, ES JSON DSL, CQL...) 
Benefits: `Decouples applications from hard to maintain and backwards-incompatible Datastore APIs`

### Example DALs
#### KeyValue (KVDAL)
"Hashmap as a service"
#### Entity Service (EntityDAL oldname: MES)
Feature-rich data abstraction for Entity-centric persistence

