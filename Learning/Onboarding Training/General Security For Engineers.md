https://netflix.atlassian.net/wiki/spaces/CLOUD/pages/1220968911/Security+Paved+Road

## Meechum
SSO (single sign on) on top of the Pandora identity store 

## Metatron
- secure access to app secrets 
- app-to-app mutual TLS
- "Metatron Identity" as basis for API authN

## Gandalf 

## Wall-E
A version of edge-facing API Gateway. 
Accepts traffic from the Internet, routes it securely to the app 

## CDE Data Gateway 
secure db proxy 
use for abstraction/reliability etc 
Secured by Gandalf policy 


![[Screenshot 2023-07-19 at 1.49.33 PM.png]]

## Code Hygiene 
go/qcc
Quarterly Change Cycle
use artifactory 
managing external dependencies 