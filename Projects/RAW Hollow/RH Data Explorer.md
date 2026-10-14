`updateRecord`
```
mutation update($current: String!, $update: String!) {
  updateRecord(schemaName: "ModelFeature", update: $update, current: $current) 
}

{
  "current": "{\"id\":1, \"key\":\"account_cnt\",\"desc\":\"Number of Accounts\",\"blacklist\":true}",
  "update": "{\"id\":1, \"key\":\"account_cnt\",\"desc\":\"Number of Accounts\",\"blacklist\":true}"
}
```