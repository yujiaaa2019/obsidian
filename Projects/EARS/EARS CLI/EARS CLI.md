# Newt 
[remote builds](https://manuals.netflix.net/view/newt/mkdocs/master/user-guides/remote-builds/additional-features/#local-mounts)
- give network ip to laptop 


No Code -- auto rebuild 
Low Code -- disable auto rebuild 

python file monitor 


[A good example doc for local debug](https://manuals.netflix.net/view/cosmosguide/mkdocs/master/developing-your-service/plato/run-locally/)
# CLI TODO List 
current functions: 
- [x] specify stack name + path/to/local/jar ✅ 2023-11-12
- [x] local debug ✅ 2023-11-12
- [x] newt init ears project ✅ 2023-11-29
improvments: 
- [ ] add arguments for runtime version 
- [x] version control ✅ 2023-12-01
- [x] local validation --> no need to commit code first ✅ 2023-11-29


<mark class="hltr-pink">2023-11-28</mark>
have a txt file that stores the version of sbn-dev-agent-client and boot.jar; everytime we first check the local version vs. latest version; if unmatch, then do a fetch update; otherwise skip the download. 


# Archived
## NEWT 
java-project will create a Jenkins job, which is preconfigured 

gradle task: publishToArtifactory 
buildAndPublish 

netflix.ci --> how it runs in Jenkins 
newt build_and_publish all --> build docker image

### Possible solution
maybe create another docker image that has a different entry point 
- spirng cli application 
- one approach: add some configuration ins netflix.ci to let the Jenkins job to build a new docker image so that it has a new entry point, which is to run the application with a command line and takes in some variables, which could be passed into the command line
- then, we could pull the docker image, and we can run the container so that it starts with the command line. 
- so first, just figure out how to run the application with a command line, and how to pass in the variables like local jar so that it could run locally

In netflix.ci, find out a way to first build a jar file of the cinder-cloud-service application. 
and then do sthm like this in Dockerfile
```Dockerfile
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

Under the hood, Spring Boot Netflix automatically generates a ==Titus== compatible Dockerfile as part of the gradle debian creation (`./gradlew buildDeb`). which in our case, is this step: `Task :cinder-cloud-service-server:buildDeb`
in jenkins job: 
```
**03:39:04** > Task :cinder-cloud-service-server:buildDeb
**03:39:04** > Task :cinder-cloud-service-server:runtimeManagedDockerfile
**03:39:04** > Task :cinder-cloud-service-server:generateDockerImageMetadata
**03:39:09** > Task :cinder-cloud-service-server:buildDebPublish
```

- Result: doesn't support this approach, need to use a regular cli solution 