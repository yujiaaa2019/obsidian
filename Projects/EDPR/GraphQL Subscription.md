## Testing 
```
 metatron curl -timeout 100s -v -a dgwhollow.coalesce  -H "Accept: text/event-stream" -H "Content-type: application/json" https://dgwhollow.coalesce-explorer.bcbd.cluster.us-east-1.test.cloud.netflix.net:8443/graphql -d '{"query": "subscription ListenForDataChange {listenForDataChange(schemaName: \"InstanceDetail\") {schemaName json}}"}'
```

```
 metatron curl -timeout 100s -v -a dgwhollow.cynquickstart  -H "Accept: text/event-stream" -H "Content-type: application/json" https://dgwhollow.cynquickstart-explorer.ce32.cluster.us-east-1.test.cloud.netflix.net:8443/graphql -d '{"query": "subscription ListenForDataChange {listenForDataChange(schemaName: \"Movie\") {schemaName json}}"}'
```

```
metatron curl -timeout 200s -v -a dgwhollow.cynquickstart  -H "Accept: text/event-stream" -H "Content-type: application/json"  https://cyn-quickstart.explorer.dgwhollow.vip.us-east-1.test.cloud.netflix.net:8443/graphql -d '{"query": "subscription ListenForDataChange {listenForDataChange(schemaName: \"Movie\") {schemaName json}}"}'
```

local 
```
metatron curl -timeout 200s -v -a rawhollow -H "Accept: text/event-stream" -H "Content-type: application/json" https://localhost:7004/graphql -d '{"query": "subscription {listenForTopLevelTypesChange}"}'
```

## getUpdatesInRange
```metatron curl -v -a dgwhollow.cindercloudserviceurlstatesharer  -H "Content-type: application/json"  https://urlstatesharer.explorer.dgwhollow.vip.us-east-1.test.cloud.netflix.net:8443/graphql -d '{
    "query": "\n    query GetUpdatesSinceTime($schemaName: String!, $since: DateTime!) {\n  getUpdatesSinceTime(schemaName: $schemaName, since: $since) {\n    edges {\n      cursor\n      node {\n        schemaName\n        json\n primaryKeys\n operation\n     }\n    }\n    pageInfo {\n      hasNextPage\n      hasPreviousPage\n      startCursor\n      endCursor\n    }\n  }\n}\n    ",
    "variables":
{"schemaName": "URLState", "since": "2025-05-02T06:17:00.000Z"}

}'
```