---
layout: post
title:  "Simulations"
date:   2023-11-12 17:26:36 +0000
categories: [science]
---
Simulations are an underappreciated and underused tool in many companies. 
Most companies have multiple complex systems, many times interacting with each other, which require constant optimisation and simply need constant monitoring to understand.
We are generally afraid to touch these systems as we're unsure of the impact of the changes on it or on downstream systems.
If we can build simulations that can correspond well enough to these systems to answer important questions about the systems, then we can more freely experiment in that sandbox environment and understand our systems more completely.
Successes at large companies are out there ([Amazon](https://d1.awsstatic.com/events/Summits/reinvent2022/INO105_Supply-chain-and-logistics.pdf) and [Doordash](https://doordash.engineering/2022/08/16/4-essential-steps-for-building-a-simulator/) are two examples).
I want to argue that teams that are implementing Machine Learning products are the same teams that have complex enough systems to warrant simulating, but are also the same teams with the skill sets to build and use simulations.
In this post I want to go through the different components of a simulation, how to ensure your simulation can answer your questions without overengineering, and also ...

## What is a simulation
> A simulation is the imitation of the operation of a real-world process or system over time

_[INTRODUCTION TO SIMULATION, Jerry Banks](https://dl.acm.org/doi/pdf/10.1145/324138.324142)_

I think of a simulations as trying to reproduce reality in a simpler, modifiable setting. 
A very tangible example of a simulation is the [Mississippi River Basin model](https://en.wikipedia.org/wiki/Mississippi_River_Basin_Model). 
This was built in the 1940s to aid systematic understanding of flood control measures that had been built on the Mississippi river in the previous decade.
These locks, run-off channels and levees could prevent local flooding but may increase flooding in other areas so it was clear that a big picture understanding of how to implement these measures was needed.
Various segments of the Mississippi River became operational in the simulation throughout the 1950s, helping to avoid damage of an estimated $65 million in 1952 along (almost a billion dollars in today's money)
Tests on individual problems were conducted until 1971 but high costs and growth of computer modelling meant that the facility was put on standby.
 ![img.png](/assets/images/simulations/MissBasinModel_Color_Aerial_800x538.jpg) ![img.png](/assets/images/simulations/Mississippiriver-new-01.png)

We can use this example to explore the different parts that make up a simulation.
### What makes up a simulation
It's useful to separate the components of a simulation into three different parts: `Input`, `Mechanism`, and `Output`

 ![img.png](/assets/images/simulations/components.png)

For example in the Mississippi River Basin Model, the `Input` is the amount of water added at upstream points, and the `Output` is the water heights at downstream points at various times.
The `Mechanism` is a literal recreation of the landscape at reduced scales, and the simulation propagates along using real physics (gravity, hydrodynamics etc).
Many successful simulations take advantage of re-using parts of the real system for their mechanism.
An important part of the mechanism are the configuration parameters, which in this case would be the various settings of the dams and levees.
Many different simulations can be run with various `Inputs` and `Mechanism` config parameters to obtain `Outputs`.

![img.png](/assets/images/simulations/components-for.png)

Depending on which part of the simulation is generating *new* data, the simulation is used for different purposes. 
For many practical applications, simulations are used for Prediction, i.e. the Mississippi River Basin Model was mostly used to predict what `Output`s would arise from various rainfall levels and flood control measure settings.

For more theoretical applications, simulations are used for Explanation. Given `Inputs` that produce known `Outputs` in reality, if a `Mechanism` can reproduce these we can gain confidence that this corresponds to reality.
This can be particularly useful in applications where fine-grained measurements of reality aren't possible. Imagine in 1950s that scientists wanted to understand the mechanism of water flow in remote regions of the Mississippi basin.
Before satellites this measurement would have been difficult, but using a simulation these scientists could examine this in fine detail.
In computer simulations, scientists may try to simplify the mechanism to the bare minimum required to reproduce known `Outputs` from `Inputs`, thus  gaining theoretical understanding of their domain.

A less common application is to use simulations for Retrodiction. An interesting example of this is the theory that a [planet known as Theia](https://en.wikipedia.org/wiki/Theia_(planet)) smashed into Earth billions of years ago which resulted in the formation of our Moon.
Many different `Inputs` (planet sizes, speeds etc) were trialed to find ones that match an `Output` of an Earth and Moon systems like ours (with the `Mechanism` being a physics engine)

### Mechanism
Dive into disease modelling
#### time series
https://www.sciencedirect.com/science/article/pii/S0960077920303441

![img.png](/assets/images/simulations/timeseries-disease.png)
#### compartment
![img.png](/assets/images/simulations/compartment-disease.png)

https://covid19.uclaml.org/model.html 
https://en.wikipedia.org/wiki/Mathematical_modelling_of_infectious_diseases

#### agents
 ![img.png](/assets/images/simulations/agent-disease.png)
https://pubmed.ncbi.nlm.nih.gov/16642006/



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