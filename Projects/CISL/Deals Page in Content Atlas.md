EARS Iceberg -> Hollow  

- How to create Iceberg Producer (Iceberg -> Hollow): [Iceberg Hollow Producer](https://coda.io/d/CISL_d_ZSz7-l-d9/Iceberg-Hollow-Producer_suwSk?loginToken=eduardor@netflix.com#_luKvb)

- Extra context on Materialized Views: [Materialized Views](https://coda.io/d/CISL_d_ZSz7-l-d9/Materialized-Views_su240?loginToken=eduardor@netflix.com#_lujxI)

- Hollow docs: [https://hollow.how/](https://hollow.how/) (there's also [go/cinder](https://manuals.netflix.net/view/cinder/mkdocs/master/) that has docs on the internal pieces of Hollow, ie the Cinder part)
- Docs on adding listeners to a consumer to react to changes: [Responding to Updates](https://manuals.netflix.net/view/cinder/mkdocs/master/howto/consumer/#responding-to-updates)

- the docs on listeners are not very great so lmk if you are interested in this option and we can pair on a prototype

- You can use [CinderDevUtils](https://manuals.netflix.net/view/cinder/mkdocs/master/tooling/ide-shorthands/) to easily generate the API client:

CinderDevUtils.generateAPIs("my-namespace", "test", "path/to/output", "com.netflix.org.hollow.api, "MyAPI");

RAW Hollow  

- the homebase for docs [go/rawhollowdocs](https://docs.google.com/document/d/1V_EFMUrtIxDstEjNWJJgf26e8SOUr7xogrrZZaPXTag/edit#heading=h.vwtgyfte2waj)

- The [Quick Start guide](https://docs.google.com/document/d/1RSQUhW9aDh-8vRRnE_GAKtyKzzfkWc8wnAt5t-PmeJ8/edit?pli=1#heading=h.rtz4zlx2wegy), you should try this to give it a spin. you can create a toy namespace and write/read to it to get a feeling of how it works 

- the docs on how to define Hollow entities: [https://hollow.how/data-modeling/](https://hollow.how/data-modeling/)