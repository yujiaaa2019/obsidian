
BE: 
- [x] TODO add API for createNamespace ✅ 2023-08-15
- [x] TODO add API for listNamespaces ✅ 2023-08-15
- [x] TODO create RawHollow Persister ✅ 2023-08-15
- [x] TODO Create RawHollowNamespace in MES ✅ 2023-08-15
FE: 
- [x] TODO add UI for RawHollow Page ✅ 2023-08-15

## 2023-08-15
add overwrite and truncate functions 
- overwrite: 
	- users should be able to overwrite the namespace with the same name 
	- on UI, make it double step: 
		- click `overwrite` button
		- ask user again for confirmation 
	- strictly FE. BE no change
- truncate: 
	- clear the namespace
	- i.e. end the namespace first, and then start it 
	
```java
import com.netflix.rawhollow.namespace.RawHollowNamespaceEnder;
import com.netflix.rawhollow.namespace.RawHollowNamespaceStarter;

public class Truncate {
  public static void main(String[] args) {
    // Replace `your-namespace` with your namespace and run this main as is
    RawHollowNamespaceEnder.endNamespaceInTest("your-namespace");
    // Once that finishes successfully, comment out the above line and uncomment the below line and run this main again
    // RawHollowNamespaceStarter.startNamespaceInTest("your-namespace", /* overwrite */ true);
  }
}
```

Think about how to structure the code 
idea: 
- in RH repo, only includes operational functions for RH, avoid any deployment-related functions 
- put deployment-related ones in control plane 

TODO: 
1. bug in getNamespaceDetails ==> need to check if namespace exists 
2. bug in createNamespace ==> did not catch all exceptions (fixed, not pushed)
3. truncate: end namespace first, need to wait until the pipeline is finished, and then start the starter. 


1. have a function to trigger the clean-up pipeline (in controller)
2. in service, implement the function to start the clean-up pipeline, and once finished, trigger the second pipeline 

## 2023-09-01
TODO: 
- [ ] change all functions into completablefuture
- [ ] change RawHollow object to have the following: 
	1. namespace 
	2. createdBy 
	3. currentAction: CREATE, OVERWRITE, TRUNCATE, DELETE
	4. actionBy 
	5. currentStatus: SUCCEED, PROCESSING, FAILED
- [ ] first check MES status, if processing, then throw exception; if not, change the status to processing, and then start the operation. use completefuture to return the updated MES object, and kickoff operation pipeline. Once pipeline is finished, change the MES object to succeed/failed, and return its status. 
- [ ] have a getStatus function that check the MES object. 


Changes: 
1. group the exceptions (first check exist, then check status with actions) 
	1. better for later adding new actions & code maintanance 
2. make the getCompleteStatus async only, other part sync

ssh instance-id 
cd /var/log/java

OVERWRITE: 
- deleted the namespace for whatever reason, need to reinitialize it again 
- no RH cluster in rawhollow Spinnaker 

TRUNCATE
- has RH cluster in rawhollow Spinnaker --> so need to delete it first (`endNamespace`), and then run `startNamespace`

DELETE + OVERWRITE = TRUNCATE 


## 2023-09-20
### Improvements 
1. refractoring 
2. what is the instance is down? 
	1. add the pipeline id in MES 
	2. once people visit the page, if PROCESSING, then check the status of the pipeline using the pipeline ID
3. change to .
4. change whenComplete to whenCompleteAsync 

## 2023-09-27
### UI 
Q: should we move to CDE UI and how (separate of work)
operational things 
1. can deploy a RH namespace through CDE UI 
2. refresh -- still under code phase 
3. provisioning -- run starter / ask 
	1. so no UI 
4. truncate - (aka reset)
	1. Drew - better in explorer UI 
5. UI 
	1. better for look into how many clusters are there

visibility tools (urgent need)
- what functions should visibility have? 
1. ~2025 for CDE team: so we need to do 
2. Drew -- less about maintanence stuff, more on visibility stuff