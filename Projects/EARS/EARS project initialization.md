### Topcoat Command 
modify repo settings URL: 
```
metatron curl -a topcoat -X PATCH "https://git.prod.netflix.net:7004/api/v3/repos/corp/ears-cyntest10" \
  -H "Accept: application/vnd.github+json" \
-d '{"allow_rebase_merge":true, "allow_squash_merge":true}'
```
repo settings list: [Netflix Authentication](https://sourcegraph.netflix.net/github.netflix.net/corp/scm-scm-dgs/-/blob/scm-dgs-server/src/main/java/com/netflix/scmdgs/services/github/model/GitHubCreateRepoRequest.java?L18:57-18:73)


```
mutation {
  ears_createRepo(repoName: "cyntest", permissions: {
    level: ADMIN, 
    type: GITHUB_TEAM,
    id: "ears-admin-access"
  }, description: "test repo", topics: ["ears"]) {
    errors {
      message
    }
  }
}
```
--------------

```
mutation {
  ears_createRepo(input: {
    name: "cyntest-hook", 
    description: "test repo for EARS hooks", 
    topics: ["ears"],
    owner: CORP, 
     permissions:[
          {id:"ears-core", type:GITHUB_TEAM, level:ADMIN},
      {id:"yujiax", type: GITHUB_USER, level: ADMIN}
        ]
  },
    stack: "cyntest", 
    repoType: RAW_HOLLOW
  ) {
    pipelineExecutionUrl
  }
}
```

`metatron curl -a stash` commands:
``` Create PR
metatron curl -a stash -X POST "https://stash.corp.netflix.com:7004/rest/api/latest/projects/EARS/repos/cyntest-1/pull-requests" -H "X-Atlassian-Token: no-check" -H "Content-Type: application/json" -d '{
    "title": "test", "description": "hi",
    "state": "OPEN",
    "open": true, "closed": false,
    "fromRef": {
        "id": "refs/heads/test",
        "repository": {
            "slug": "cyntest-1",
            "project": {
                "key": "EARS"
            }
        }
    },
    "toRef": {
        "id": "refs/heads/main",
        "repository": {
            "slug": "cyntest-1",
            "project": {
                "key": "EARS"
            }
        }
    }
}'
```

```
metatron curl -a stash -X PUT "https://stash.corp.netflix.com:7004/rest/api/latest/projects/EARS/repos/cyntest-1/browse/.graphdoctor.yml" -F content="hi_2" -F branch="test"  -F sourceBranch="main" -F sourceCommitId="c4886620e024deac5fe50b8d19c393129148b611"

metatron curl -a stash -X PUT "https://stash.corp.netflix.com:7004/rest/api/latest/projects/EARS/repos/cyntest-1/browse/README.md" -F content="hi_2" -F branch="test"
```

--------


Learnings & Memo 
1. Github App act on-behalf-of-a-user --> GH App's "writes" privileges are constrained by the user's write privileges. 
2. When a Spinnaker App makes a REST call to `topcoat`, it goes through the credential proxy which provides metatron authorization 
	1. `topcoat` == `git proxy` == `credential proxy`
	2. it provides metatron authorization, and redirects the call to proper SCM (either GH or Stash)
3. Each spin app has it's own Metatron Identity. It should default have read access to all repos, but no write access. 
4. Git Credential Manager is a Github App that handles authenticating to GHES (GitHub Enterprise Service)
5. In order for a spin app to be able to write to a GH repo: 
	1. first need to register the spin app should be authenticated on behalf of a user. 
	2. Github App is the Github version of managing the App's repo access to other repos, so we need to first install a Github App in our Github Repo, make the App having CORP-level permission, and then register the Github App as an app-on-behalf-of-user app (need to be done by the GH team)


--------
Github Authorization Issue 
1. 403 Unauthorized -- service/application only has READ access for all repos; user can have WRITE/ADMIN access based on repo permission; topcoat (github) will throw error if try to use user identity to GET some resources
2. 

DGS MFA gate: 
1. need the UI to elevate risk level to EXTRA_HIGH and register with Claire in order to get the access token from MFA-gated SCM DGS
2. Once UI is authenticated, user identity will be passed in for the API call, the user identity will then be appiled to control plane API call that actually access the SCM DGS using user identity. 

Read File in deployed application: 
Since the SBN application is being compiled into a boot-jar for deployment, static files in resource folder need to be handled differently than local file system. Solution is to use JarURLConnection so that it opens a JarConnection that could read the file system. 


------
bitbot create-repo: 
[Initiating SAML single sign-on](https://github.netflix.net/corp/engtools-bitbot/tree/9aec491236adb0cc43c627223d0613acc60e1345/src/main/java/com/netflix/bitbot/service/github)

Query to create Github repo: 
```
mutation createRepo{
    scm_createRepository(input:{
        owner:CORP,
        name:"cyntest-2",
        topics:["mwi", "ears"],
        description:"test for creating git repo from mutation",
        permissions:[
          {id:"ears-core", type:GITHUB_TEAM, level:ADMIN}
        ]
    }) {
        scmId
        url
    }
}
```

Steps: 
1. Create API call to create github repo (empty)
2. once succeeded, make another call to push initial commit to the repo
	1. includes all necessary files in ears-starter repo 
Form info: 
1. Repo name!
2. topics !
3. description
4. permissions (default adds ears-core, user can input other info)
5. EARS usecase name 



{
  "repoName": "cyntest-2",
  "topics": [
    "ears"
  ],
  "description": "",
  "permissions": [
    {
      "type": "GITHUB_TEAM",
      "id": "ears-core",
      "level": "ADMIN"
    }
  ],
  "useCaseName": "cyntest",
  "useCaseOwner": "yujiax@netflix.com"
}

