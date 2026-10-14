```groovy
java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(17)
    }
}
```

This block determines what version of Java to compile, test, and  run the project. 
- it will use JDK 17 for compiling 
- it is the JVM version used for compiling the Gradle Plugin