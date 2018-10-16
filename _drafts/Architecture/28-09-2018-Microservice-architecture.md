---
layout: post
title: Microservice Architectures 
author: jon_maloney
summary: A quick summary of microservice anti patterns
---

The timeout anti pattern

In instances where there are is a request chain it might seem feasable that a timout is used on API requests. This becomes a problem if a service is failing to respond. This is because each request would have to wait the timout period. 

Instead of timouts a circuit breaker should be used. This pattern detects services that are non responsive and stops requests going to the service until the service becomes responsive again. 

--Shared code antipattern

Microservices typically share nothing to ensure they are as independant as possible. Hoever this is not always possible. For example utils libraries or persistance layers may be required across multiple services. In this case there are three possibilities: 
	1. Project level integration - compile the code into each microservice. 
	2. Library level integration - compile a library file and use this library in the service. 
	3. Create a seperate copy of the code. - Copy the code and place this in the microservice. 

The third option should only be used for very stable code as changes to this code would require updates to each copy. 	

--Microservice Reporting

Once data schema is sectioned into microservices it becomes difficult to report on this data. This is especiallt the case when data needst to be combined from multiple data schemas. 

One pattern is to use a reporting database. The reporing service will batch upload data into the reporing database. The reporting databse will then be used to generate reports. 

Another pattern is to use asyncronous messaging to update the reporting database as the database in the microservice db gets updated. This pattern is more difficult to implement.


