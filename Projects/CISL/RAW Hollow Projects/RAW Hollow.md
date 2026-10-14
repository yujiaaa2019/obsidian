# RAW Hollow
Read-after-write 
RAW Hollow Architecture
**![](https://lh4.googleusercontent.com/hLlE_VkQBS64nJGo79WDnFi-CKK_Q-I8915uNC4ieHW56fMpN8KJeXM_VB4AE_P4TGEFj7uN489J9PI1wplSX92Gijg_9MZWF-KIntgb_YBCAoaLDAI5GalkdxOQeTTH4RQVlm1_fn9RueONzDXDouc)**
**![](https://lh3.googleusercontent.com/qo4fuxsYLSDgrOPhz8kKRH_4IVL79UZzKVJWA7CW05cOdcheqPQ5swftZ0N2leAUKlMc0k4EgmXoQ07dpVh6nGj8BD4mu8XgTbhu1MHFdmlti0yzJzgv-e1W7SWf1efw9tHT7FX5t6JH5xCacklcRhE)**
overlays real-time updates on tradition Hollow framework 
- host a writer instance for each namespace
- the real-time updates to Hollow consumers (app) of the namespace is made through *in-flight messages log*

Suitable use-cases: 
few writes, heavy reads 
- not for transactional operations 

RAW Hollow
- put a distributed system on top of Hollow 
- in memory access with RAW consistency 