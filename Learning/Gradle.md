### JAR 
build a single Jar file with multiple modules: 
in the source root `build.gradle` 
```
jar {  
	from {  
	subprojects.collect { it.sourceSets.main.output }  
	}  
}
```

### settings.gradle
gradle add nested subproject from a multi-module project as dependency of another subproject 
![[Screenshot 2023-10-23 at 10.47.35 AM.png]]
[Gradle Cheet Sheet](https://manuals.netflix.net/view/nebula/mkdocs/main/getting-started/cheat-sheet/)

java -cp lib 
ax -r unzip deb file 


1. try if the current plan jar works 
2. if not, figure out how to include the boot.jar into deb
3. use JFrog API to get the latest release version of deb file and unzip it in the cli repo. 
4. run the application using java -jar