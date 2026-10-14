The bom iterates over the `apis` defined in the modules, and resolve the correct versions for them, and then append the classpath to compile time and runtime time respectively 
- on user end, foundational ears dependencies will be packaged into a gradle plugin, the user could specify a platform version to point to a specific bom 
- on developer end, to update the dependency, just add/delete the `api` in the modules
	- in most case, the bom should take care of resolving the versions

Testing if bom works:
- build the project that has dependencies on the bom itself
	- if the bom is incorrect, the build will fail 

Security gate on Deploy 

- access control 
- Only specified team should be allowed to view the usecase 
user-level access to github 

every repo is private now 
app should be auth behalf of the user

Jenkins job reads the repo content 
- Jenkins has only one Jenkins identity 
- Jenkins job with each have an identity for each workflow (in the future)

if not able to see on GH, then not able to see on EARS UI
list of available apps to onl;y have the ones that users have read access to