## Functional Test 
run in build time; be able to run offline 
generally unit/integration 
should be self-contained 
don't touch network deps 
- fast and reliable 

Deploy time 
- end to end
	- verify real systems interact correctly together 
	- go/karbon (UI)
	- go/mopro (mobile)
- expensive to run/debug/maintain 
- can run to smoke test deploys 

## Non Functional Test
- load/scale/squeeze 
- performance 
- usability/exploratory 
- cross browser / device / platform

## Test Pyramid 
            E2E
	    Integration
	Unit Test ______

Pyramid: Less E2E, more Unit test 
Honeycomb: More integration test, less unit and E2E test 

Code coverage test: 80% upper bound 
- doesn't tell you code was validated 
