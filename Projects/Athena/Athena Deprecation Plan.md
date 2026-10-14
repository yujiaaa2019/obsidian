Current State: 
1. search page -- hitch API 
2. 28DS dashboard 
3. Watch list 
4. presonnel prediction 
5. title insights 

Next Steps: 
1. identitfy the coverage of Ruby backend 
2. change the API for user authentication from Ruby to Hawkins component
3. change the API for buyer information from Ruby to GQL API 
4. remove Ruby prediction context name mapping


## search Users
```
query($pandoraQueries:[USRSearchQuery]) {
  searchUsers(searchRequest: {
    queries:$pandoraQueries
  }) {
    edges{
      node {
        userId
        userName
        primaryEmail
        fullName
      }
    }
  }
}


{
  "pandoraQueries": [
    {
      "text": "andrew mc",
      "fields": [
        "emails.address",
        "name.fullName",
        "userName"
      ],
      "filters": [
        {
          "field": "status",
          "operator": "EQUALS",
          "values": ["ENABLED"]
        }
      ]
    }
  ]
}
```