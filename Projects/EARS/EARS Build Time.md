
1. Identify fast tests / slow tests
	1. give reports to the list of slow tests (should be part of tech health)
2. compile time + test time <= 10s 
3. fast deployment is very important 
	1. encourage users to do early deployment (fast rollback as well)
		1. get fast feedback 
4. operational agility for exploratory testing 
5. `Shift-left testing`
	1. emphasizes moving testing activities earlier in the development process for improved software quality, better test coverage, continuous feedback and a faster time to market.
6. hybrid managed infra 
	1. provide EARS functionalities as a library 
		1. have existing apps pull in EARS lib; EARS would manage the EARS part of your apps
7. a more robust story for local dev
8. remote dev
	1. value --> code sharing, collaboration, feature stack...
9. have a local cluster constantly running, picks up local change, and listen for new updates and hot loading new changes