explorer-control-plane 

```
type Query {
	getRHDataByDataType(namespace: String, env: String, dataType: String): DataType
	getSchema(namespace: String, env: String): 
}

type RHData {
	metadata: [RawHollow_NamespaceMetadata]
	schema: [HollowSchema]
}
type RawHollow_NamespaceMetadata {
	metadata: String 
	value: string
}

type PrimaryKey {
	keyFieldPath: value
}

type HollowSchema {
	int schemaId;
	HollowSchemaType schemaType;
	HollowSchemaName name;
	HollowCollectionSchemaData collectionData;
	HollowObjectSchemaData objectData;
}

type HollowSchemaFieldDefinition {
	HollowObjectSchemaFieldType fieldType;
	HollowObjectSchemaFieldName fieldName;
	HollowSchemaName referencedTypeName;
}



```

Control Plane GQL API
```
/controlplane/graphql
```
DGS 
```
type Query {
	getAllNamespaces: [RHNamespace]
}
type RHNamespace {
	name: String 
	owner: String 
	environment: String 
	vip: String
}
```


EARS GQL API
```
/{namespace}/graphql
```
DGS
```
type Query {
	getNamespaceDetail: [NamespaceDetail_{namespace}]
}
// auto generated depending on the APIDefinition
type NamespaceDetail_{namespace} {
	
}
```


BFF: avoid having the UI to talk to all of the GQL servers --> serve as a proxy
- have a Graphql server 
- serve as HTTP schema