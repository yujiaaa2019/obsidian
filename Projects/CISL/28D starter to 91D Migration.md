<mark class="hltr-pink">2024-02-02</mark>
steps to make start_91d the default metric: 
1. contentinsights: 
	1. change sql to have only have start metrics (cb_prediction_history, cisl_mid_insights,  cisl_insights_history)
	2. change start to be the default
	3. remove the metric version (or we could leave it there)
2. trisolaris: 
	1. rerun prediciton conversion 
	2. change start to be the defaut

if don't need to change the current data: 
1. cislInsights: 
	1. remove NEXT, make CURRENT start_91d, PREVIOUS start
	2. default to start_91d

Core metrics: 
- starters 
- MVS (measured on the 91 day level)
- Impact value

These two are highly correlated, mostly the same 

- minor change 

creative predictions: 
- batch translation for past 28D predictions 

Athena make changes in mid-Nov

CISL -- has version of "starter" 
91D transition also occurs in the primary prediction


<mark class="hltr-pink">2023-08-30</mark>
Creative predictions + CB predictions 
priority: creative predictions
seems like just CISL and Athena 
timeline: 
1. Nov before thanksgiving ==> company 91D start 
2. CB in prod before 9/11
	1. translated creative prediction 

other apps? 
- do want all apps, but maybe not urgent 

Athena DB -- rails app 

- maybe retire CS&A predictions? (in Athena)

Questions: 
1. just for latest insights? or history starter predictions as well? 
	1. just latest insights 
2. the plan is to create a new metric code in the cisl_mid_insights table called `starter_91d`, and default to this in cisl_insights? for searchInsights, default to starter? 
3. what is the associate prediction_class? is it still audience_size? or hybrid

Steps: 
1. change the workflow for cie_test.cisl_mid_insights to add a new metric_code type `start_91d`
	1. cislInsights consumers could opt in for start_91d, but the default is still 28d
	2. for searchInsights, need to error if user try to use `start_91d`
2. once it is in stable version, then we could just remove the target_age column, consumer don't have to change anything. We just need to remove the target_age condition when filtering data. 
- just the latest insights, all default to 28d starter, but provide 91day as an option
- but htne how do you limit users 

if have time: 
- explain a bit how does Docker work in Netflix. like the dockerregistry image, how is it been published? and where can I find more resource to learn and understand? 

Context 
-----------------------
Why we choose to simplify the metrics
Content Metrics: MVS focus nonfiction 

secondary metrics: 
- size: 91D starter % (replace impact value)
- cost effectiveness: starter targets fiven budget (replace efficiency)

the two things that the content team would only focus on: 
content quality, creative vision 