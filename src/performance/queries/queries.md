---
en_title: Performance Best Practices - Queries
---

# Performance Best Practices - Queries

In multi-tier applications, the bottleneck is often situated at the database level, namely in the queries. The importance of these practices is more notable when dealing with external/linked servers. Specific knowledge about the database engine will help in optimizing your queries. However, if you follow the next set of practices, you’ll considerably diminish your application’s performance drawbacks.

## Don’t do joins over linked servers

### Description

Cross server joins are very inefficient. Avoid them at all costs.

### Solution

When using linked servers, avoid the cross server joins.

### Importance

The table in the linked server is completely loaded to the DB server and then the join is performed.

### Remarks

Inner joins with foreign entities tend to result in slow queries. However, if you must make a join over a linked server, make sure to use additional logic so that only a data subset is pulled from the linked server and only what was pulled will be joined (temporary tables, OPENQUERY, ...). Whatever the case, joins over linked servers need to be addressed very carefully.

## Minimize the number of fields fetched from the database

### Description

Avoid passing large Records/RecordLists as parameters of actions because this results in the platform fetching all the queried fields.

### Solution

When dealing with actions that receive or output Record/RecordLists with a large number of attributes consider using structures instead of entities for those records. Structures will narrow down the amount of data that flows through the entire system producing significant net benefits.

### Importance

In order to minimize the size of data fetched from database and to prevent slow queries and excessive memory usage, the compiler optimizer automatically detects which fields from query result records are used. The unused fields are unnecessary and as such are not fetched from the database. However when the records of a query result are used as argument of an action the optimizer doesn't have enough information to determine which fields are used and which fields aren't. In this situation all the fields will be fetched from the database which in some cases might be a prohibitive amount of data.

### Remarks

There is a balance that needs to be taken into account between maintainability and performance. Using entity records as query results and input/output parameters usually results in highly reusable easy to change components. This best practice should only be used when dealing with entities/query results with a large number of fields.

## Keep Max Records consistent with your needs

### Description

Keep the Max Records property of Aggregates consistent with the amount of data that you're displaying.

### Solution 

When there are limitations to the amount of records that will be fetched from a query, you should fill in the Max Records property of an Aggregate accordingly to optimize the query execution time. This is especially useful in table records or when an Aggregate is used to get a single record.

### Importance

Usually, we don't need to display thousands of records in a single screen, so there's no need to get all of them from the database. Only get the amount of rows that will be displayed. This improves screen loading.

### Remarks

Setting the Max Records property in SQL elements will **not** change its SQL statement. This limit is only applied at the application level to the results returned by the database. Unlike Aggregates, you will need to add a SQL clause to the statement of the SQL element (e.g. TOP) to filter the results at the database level.

## Use SQL queries for bulk operations

### Description

When performing massive operations use SQL queries instead of using a foreach loop with Aggregates (keep in mind that Aggregates are always more built to change, though). Also updates and massive deletes are faster in SQL queries than using Entity Actions. Write queries that update as many rows as possible in a single statement rather than using multiple queries to update the same rows.

### Solution

1. If you do not need to retrieve all Entity Attributes, consider replacing the Get invocation by an Aggregate using an Identifier parameter. The Platform will optimize the query to retrieve the required attributes.

2. f you don't need to update all Entity Attributes, consider replacing the Update invocation by an SQL Query using an UPDATE SET statement, setting just the required attributes. The Platform also uses an UPDATE SET statement, but always updates every attribute.

3. If you need to delete multiple records, use an SQL Query with a single Delete instead of a For Each followed with a Delete operation for each record.

4. Use Update instead of a Delete followed by a Create .

5. Use a CreateOrUpdate instead of a Select followed by a Delete and a Create.

6. Use an SQL query with a sub query instead of an Aggregate, followed by a for-each loop over the query result with another simple query for each record.

### Importance

Every operation done against the database pays a round trip cost of communicating with the server. When doing a large number of operations the sum of all the round trip cost is quite significant; so there is a real benefit in reducing the number of operations. Also the databases are significantly more efficient at performing batch operation than many small ones. There is also a significant benefit in not retrieving information that isn't necessary.

### Remarks

This is a very important best practice, and one that is not being used very often, mostly due to lack of experience of many developers, that feel more comfortable using Aggregates and avoid SQL queries at all cost.

## Avoid using isolated Aggregates

### Description 

Never create an action to execute an Aggregate. When the query is isolated in an action by itself, the Platform can't optimize the number of fields to be retrieved.

### Solution

The compiler optimizer automatically detects the used fields. However, when calling an action whose output is an entity or record list, the entity (or entities in the case of a record list) will be fetched entirely from the DB.

### Importance

This is even true even if the action doesn’t read all the fields of the entity/record list. This will cause unnecessary database overload and unnecessary memory usage.

### Remarks

Please note that this invalidates code reusability - so, in some cases, (especially when the output is not large) it is better to have isolated queries instead of a lot of similar ones.

## Iterate only once and avoid using indexers ([ ]) over a query

### Description

Iterating more than once over a query result is not a good practice. By doing so, the query results will be copied into the memory. The same applies when using direct indexers (like query[2].value expressions).

### Solution

If conserving server memory is necessary, try to avoid doing multiple iterations (for each, table/list records) over the same query. It is better to do another query in order to execute the second iteration (Note: if DB is the bottleneck, this doesn’t apply).

### Importance

When doing multiple iterations on the same query, the query results are automatically loaded into memory. If the query returns a lot of results this may increase memory consumption substantially, especially if the page is loaded very often. Using indexers on your expression also loads the results in memory independently of the number of iterations.

### Remarks

This also applies to lists displayed on a screen. For example if you iterate a query once and then display it in a table records, your query will be copied into memory. This happens because the rendering of the table records iterates through the query results again.

## Minimize the number of executed queries

### Description

Minimize the number of executed queries. Often it is possible to fetch all necessary data in a single query execution instead of multiple ones.

### Solution

Whenever possible, group several Aggregates into a single one to avoid unnecessary trips to the database. Be especially weary of queries inside for-each loops as they usually lead to severe performance problems.

### Importance

Even the simplest of queries has to pay the round trip cost for contacting the database. Furthermore, the net cost of loading and querying data is usually lower if it's done in a single query instead of multiple queries.

### Remarks

While minimizing the number of executed queries can yield large performance gains, it is usually done at the expense of readability. So the key here is to optimize only when required. Use all Service Center reports to pinpoint the bottlenecks. Remember that fast queries won't show up on the slow queries report but, if they are executed often, their aggregate time will influence all other logs (screen logs, web service logs etc.)

## Avoid ReturnedRowCount (version 4.2 or below)

### Description

Avoid using a query only to look at the ReturnedRowCount value.

### Solution

Use an SQL query with a count instead.

### Importance

Before OutSystems Platform version 4.2, when using ReturnedRowCount the query output goes to memory. Also the Max. Records attribute was mandatory so the query could return an incorrect value.

### Remarks

This is only worth the trouble for queries that can return a large number of results.

If your OutSystems Platform version is below 4.2, please contact [OutSystems Technical Support](https://success.outsystems.com/Support/Enterprise_Customers/OutSystems_Support/01_Contact_OutSystems_technical_support) for further assistance.

