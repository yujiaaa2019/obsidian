# CISL API Architecture

### cisl_predictionContext
CISL doc ==> does not have instructions on live query wiring.yaml configuration setup 
`typeReference`: what should we put in keyMapping if the field is a list for Trino 	

- live query only -- Iceberg table (cb_prediction_history)
not used at all in TEST 
### cisl_historicalCBPredictions
1. if `start` && `audience_size` && `mid` && `global` ==> Cinder 
2. if `predictionClass != nos && not global` ==> live query -- Iceberg table (cb_prediction_history)

### cisl_searchInsights
- ignore Hitch Title for GQL 
- rather than using Unique Index, use HASH index on movieId, and filter the rest of inputs 
- cisl.latestInsights.v4 is too large for local dev. Need to create a new local cache version 
- query to create a table 
```sql
CREATE TABLE cie_test.cisl_content_subregions USING iceberg AS

SELECT

c.content_subregion_sk,

c.content_subregion_desc,

c.content_subregion_short_desc AS content_subregion_code,

c.is_active,

to_json(cast(array_agg(lower(d.country_iso_code)) as array<string>)) AS country_iso_codes

FROM

dse.geo_content_subregion_d c

LEFT JOIN

dse.geo_country_d d ON d.content_subregion_sk = c.content_subregion_sk

WHERE

c.content_subregion_sk <> -1001

GROUP BY

1,2,3,4

ORDER BY

1,2,3,4;
```
- query to overwrite a table
```sql
-- nohup spark-sql --ver 2.4.4 -f transform.cisl_aggregation_weights.sql -d SQL_TABLE=cie_test.cisl_aggregation_weights -d DSE=dse &
 2
 3WITH predictions AS (
 4  SELECT
 5   p.season_title_id as movie_id, 
 6   p.metric_code as metric_code, 
 7   cast(p.prediction_amt as float) as metric_value, 
 8   p.prediction_date
 9  FROM dse.cbl_season_global_detail_f AS p
10  WHERE p.metric_code = 'mvs'
11  limit 1000
12)
13
14INSERT OVERWRITE TABLE ears_prod.hollow_mvs_global
15SELECT * FROM predictions;  
```

### cisl_searchContentSubRegion
- move the table onto EARS MV -- managed workflow 

==contentinsights==
cannot use Orika with Java 17
so to migrate from java 8 to 17, need to migrate to other library first 

inject Jdbc template 

Entity transfering 

DGS sync is needed

move a federeated type? 

ask the DGS team ==> 
declare the override param type on EARS, 
remove things from CISL 


1. remove OSCountry, annouce to Athena users that we are migrating away

#### Examples 
input for countryContext: 
```
{
    "input": {
        "movieId": 81266017,
        "geoGrainType": "COUNTRY",
        "geoGrainCode": "us",
        "metricCode": "start",
        "predictionClass": "audience_size",
        "predictionDate": "2023-07-09"
    }
}
```

input for aggregatedContext: 
```
{
    "input": {
        "movieId": 81297917,
        "geoGrainType": "GLOBAL",
        "geoGrainCode": null,
        "aggregationType": "ALL",
        "metricCode": "start",
        "predictionClass": "audience_size",
        "predictionDate": "2023-07-08"
    }
}
```
input for featureInfluence: []
```
{
    "input": {
        "movieId": 81231975,
        "geoGrainType": "COUNTRY",
        "geoGrainCode": "us",
        "metricCode": "start",
        "predictionClass": "audience_size",
        "predictionDate": "2023-08-06"
    }
}
```

##### cisl_SearchInsights: 

##### cislInsights
```
query GetMetricDetailsForTitle($representations: [_Any!]!, $input: CISLInsightInput) {
  _entities(representations: $representations) {
    ... on Movie {
      movieId
      cislInsights(input: $input) {
        source
        metricVersion
        metricCode
        predictionDate
        predictionClass
        geoGrainType
        geoGrainValue {
          ... on NFLXCountry {
            id
            namespace
            territoryType
            __typename
          }
          ... on CISLContentSubRegion {
            code
          }
        }
        aggregationType
        contextSchema
        modelShortName
        metricValue
        lowerConfidenceAmt
        upperConfidenceAmt
        predictionComponents {
          isPrimary
          metricValue
          source
        }
      }
    }
  }
}

{
  "representations": {
    "__typename": "Movie",
    "movieId": 80186833
  },
  "input": {
    "metricCodes": [
      "start",
      "stick",
      "avs"
    ],
    "aggregationTypes": [
      "ALL",
      "IN_REGION"
    ],
    "geoGrainTypes": [
      "COUNTRY"
    ],
    "predictionClasses": [
      "audience_size",
      "catalog_adjusted"
    ]
  }
}
```
##### predictionContext
```
query GetPredictionContext($input: CISLPredictionContextFilter!) {
  cisl_predictionsContext(requestInput: $input) {
    modelDetails {
      predictionDate
      predictionValue
      modelShortName
      geoGrainType
      geoGrainValue {
        ... on CISLContentSubRegion {
          code
          description
          __typename
        }
        ... on NFLXCountry {
          id
          namespace
          __typename
        }
        __typename
      }
      __typename
    }
    predictionContextDetails {
      ... on CISLCountryContext {
        influenceAmt
        influencerName
        value
        featureDescription
        boxPlotField {
          min
          max
          five
          twentyFive
          fifty
          seventyFive
          nintyFive
          __typename
        }
        __typename
      }
      ... on CISLAggregatedContext {
        geoGrain {
          ... on CISLContentSubRegion {
            code
            __typename
          }
          ... on NFLXCountry {
            id
            namespace
            __typename
          }
          __typename
        }
        modelShortName
        predictionValue
        contextSchema
        rawWeight
        predictionContextAvailable
        __typename
      }
      __typename
    }
    __typename
  }
}

{
  "input": {
    "movieId": 81231975, 
    "geoGrainType":"GLOBAL",
    "aggregationType": "ALL",
    "metricCode": "start", 
    "predictionDate": "2023-07-09", 
    "predictionClass": "audience_size"
  }
}
```

##### HistCbPredictions 
```
query GetHistoricalPredictions($input: CISLHistoricalCBPredictionsFilter!) {
  
  cisl_historicalCBPredictions(requestInput: $input) {
    movie {
      movieId
      plannedLaunchDate {
        date
        __typename
      }
      __typename
    }
    geoGrainValue {
      ... on CISLContentSubRegion {
        code
        __typename
      }
      ... on NFLXCountry {
        id
        __typename
      }
      __typename
    }
    geoGrainType
    metricCode
    metricVersion
    historicalCBPredictions {
      predictionDate
      metricValue
      lowerConfidenceAmt
      upperConfidenceAmt
      modelShortName
      contextSchema
      __typename
    }
    __typename
  }
}

{
    "input": {
        "movieId": 81203480,
        "geoGrainType": "GLOBAL",
        "aggregationType": "IN_REGION",
        "geoGrainCode": null,
        "metricCode": "mvs",
        "predictionClass": "audience_size"
    }
}
```
## Suggestions
1. Add a "Converter" annotation? like the `CustomConverter` in CISL 
2. Add a "QueryBuilder"
3. Validator? 
4. Doc: add instruction on multiple indexFields (in EARS features)
5. If want to regenerate API, then needs to delete the folder, comment out all the code that uses HollowObject, and comment back when finished -- super inconvenient 
	1. solution: have a pull + generateHollowObject in build.gradle, and jar{dependsOn(generateHollowObject)}
6. In wiring.yaml, let the user specify the test config they want to user for local developing (like different Cinder namespace)

## Keep In Mind
don't migration UberCbParams, don't use orika

- use mapStruct java package
- use java query builder 
	- right now is hard-coded


1. [x] give feedback to Eddy about HollowQuerier runtime construction
2. remove OBSERVE_ONLY, set BEST fallback type as default, allow NOS_ONLY type
3. 

error message when pushed to test: 
```
   "errors": [
        {
            "message": "ClassCastException: class com.netflix.ears.contentinsights.graphql.types.Movie cannot be cast to class java.util.Map (com.netflix.ears.contentinsights.graphql.types.Movie is in unnamed module of loader java.net.URLClassLoader @32040bb1; java.util.Map is in module java.base of loader 'bootstrap')",
            "locations": [
                {
                    "line": 1,
                    "column": 431
                }
            ],
            "path": [
                "cisl_historicalCBPredictions",
                "movie",
                "cislInsights"
            ],
            "extensions": {
                "errorType": "INTERNAL",
                "origin": "cindercloudservice_contentinsights",
                "debugInfo": {}
            }
        }
    ],
```
I think the problem is that the env.getRequestParameters is being casted into a Map, but the actual return value is of Movie.class. 


Error in Contentinsigths: 
error message: 
```
 "message": "The field at path '/_entities[0]/cislInsights[9]/geoGrain/id' was declared as a non null type, but the code involved in retrieving data has wrongly returned a null value.  The graphql specification requires that the parent field be set to null, or if that is non nullable that it bubble up null to its parent and so on. The non-nullable type is 'ID' within parent type 'CISLContentSubRegion'",
```
Reggie link: [Netflix Authentication](https://bfgui.netflix.net/test/studio/graphiql?query=query+GetMetricDetailsForTitle%28%24movieId%3A+Int%21%2C+%24aggregationTypes%3A+%5BCISLAggregationType%21%5D%2C+%24geoGrains%3A+%5BString%21%5D%2C+%24geoGrainTypes%3A+%5BCISLGeoGrainType%21%5D%2C+%24metricCodes%3A+%5BCISLMetricCode%21%5D%2C+%24predictionClasses%3A+%5BCISLPredictionClass%21%5D%29+%7B%0A++movies%28movieIds%3A+%5B%24movieId%5D%29+%7B%0A++++...MovieFragment%0A++%7D%0A%7D%0A%0Afragment+MovieFragment+on+Movie+%7B%0A++cislInsights%28%0A++++input%3A+%7BaggregationTypes%3A+%24aggregationTypes%2C+geoGrains%3A+%24geoGrains%2C+geoGrainTypes%3A+%24geoGrainTypes%2C+metricCodes%3A+%24metricCodes%2C+predictionClasses%3A+%24predictionClasses%2C+filter%3A+EXCLUDE_CSNA%7D%0A++%29+%7B%0A++++source%0A++++metricVersion%0A++++metricCode%0A++++predictionDate%0A++++predictionClass%0A++++geoGrainType%0A++++++++geoGrain+%7B%0A++++++...+on+NFLXCountry+%7B%0A++++++++namespace%0A++++++++%0A++++++++territoryType%0A++++++%7D%0A++++++...+on+CISLContentSubRegion+%7B%0A++++++++code%0A++++++++id%0A++++++%7D%0A++++%7D%0A++++geoGrainValue+%7B%0A++++++...+on+NFLXCountry+%7B%0A++++++++namespace%0A++++++++%0A++++++++territoryType%0A++++++%7D%0A++++++...+on+CISLContentSubRegion+%7B%0A++++++++code%0A++++++++id%0A++++++%7D%0A++++%7D%0A++++aggregationType%0A++++contextSchema%0A++++modelShortName%0A++++metricValue%0A++++lowerConfidenceAmt%0A++++upperConfidenceAmt%0A++++predictionComponents+%7B%0A++++++isPrimary%0A++++++metricValue%0A++++++source%0A++++%7D%0A++++__typename%0A++%7D%0A%7D&variables=%7B%0A++%22aggregationTypes%22%3A+%5B%0A++++%22IN_REGION%22%0A++%5D%2C%0A++%22geoGrains%22%3A+%5B%5D%2C%0A++++%22geoGrainTypes%22%3A+%5B%0A++++++%22GLOBAL%22%2C%0A++++++%22COUNTRY%22%2C%0A++++++%22CONTENT_SUB_REGION%22%0A++++%5D%2C%0A++%22metricCodes%22%3A+%5B%0A++++%22start%22%0A++%5D%2C%0A++%22movieId%22%3A+81615967%2C%0A++%22predictionClasses%22%3A+%5B%0A++++%22audience_size%22%2C%0A++++%22hybrid%22%0A++%5D%0A%7D)

code link: [link](https://stash.corp.netflix.com/projects/SAE/repos/contentinsights/browse/contentinsights-server/src/main/java/com/netflix/sae/contentinsights/converter/CISLInsightGQLConverter.java)

Questions: 
in the current contentinsights, the `predictionComponents` in `cisl_searchInsights` is always `null`, should we do the same thing in the new API? 
- DataLoader -- cannot use MappedBatchLoader because the `load` function cannot take Hollow querier 
- workaround: is it safe to put env.getHollowQuerier in the Custom Context so that it is globally accessible? 



