---

---
## why not use GraphSearch DSL 
1. nested object syntax doesn't seem to work well
2. not work well with JOIN different entity 
3. HollowSQL --> RH don't need aggregation 

benefit: 
migration from RH to GraphSearch

_ANTLR_ (ANother Tool for Language Recognition) is a powerful parser generator for reading, processing, executing, or translating structured text or binary files.

use GraphSearch 
query DSL 
filter DSL 

1. cross-entity search 
	1. Graph Search supports
		1. problem: lots of fetching (over fetching)
		2. need to join multiple indices
2. benefits
	1. can enrich a EARS entity
3. Single entity search
	1. works without federation 
	2. __search API
		1. only talk directly to the RH namespace 
-----

1. install ANTL in gradle
	1. `apply plugin: antlr`  
		1. repo: studio-search-dgs
	2. dependency 
2. define grammar --> .g4 file
3. then generate the code 
4. write our own SearchParser
	1. using the generated parser 
5. do no support .key and .value for Map 


---- 
populate user idenity from UI. whitelist UI app in AuthZ policy for all namespaces


1. compareAndSwapAsync(FlatRecord, FlatRecord)
	1. safer than addOrUpdateRecord --> might have collision 
2. implement a JSON -> FlatRecord mapper 
	1. using HollowWriteRecord and HollowOjectSchema