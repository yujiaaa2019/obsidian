ideal result: 
- users are able to use command line to start the service locally, and pass in their local jar file. Then, they should be able to step into their business logic code to debug. 
```
newt develop-local -jar /path/to/your/jar 
```
Problems: 
1. how to use command line to start a service locally 
	1. need to use `newt`

2023-10-25 Updates: 
1. two command lines: use `newt` to construct an EARS app (like ears-demo) `newt apptype` and the user need to enter the project name, stack name, 

## Newt 
use newt to 
1. creates something like the ears-starter project
2. download the cinder-cloud-service runtime project
3. pass in vm-options to make the runtime to listen to a speficied port, default 5050. 
## EARS app
create ears-starter app: 
`newt init --app-type ears` create an ears-starter project
for later: 
`newt validate` validate the latest commit's schema 
`newt deploy` deploy the latest commit

## runtime
start local runtime
`newt init --app-type ears-cli`: download the runtime
- will ask to provide path/to/jar
- will ask to provide debug port, default 5050


do we need to add a separate container for the runtime to run locally
if container is a paved path then use it. 

docker pull pdt/cinder-cloud-service:main
docker 

define static stuff in the wiring.yaml 



##### GPT response
Q: Pass Args in `application.yaml`:
A: 
Yes, you can pass command-line arguments, which you can then access in your `application.yml` (or `application.properties`) file.

In Spring Boot applications, you can pass arguments using the `--args` flag, and then reference them in your YAML file using `${...}`.

Here's an example:

bash

Copy code

```bash
./gradlew bootRun --args="--localStack=contentinsights --localEnabled=true --localPath=/Users/yujiax/Documents/Code/cisl-ears/build/libs/cisl-ears.jar"    
```

This will pass `some_value` as the value for `my.argument`.

You can then access this in `application.yml` like so:

yaml

Copy code

```yaml
my:
  argument: ${my.argument}
```

This will set the `my.argument` value in your configuration to the value passed in via the command line.

If you want to have a default value for the argument (i.e., a value that will be used if you don't pass the argument via the command line), you can specify it like so:

yaml

Copy code

```yaml
my:
  argument: ${my.argument:default_value}
```

In this case, if you don't pass `my.argument` via the command line, it'll be set to `default_value`.

