# Inversion of Control
IoC: a principle in SWE
- transfer the control of objects or portions of a program to a container or a framework
### Advantages 
1. decoupling the execution of a task from its implementation
2. making it easier to switch between different implementations
3. greater *modularity*
4. greater ease in testing 
	1. isolate component or mock its dependencies 
	2. allow components to communicate through contracts
## Dependency Injection
*A pattern to implement IoC*
```java
public class Store {
    private Item item;
    public Store(Item item) {
        this.item = item;
    }
}
```

## IoC Container 
==manages the lifecycles of dependencies==
`ApplicationContext`
Objects can be injected at runtime or startup time 
- the IoC Container is responsible for instantiating, configuring, and assembling objects known as **beans**, and managing their life cycles
	- example implementations of `ApplicationContext`:
		- _ClassPathXmlApplicationContext_
		- _FileSystemXmlApplicationContext_
		- _WebApplicationContext_: for web applications
**Dependency Injection in Spring can be done through constructors, setters or fields.**
### Constructor-based DI 
```java
@Configuration
public class AppConfig {

    @Bean
    public Item item1() {
        return new ItemImpl1();
    }

    @Bean
    public Store store() {
        return new Store(item1());
    }
}
```
_@Configuration_ annotation indicates that the class is a source of bean definitions.
_@Bean_ annotation on a method to define a bean
	For a bean with the default _singleton_ scope, Spring first checks if a cached instance of the bean already exists, and only creates a new one if it doesn't. If we're using the _prototype_ scope, the container returns a new bean instance for each method call.
### Setter-based DI
```java
@Bean
public Store store() {
    Store store = new Store();
    store.setItem(item1());
    return store;
}
```
the setter method will call a *constructor* or a *static factory* to instantiate the bean

### Field-based DI 
```java
public class Store {
    @Autowired
    private Item item; 
}
```
use the _@Autowired_ annotation to inject dependencies on a field 
## Bean Factory
==Bean factory is the IoC container==
don't usually interact directly with Bean Factory (can use life cycle hooks)

will conduct analysis of construction order so that beans do not have conflicting dependencies 
## Application Context 
will trigger the application lifecycle 
It is a `wrapper` around the bean factory, and then share some common interfaces 
## Refereces
Bean Factory maintains `references` to Beans

# Gradle 
`./gradlew build --refresh-dependencies`: build the project with refreshed dependencies 

# Java Common
## SBN 
To add a submodule into a project, need to add `include "contentinsights-ears"` into settings.gradle
## Future
The *Future* Interface is a result of an asynchronous computation 
*CompletionStage* Interface: defined the contract for an async computation step that we can combine with other steps 
*CompletableFuture*: implement both the Future and the CompletionStage Interface

The _CompletableFuture.allOf_ static method allows to wait for the completion of all of the _Futures_ provided as a var-arg

## Syntax
### .equals vs. ==
We can use == operators for reference comparison (**address comparison**) and .equals() method for **content comparison**. 

# Jackson
ObjectMapper

# mapStruct
@Named & qualified name

# Spring Data JPA
## Specifications
```java
public interface Specification<T> {
  Predicate toPredicate(Root<T> root, CriteriaQuery query, CriteriaBuilder cb);
}
```
Specification is an interface for reusable `Predicate`s
- Querydsl might not be suitable for gradle project 

## Consumer 
```
the `Consumer<T> sweep` doesn't directly take a `RecordPrimaryKey`. Instead, it takes a parameter of type `T`, which is the result of applying the `readFunction` to a `RecordPrimaryKey`.


private <T> Consumer<List<RecordPrimaryKey>> wrapSweepConsumer(Consumer consumer, Function<RecordPrimaryKey, T> readFunction) {  
    return primaryKeys -> {  
        primaryKeys.parallelStream().forEach(pk -> {  
            try {  
                if (rhEventsCoordinator.proceedWithSweep(pk)){  
                    consumer.accept(readFunction.apply(pk));  
                }  
            }  catch (Exception e) {  
                LOGGER.warn("Exception while sweeping for key {}", pk, e);  
            } finally {  
                rhEventsCoordinator.sweepCompleted(pk);  
            }  
        });  
    };  
}
```

# FailedAnalyzer
when sth failed, they write the logs to a local box