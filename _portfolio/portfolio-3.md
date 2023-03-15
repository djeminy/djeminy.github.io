---
title: "Open Multithreaded Transactions"
excerpt: "Open Multithreaded Transactions (OMTT) are an advanced transaction model that provides features for controlling and structuring not only access to objects, as usual in transaction systems, but also threads taking part in transactions. Due to the isolation property and disciplined exception handling, OMTTs constitute ideal units of fault tolerance for structuring the execution of loosely coupled cooperative and competitive concurrent systems."
collection: portfolio
---

The Open Multithreaded Transaction (OMTT) model is a transaction model that provides features for controlling and structuring not only accesses to objects, as usual in transaction systems, but also threads taking part in transactions. The model allows several threads to enter the same transaction in order to perform a joint activity. It provides a flexible way of manipulating threads executing inside a transaction by allowing them to be forked and terminated, but it restricts their behavior in order to guarantee correctness of transaction nesting and isolation among transactions. Lightweight and heavyweight concurrency are treated in the same way in the open multithreaded transaction model, meaning that what is called thread here might as well be a process executed on a single machine or in a distributed setting.

OPTIMA is an object-oriented framework that provides the necessary runtime support for open multithreaded transactions. AspectOPTIMA is a purely aspect-oriented implementation of the framework.

The auction system case study has shown how the inherent complexity of a dynamic, distributed, concurrent application can be reduced by structuring the execution with open multithreaded transactions. Reasoning about fault tolerance issues and consistency of the overall system is also made a lot easier. Without open multithreaded transactions, the application programmer had to deal with threads, object creation and deletion, and exception propagation in an ad hoc way, making the process error prone.

Due to the isolation property and disciplined exception handling, open multithreaded transactions act as firewalls for errors, since they can not propagate to the outside. This fact, together with the ability to nest open multithreaded transactions, makes them ideal units of fault tolerance.

