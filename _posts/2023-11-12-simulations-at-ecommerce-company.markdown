---
layout: post
title:  "Simulations"
date:   2023-11-12 17:26:36 +0000
categories: 
---
Simulations are an underappreciated and underused tool in many companies. 
Most companies have multiple complex systems, many times interacting with each other, which require constant optimisation and simply need constant monitoring to understand.
We are generally afraid to touch these systems as we're unsure of the impact of the changes on it or on downstream systems.
If we can build simulations that can correspond well enough to these systems to answer important questions about the systems, then we can more freely experiment in that sandbox environment and understand our systems more completely.
Successes at large companies are out there ([Amazon](https://d1.awsstatic.com/events/Summits/reinvent2022/INO105_Supply-chain-and-logistics.pdf) and [Doordash](https://doordash.engineering/2022/08/16/4-essential-steps-for-building-a-simulator/) are two examples).
I want to argue that teams that are implementing Machine Learning products are the same teams that have complex enough systems to warrant simulating, but are also the same teams with the skill sets to build and use simulations.

## What is a simulation
> A simulation is the imitation of the operation of a real-world process or system over time

Trying to reproduce reality in a simpler modifiable setting

### Mississippi River Basin Model
- Could run 1 day of water in 5 minutes
- Could run different levels of flooding to examine effects
- Could evaluate effectiveness of different flood protection measures
- Saved millions in 1950s (on the order of a billion in today's money)
- Tests on individual problems were conducted until 1971 but high costs and growth of computer modelling meant that the facility was put on standby.
 ![img.png](/assets/images/simulations/MissBasinModel_Color_Aerial_800x538.jpg) ![img.png](/assets/images/simulations/Mississippiriver-new-01.png)



### What makes up a simulation
 ![img.png](/assets/images/simulations/components.png)
- Input
  - Amount of water added at upstream points
- Mechanism
  - Literal recreation of the landscape at reduced scales
  - Real physics (gravity, hydrodynamics etc)
- Output
  - Water heights at downstream points at various times

![img.png](/assets/images/simulations/components-for.png)

Depending on which part of the simulation is generating new data, the simulation is used for different purposes
For example using earth's atmosphere as a domain:
- What’s the weather tomorrow? -> Prediction
- What would the climate be like next century IF we reduce emissions -> Prediction
- How do hurricanes form? -> Explanation
- What was the climate like 1million years ago -> Retrodiction

### Mechanism
Dive into disease modelling

### Correspondence
Why some simulations were bad 

### Calibration
Necessary for later in presentation, not necessary here?

### Running a simulation

### Why use simulations
- Make predictions about the future
- Too risky/expensive to do in reality
- Floods of areas
- Too slow to do in reality
- Simulate the cosmic web
- Need to run too many experiments
- Waymo has driven 1000x more in simulations than in reality
- Reinforcement learning requires more data than reality can provide
- Impossible to make measurements of real system
- Protein simulations allow visualization of the movement of proteins 

### Conclusion
You should simulate too. If you want to, look into reinforcement learning papers to find how they've simulated since it's so data hungry