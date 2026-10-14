Current Functions:
Getting latest 91 day starters insights for all Netflix titles (series + shows), and 91 day starters / stickiness / avs historical predictions, and add / delete creative predictions 
Also getting details about a prediction if available through prediction context
Core Functions: 
1. Get all upcoming titles and details, this is through `search`, and ranked by plannedLaunchDate. Details including Crystall Ball predictions and Creative Personnel Predictions. The Primary prediciton displays the prediciton value for the hybrid prediction class, which is described by the doc here: [docs.google.com/document/d/1Ii08LVDtOAJ4DQAwsl\_eqkyrsfCrUFxquLPKpE\_hnpk/edit#heading=h.wi9rnrbhd27v](https://docs.google.com/document/d/1Ii08LVDtOAJ4DQAwsl_eqkyrsfCrUFxquLPKpE_hnpk/edit#heading=h.wi9rnrbhd27v)
2. Title Details Page: 
	1. historical predictions for 91 Day starters, stickiness, and 52 week AVS. 
	2. prediction context: basically the prediction model details breakdown. The first page is the country level prediction breakdown, all of these metrics and weights are directly supplied by DSE data, and surfaced through CISL apis
	3. clicking on the country details, it will show the model details breakdown, specific to individual influencers. These details are also directly supplied by DSE data. 
	4. clicking on the individual model details, it will show boxplot details. We don't need to understand what the number means, when asked just redirect them to #ask-cdm 
	5. prediction context model features, there is a mapping between model_names and human-readable names, and it is managed by the predictionContextFeatures EARS usecase. Will cover more in the CISL handoff session. 
	6. cislInsights querying parameters: 
		1. geo grain type: GLOBAL, COUNTRY, CONTENT_SUBREGION
		2. geo grain value: only for COUNTRY and CONTENT_SUBREGION 
		3. aggregation type: only for GLOBAL, ALL and IN_REGION
		4. prediction class: ''audience_size" for starters and stickiness, "catalog_adjusted" for AVS (by default)
		5. metric code: "start", "stick", "avs"
3. Latest Predictions: 
4. Runbook 
Debug workflow: 
1. check if it's a UI or BE problem: 
	1.  if the data is inconsistent between Cinder Namespaces and UI --> probably UI error
	2. if the data is inconsistent between namespace and underlying materialized view --> probably Cinder failed to refresh the data 
	3. if the data is inconsistent between MV and underlying DSE IB table --> probably something wrong with jobs or sql. 
	4. example: if Natali reached out and say latest insight is wrong, then first go to cisl.latestInsights to search for that title and check values. 
2. 