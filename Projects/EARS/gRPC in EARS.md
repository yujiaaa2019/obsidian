<mark class="hltr-pink">2023-12-19</mark>
Remove DYNAMIC aggregation type: no usage in the pass two weeks 

Drew's opinion: 
1. not making user experience more complicated with gRPC
	1. gRPC needs a client folder
Eddy's opinion: 
1. further support to gRPC is mininal 

## Multimodule support 
just changing path based on the module

## gRPC
<mark class="hltr-pink">initial approach </mark>
client write proto file --> generate client JAR --> in runtime, open JAR and add implementation during the runtime 
problem: 
1. could implement graphql server 

timeline: 
end of Q4

value adding for EARS: <mark class="hltr-pink">users don't need to worry about server</mark>

Learning: 
`repeated int64 title_ids`  will be implemented in Java code as `List<Long> titleIdsList`



## Sample gRPC cli 
#### list_insights
```
grpc --local -d '{"titleIdType": "NETFLIX", "movieIds": [70279000]}' com.netflix.ears.contentinsights.InsightsService.ListCISLInsights
```
### AggregateToCsrgns
```
grpc --local -d '{                                              
  "metricType": "start",
  "countryPredictions": [
    {
      "countryCode": "CA",
      "metricValue": 0.4
    }
  ]
}' com.netflix.ears.contentinsights.InsightsService.AggregateToCsrgns
```
### GetUpcomingLicensedTitles
```
 grpc --local -d '{                                  
  "metricType": "start"
}' com.netflix.ears.contentinsights.InsightsService.GetTopUpcomingLicensedTitles
```

### AggregateCsrgnToGlobal
```
grpc --local -d '{
  "metricType": "start",
  "aggVoteWeightDate": {
    "value": "20230101"
  },
  "csrgnPredictions": [
    {
      "csrgnCode": "latam",
      "metricValue": 2
    }
  ]
}' com.netflix.ears.contentinsights.InsightsService.AggregateCsrgnsToGlobal
```

```
grpc --local -d '{
  "metricType": "start",
  "aggVoteWeightDate": {
    "value": "20230101"
  },
  "csrgnPredictions": [
    {
      "csrgnCode": "us",
      "metricValue": 3
    }
  ]
}' com.netflix.ears.contentinsights.InsightsService.AggregateCsrgnsToGlobal
```

### AggregateCountryPredictions
```
grpc --local -d '{                                              
 "requests": [{
      "metricCode": "start",
      "countryPredictions": [
        {
          "countryCode": "CA",
          "metricValue": 0.40
        }, {
          "countryCode": "US",
          "metricValue": 0.20
        }, {
          "countryCode": "UK",
          "metricValue": 0.12
        }
      ],
      "aggregationWeightDate": {
        "year": 2024,
        "month": 1,
        "day": 10
      }
    }
  ]}' com.netflix.ears.contentinsights.InsightsService.AggregateCountryPredictions
```
### GetHistoricalCbPredictions
1. only accepts hitch title id
2. no DYNAMIC agg_type_code
3. single value only for each field
4. not sure if prediction class and metric code are required 
```
grpc --local -d '{
  "titleId": 3876590,
  "grainType": "GLOBAL",
  "aggTypeCode": "ALL",
  "metricType": "start",
  "predictionClass": "audience_size"
}' com.netflix.ears.contentinsights.HistoricalCbPredictionsService.GetHistoricalCbPredictions
```

```
grpc --local -d '{ 
  "titleId": 3876590,
  "grainType": "COUNTRY",
  "grainCode": "us",
  "metricType": "start"
}' com.netflix.ears.contentinsights.HistoricalCbPredictionsService.GetHistoricalCbPredictions
```

Local Grpc testing 
1. Follow this [starting_guide](https://manuals.netflix.net/view/python/mkdocs/main/grpc/getting_started/#minimal-python-config) to add configuration in `build.gradle`
2. run `./gradlew pythonGenerateClient`
3. run `pip install -e path/to/client-python`
4. then we can have python files to connect with the server: 
```
from cindercloudservice_client_py import InsightsServiceClient  
from cindercloudservice_client_py.pb.latest_insights_pb2 import ListCISLInsightsRequest, InsightsRequest  
  
def main():  
config = InsightsServiceClient.DEFAULT_CONFIG.clone()  
client = InsightsServiceClient(config=config)  
  
response = client.search_insights(request=InsightsRequest(  
title_id_type="NETFLIX",  
title_ids=[80186833],  
metric_type="start",  
grain_type="GLOBAL"  
), timeout=30000)  
print(response.insights)  
return client  
  
  
if __name__ == "__main__":  
	main()
```
if want to test the local server, change to 
```
client = InsightsServiceClient(config=config, address="localhost:8980")  
```

*important*
need to use `pynt` to create an python env rather than `python -m venv env` 
pynt = `newt --app-type python ...` so that the environment is Netflix compatible. 

