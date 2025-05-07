# View Pattern

Created: 2018-11-16 09:56:53 -0600

Modified: 2019-01-04 09:51:47 -0600

---

Summary of the data or just subset of the data or joining tables or combining data entities into a temporary view



This can help support efficient querying and data extraction, and improve application performance.



we ususally store the view and souce data in the different location,if the souce location is not available. we can guarantee the read function is still working



When the source data for the view changes, the view must be updated to include the new information. You can schedule this to happen automatically,

or when the system detects a change to the original data.



How and when the view will be updated. Ideally it'll regenerate in response to an event indicating a change to the source data,

Alternatively, consider using a scheduled task, an external trigger, or a manual action to regenerate the view.



In some systems, such as when using the Event Sourcing pattern to maintain a store of only the events that modified the data, materialized views are necessary.



consistency issue





The source data is simple and easy to query.

The source data changes very quickly, or can be accessed without using a view.

Consistency is a high priority. The views might not always be fully consistent with the original data.
