```
grpc --local com.netflix.remoterepoloader.RemoteRepoLoaderService.GetClasses -d '{"repoName": "custom-rawhollow-transform-example", "repoProject": "SAE", "useGutenberg": false, "superClass": "HollowSinkConverter"}'
```

Q: how do we handle dependency
1. only look at dependency 
2. if using Jenkins build --> need to have several hooks 

how do you see logs of compilation operationally 


Data Mesh call the RH sink API to get the data record. 
RH can invoke custom transformation logic provider

server compiles the code if commit not present 
client gives the gutenberg topic; or tells the server to compile code. 

Alignment: 
1. users should only include necessary class files in the module they want as custom converter
2. only one converter for one namespace

