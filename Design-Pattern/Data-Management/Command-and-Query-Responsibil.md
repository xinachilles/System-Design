# Command and Query Responsibility Segregation (CQRS) pattern

Created: 2018-11-04 23:49:33 -0600

Modified: 2018-11-04 23:49:54 -0600

---

split the application into two parts: the command-side and the query-side.

The command-side handles create, update, and delete requests and emits events when data changes.

The query-side handles reading reques against one or more denormalized view

the query-side kept up to date by subscribing to the stream of events emitted when data changes.

(append-only stream of events driven by execution of commands)





for command-side, we can have some business logic, input validation, and business validation to ensure that everything is always consistent for each of the aggregates



Resulting context

This pattern has the following benefits:



Necessary in an event sourced architecture

Improved separation of concerns = simpler command and query models

Supports multiple denormalized views that are scalable and performant (in many systems the number of read operations is many times greater than the number of write operations.)



differtn security rules for different part





This pattern has the following drawbacks:



Where the domain or the business rules are simple.



Where a simple CRUD-style user interface and the related data access operations are sufficient.





Increased complexity

Potential code duplication

eventually consistent views

There will be some delay between the event being generated and the data store being updated.
