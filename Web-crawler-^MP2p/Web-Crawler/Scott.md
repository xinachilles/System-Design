# Scott

Created: 2021-01-18 00:54:00 -0600

Modified: 2021-01-18 00:54:42 -0600

---

Design Crawler / Archive

- Crawl Amazon/eBay/etc, the user specifies a domain name to crawl
- Automatically discover the new URL
- Periodically crawl: daily
- Smart schema detection
- Extract certain information like product description/price etc.

10 mins: Business (Use Cases)/common sense

10 mins: Constraints

10 mins: Architecture

10 mins: API Design

10 mins: DB choice

10 mins: Data model





~~Conflicts in the system design articles:~~

- ~~Fundamental not working~~
- ~~A working solution but may not be optimal~~



Goal:

- High scalable / flexibility in scale (don't need high availability
- Fault-tolerant
  - Idempotent
- Schema flexible / extensible



Not in discussion:

- Server-side oAuth2
- [Server-side throttle]{.mark} -- need politeness, have agreement





Constraints:

- Empirical results driven
- From database perspective, Data size is the limitation
  - gurugamer game replay: 20k, daily replay: 1.7M, [could be used to estimate the total data size and forecast the foreseeable future]{.mark}.
- From the whole system perspective, server end throttling is the main limitation
  - Use different accounts
  - Use different IPs
  - Multi-thread
  - Be a good citizen, don't bring down the server
  - The same node crawl several sites in parallel



Strategy

The basic algorithm executed by any Web crawler is to take a list of whitelisted domains and seed URLs as its input and repeatedly execute the following steps.

- Pick a URL from the unvisited URL list.
- Async crawl the URL
- Parse the document contents and store the data, index the contents.
- look for new URLs in the whitelisted, Add the new URLs to the list of unvisited URLs.









**Transactional / easier with rich sql query:** mysql / postgres

**Key - value / strict access patterns / flexible schema:** dynamoDB(The cumulative size of attributes per item must fit within the maximum DynamoDB item size (400 KB).), S3 if big object

SQL could scale requires some extra work: sharding.

Raw data: key-value read / write

Amazon S3 / dynamoDB / cassandra / mongodb

Meta data / clean data:

SQL: requires durability and transaction.



API

AddScheduledCrawl(domain_names, cron_expression=null);



StartCrawling(domain_names, iteration, blacklisted_URLs, **type {on_demand, schedule})**;



SQL support, transaction per second (lock) 100k qps







Data Model

Raw Data Table (S3): [(raw data could be html and image)]{.mark}

| Id   | data            | Created time |
|------|-----------------|--------------|
| uuid | HTML / JSON... | 21435343     |



Clean Data:

<table>
<colgroup>
<col style="width: 21%" />
<col style="width: 25%" />
<col style="width: 24%" />
<col style="width: 15%" />
<col style="width: 13%" />
</colgroup>
<thead>
<tr>
<th><p>Hash(URL, iteration)</p>
<p>(shard key)</p></th>
<th>type</th>
<th>URL</th>
<th>iteration</th>
<th>field_1</th>
</tr>
</thead>
<tbody>
<tr>
<td>string</td>
<td>Game_replay</td>
<td>steam.com/1</td>
<td>10</td>
<td>Hero: Riki</td>
</tr>
</tbody>
</table>



<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 11%" />
<col style="width: 19%" />
<col style="width: 15%" />
<col style="width: 16%" />
<col style="width: 18%" />
</colgroup>
<thead>
<tr>
<th><p>Hash(URL, iteration)</p>
<p>(shard key)</p></th>
<th>type</th>
<th>URL</th>
<th>iteration</th>
<th>headline</th>
<th>main_text</th>
</tr>
</thead>
<tbody>
<tr>
<td>string</td>
<td>News</td>
<td>cnn.com/1</td>
<td>1</td>
<td>Uber</td>
<td>What’s new?</td>
</tr>
</tbody>
</table>







Metadata:

Scheduling table:

| Id   | Cron         | user  | Creation | Name    | URLs         | state             |
|--------|---------|--------|------------|-----------|-----------|---------------|
| uuid | 0 8 * * * | scott | 214145   | replays | Link to YAML | {ON, PAUSED, OFF} |

Iteration progress





Iteration progress

Primary key: schedule_id + iteration

| schedule | iteration | start  | Urls found | Stuck | crawled | processed |
|----------|-----------|--------|------------|-------|---------|-----------|
| uuid     | 9         | 214145 | 10000      | 10    | 9000    | 8600      |

Record metadata

shard key : Id = Hash(URL, iteration) idopoent

| id  | URL       | iteration | state  | create_time | last_update | notified | Deleted |
|----|-----------|---------|----------|------------|------------|--------|---------|
| str | games.net | 9         | QUEUED | 123431      | 10          | No       |        |



STATE = {Queued, Crawled, Processed, Stuck}

Fault Tolerant

Scheduler(Apache Airflow):

1.  Read YAML: what to crawl
2.  Insert seed in records metadata, no-op if exists
    1.  Send and forget to crawler
3.  Insert progress in progress table, no-op if exists
4.  If fault, go back to state 1.

[Heavy load processing usually stateless, send & forget]{.mark}

Crawler: stateless

- Read the URL need to be crawled, if already crawled, no-op
- Crawl the URL, store data into raw data table
- Update record to crawled



Parser: stateless

- Read the record need to parsed (get message from queue), if already processed / stuck , no-op
- Parse and validate record:
  - If invalidate, update record to STUCK and return
- Store the validated record to clean data
- Update record state to PROCESSED

Sweeper: periodically wake up

- Read records in STUCK state and not yet notified
  - Notify developers to take actions.
  - Mark the record notified
- Read the records in Queued for a long period of time
  - Re-queue a crawl command
- Read the records in Crawled for a long period of time
  - Re-queue a parse command
- Update the iteration progress records:
  - Send summary notification to users.
  - May need to consolidate if cross shards.



Infinite scalable

Q & A


