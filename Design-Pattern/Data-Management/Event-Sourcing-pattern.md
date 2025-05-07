# Event Sourcing pattern

Created: 2018-12-05 00:53:17 -0600

Modified: 2019-12-09 12:21:32 -0600

---

Event Sourcing pattern





Context and problem

Most applications work with data, and the typical approach is for the application to maintain the current state of the data by updating it as users work with it.



For example, in the traditional create, read, update, and delete (CRUD) model

a typical data process is to read data from the store, make some modifications to it,

and update the current state of the data with the new values---often by using transactions that lock the data.



The CRUD approach has some limitations:



CRUD systems perform update operations directly against a data store 1. can slow down performance and responsiveness,



2. limit scalability

3. (from CRUD, only when you can afford it) Looses the context of the change

(e.g. the contact info changed because the contact has another function in the organization or the contact info changed because the department was renamed)



(condition to trigger the action)

Implies dependencies you may not want (e.g. the sales system refused the order because the price is not the same anymore, even though it has been reduced)



Omits dependencies you may need (e.g. only place the order if the time to deliver is less than 10 work days)



(only the good return can update the address)





Solution



Event recorded in an append-only store.



Application code sends a series of events that imperatively describe each action that has occurred on the data to the event store, where they're persisted.



Each event represents a set of changes to the data (such as AddedItemToOrder).



The events are persisted in an event store that acts as the system of record (the authoritative data source) about the current state of the data.





Typical uses of the events published by the event store are to maintain materialized views of entities as actions in the application change them, and for integration with external systems.



For example, a system can maintain a materialized view of all customer orders that's used to populate parts of the UI.

As the application adds new orders, adds or removes items on the order, and adds shipping information, the events that describe these changes can be handled and used to update the materialized view.





The Event Sourcing pattern provides the following advantages:



Events are immutable and can be stored using an append-only operation.



The user interface, workflow, or process that initiated an event can continue, and tasks that handle the events can run in the background.





Events are simple objects that describe some action that occurred, together with any associated data required to describe the action represented by the event.

Events don't directly update a data store.



~~Events typically have meaning for a domain expert, whereas object-relational impedance mismatch can make complex database tables hard to understand.~~

~~Tables are artificial constructs that represent the current state of the system, not the events that occurred.~~







The append-only storage of events provides an audit trail that can be used to monitor actions taken against a data store, regenerate the current state as materialized views or projections by replaying the events at any time, and assist in testing and debugging the system. In addition, the requirement to use compensating events to cancel changes provides a history of changes that were reversed, which wouldn't be the case if the model simply stored the current state. The list of events can also be used to analyze application performance and detect user behavior trends, or to obtain other useful business information.



The event store raises events, and tasks perform operations in response to those events. This decoupling of the tasks from the events provides flexibility and extensibility.



This enables easy integration with other services and systems that only listen for new events raised by the event store.





ISSUE





There's some delay between an application adding events to the event store as the result of handling a request, the events being published, and consumers of the events handling them.

During this period, new events that describe further changes to entities might have arrived at the event store.





The consistency of events in the event store is vital, as is the order of events that affect a specific entity (the order that changes occur to an entity affects its current state).

Adding a timestamp to every event can help to avoid issues.





Even though event sourcing minimizes the chance of conflicting updates to the data, the application must still be able to deal with inconsistencies that result from eventual consistency

and the lack of transactions.For example, an event that indicates a reduction in stock inventory might arrive in the data store while an order for that item is being placed, resulting in a requirement to reconcile the two operations either by advising the customer or creating a back order.





When to use this pattern



When you want to capture intent, purpose, or reason in the data. For example, changes to a customer entity can be captured as a series of specific event types such as Moved home, Closed account, or Deceased.



When it's vital to minimize or completely avoid the occurrence of conflicting updates to data.



When you want to record events that occur, and be able to replay them to restore the state of a system, roll back changes, or keep a history and audit log. For example, when a task involves multiple steps you might need to execute actions to revert updates and then replay some steps to bring the data back into a consistent state.





whe not use this



Systems where consistency and real-time updates to the views of the data are required.





Summary
