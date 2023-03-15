---
title: "Dependablity-focused Requirements Engineering"
excerpt: "At the requirements level, discovering and documenting all possible abnormal situations and irregular user behavior that can interrupt normal system interaction is of tremendous importance in the context of dependable system development. We defined a Dependability-focused Requirements Engineering Process (DREP) based on use cases that leads a developer to discover and then specify the required level of system reliability and safety at an early stage. Our “exceptional use cases” can be probabilistically analyzed to get feedback on the achievable safety and reliability of the system, if it were to be built with a given set of (potentially failing) components."
collection: portfolio
---

Discovering and documenting potential abnormal situations and irregular user behavior that can interrupt normal system interaction is of tremendous importance in the context of dependable systems development. Exceptional situations that are not identified during requirements elicitation might eventually lead to an incomplete system specification during analysis, and ultimately to an implementation that lacks certain functionality, or even behaves in an unreliable way.

DREP, our Dependability-focused Requirements Elicitation Process, is a development process that systematically guides the developer to consider reliability and safety concerns of reactive systems. After the discovery of normal system behavior by means of use cases, the developer is lead to explore exceptional situations arising in the environment that change the context in which the system operates and service-related exceptional situations that threaten to fail user goals. The process requires the developer to specify means that detect such situations, and to define the recovery measures that attempt to put the system in a reliable and safe state. The process is iterative, and refinements are carried out, if necessary, to achieve desired quality levels. The individual steps of our process are outlined in the flow chart below.

<img src="/images/drep.png"><br>

A dependable software system should attempt to at least partially satisfy user goals if full service provision is impossible due to an exceptional situation. In addition, a dependable system should evaluate the effects of the exceptional situation on future service provision and adjust the set of services it promises to deliver accordingly.

This is why DREP has explicitly defined tasks that discovering and specify degraded outcomes, i.e. well specified partial outcomes that can be provided by the system if the requested service can not be achieved due to some problem.

If an exceptional situation can lead to the degradation of provision of future services or if the system safety is threatened, DREP advocates to switch the system to an exceptional mode of operation in which only those services are offered that can be provided with satisfying reliability and safety. Apart from increasing system reliability and safety by not attempting to provide potentially problematic services, user dissatisfaction is also prevented. The system does not promise what it cannot deliver.