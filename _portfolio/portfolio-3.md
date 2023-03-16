---
title: "Open Multithreaded Transactions"
excerpt: "Open Multithreaded Transactions (OMTT) are an advanced transaction model that provides features for controlling and structuring not only access to objects, as usual in transaction systems, but also threads taking part in transactions. Due to the isolation property and disciplined exception handling, OMTTs constitute ideal units of fault tolerance for structuring the execution of loosely coupled cooperative and competitive concurrent systems."
collection: portfolio
---

The Open Multithreaded Transaction (OMTT) model is a transaction model that provides features for controlling and structuring not only accesses to objects, as usual in transaction systems, but also threads taking part in transactions. The model allows several threads to enter the same transaction in order to perform a joint activity. It provides a flexible way of manipulating threads executing inside a transaction by allowing them to be forked and terminated, but it restricts their behavior in order to guarantee correctness of transaction nesting and isolation among transactions. Lightweight and heavyweight concurrency are treated in the same way in the open multithreaded transaction model, meaning that what is called thread here might as well be a process executed on a single machine or in a distributed setting.

OPTIMA is an object-oriented framework that provides the necessary runtime support for open multithreaded transactions. AspectOPTIMA is a purely aspect-oriented implementation of the framework.

The auction system case study has shown how the inherent complexity of a dynamic, distributed, concurrent application can be reduced by structuring the execution with open multithreaded transactions. Reasoning about fault tolerance issues and consistency of the overall system is also made a lot easier. Without open multithreaded transactions, the application programmer had to deal with threads, object creation and deletion, and exception propagation in an ad hoc way, making the process error prone.

Due to the isolation property and disciplined exception handling, open multithreaded transactions act as firewalls for errors, since they can not propagate to the outside. This fact, together with the ability to nest open multithreaded transactions, makes them ideal units of fault tolerance.

### Transaction Lifecycle

Any thread can start a transaction. A thread wishing to work on behalf of an already existing transaction can do so by joining it. In order to join, it has to learn (at run-time) or to know (statically) the identity of the transaction it wishes to join. Threads working on behalf of an open multithreaded transaction are referred to as participants. External threads that create or join a transaction are called joined participants; a thread created inside a transaction by some other participant is called a spawned participant.

Within an open multithreaded transaction, threads can access a set of transactional objects. Although individual threads evolve independently inside an open multithreaded transaction, they are allowed to collaborate with other threads of the transaction by accessing the same transactional objects.

All participants finish their work inside a transaction by voting on the transaction outcome. Possible votes are commit and abort. Voting abort is done by raising an external exception. The transaction commits if and only if all participants vote commit.

Once a spawned participant has given its vote, it terminates immediately. Joined participants are not allowed to leave a transaction, i.e. they are blocked, until the outcome of the transaction has been determined. If a participating thread "disappears" from a transaction without voting on its outcome, the transaction is aborted, as this case is treated as an error.

<img src="/images/omtt_fig1.png"><br>

The figure above shows two open multithreaded transactions: T1 and T1.1. Thread C creates the transaction T1, threads A, B and D join it. Threads A, B, C and D are therefore joined participants of the transaction T1. Inside T1 thread C forks a new thread C' (a spawned participant), which performs some work inside the transaction and then terminates after voting. Thread B also forks a new thread, thread B'. B and B' perform a nested transaction T1.1 inside of T1. B' is a spawned participant of T1, but a joined participant of T1.1. In this example, all participants of T1 vote commit. The joined participants A, C, and D are therefore blocked until the last participant, here thread B, has finished its work and given its vote.

### Exceptions

The open multithreaded transaction model incorporates disciplined exception handling adapted to nested transactions. It allows individual threads to perform forward error recovery by handling an abnormal situation locally. If local handling fails, the transaction support applies backward error recovery and reverses the system to its "initial" state.

The model distinguishes internal and external exceptions; the latter ones are also called interface exceptions. The set of internal exceptions for each participant consists of all exceptions that might occur during its execution. There are three sources of exceptions inside an open multithreaded transaction:

* An internal exception can be raised explicitly by a participant.
* An external exception raised inside a nested transaction is raised an an internal exception in the parent transaction.
* Transactional objects accessed by a participant of a transaction can raise an exception to signal a situation that violates the consistency of the state of the transactional object.

All these situations give rise to a possibly inconsistent application state. If a participant does not handle such a situation, the application's correct behavior can not be guaranteed.

A participant must therefore provide handlers for all internal exceptions. If such a handler it is not able to deal with the situation, it can signal an external exception. If any participant of a transaction signals an external exception, the transaction is aborted, the exception is propagated to the containing context, and the exception Transaction_Abort is signaled to all other joined participants. If several joined participants signal an external exception, each of them propagates its own exception to its own context.

### Transactional Objects

In the open multithreaded transactions model participants perform their work by invoking operations on transactional objects. To guarantee the ACID properties, operation invocations made by participants must be controlled at two levels: Guaranteeing the isolation property of all updates made within a transaction with respect to other transactions running concurrently is the first concern. This can be achieved using existing optimistic or pessimistic concurrency control techniques. The second concern is data consistency. To ensure correct updates, operations performed concurrently by participants of the same transaction must be executed within mutual exclusion. This can be achieved by using monitors or similar techniques found in modern concurrent languages.

Early error detection and consistency of application state is important for modern applications. Existing transactional systems mainly rely on the programmer to write a transaction in such a way that it preserves consistency. In the open multithreaded transaction model, developers of transactional objects can help the transaction programmer to write consistency-preserving code by developing so-called self-checking transactional objects, that is to introduce invariants while designing them. Methods of transactional objects are to be decorated with pre- and post-conditions. When an invariant, pre- or post-condition is violated by the execution of a method, and the object is left in an inconsistent state, an exception is propagated to the participant that has invoked the operation. The participant must then handle this internal exception in order to address this inconsistency. If handling fails, the transaction is aborted, all the changes made to transactional objects on behalf of the transaction are undone and an external exception is propagated to the calling context.
