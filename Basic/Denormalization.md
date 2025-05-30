# Denormalization



---

Denormalization attempts to improve read performance at the expense of some write performance.



Redundant copies of the data are written in multiple tables to avoid expensive joins.



Some RDBMS such as [PostgreSQL](https://en.wikipedia.org/wiki/PostgreSQL) and Oracle support [materialized views](https://en.wikipedia.org/wiki/Materialized_view) which handle the work of storing redundant information and keeping redundant copies consistent.





Once data becomes distributed ~~with techniques such as [federation](https://github.com/donnemartin/system-design-primer/blob/master/README.md#federation) and [sharding](https://github.com/donnemartin/system-design-primer/blob/master/README.md#sharding),~~ managing joins across data centers further increases complexity. Denormalization might ~~circumvent the need for such complex joins.~~

Be a solution



A read resulting in a complex database join can be very expensive
