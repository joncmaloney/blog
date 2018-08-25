---
layout: post
title: Serverless Best Practices
author: jon_maloney
summary: Design tips for designing serverless apps.

---

It might be an oversimplification but serverless technologies are about reducing compute costs by only using non persistent computing resources when required. 



## Minimize code size

When writing serverless code we should always assume that a containerized environment has to start before the application can run then the function is called. When code is traditionally run most of this environment has already been started and is running. In a java application all the operating system and the java run time environment (JRE) is typically already in RAM when a new jar file is run. In a serverless environment the container needs to be loaded and started first. If your using alpine linux variant this is typically 5-8MB. If we again take java as an example the java virtual machine will then need to be loaded from disk 100MB and then your jar file 20-50MB. Once the container has started it's had to load ~130MB of data into ram. With an average speed of ~128MB/s and not including how long it might take for the CPU to start, the JVM this could take 1 second before the code you've written starts executing. In comparison a GO application might be a total of 8MB letting your code start in 63ms. 

## Low Coupling - High Cohesion

This is best practice for almost any software that is written. In simpler terms each function should only do one thing. Some 