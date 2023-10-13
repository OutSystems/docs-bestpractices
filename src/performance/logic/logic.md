---
summary: It is the developer's responsibility to maintain a high standard of coding that always takes performance into consideration.
en_title: Performance Best Practices - Logic
guid: faee48f5-9b43-40a6-906d-837feef414df
locale: en-us
app_type: traditional web apps, mobile apps, reactive web apps
platform-version: o11
---

# Performance Best Practices - Logic

This topic is a very sensitive one since it really depends on the ability of the developer to maintain a high standard of coding that always takes performance into consideration. Some tasks must be used as a rule of thumb (they apply to all kinds of applications).

## Use Service Center Reports

### Description

Use Service Center reports to find performance problems and guide the optimization process.

### Solution

Use Service Center reports to check and tune an applications' performance.

### Why important?

In the case where you find performance bottlenecks in your applications, the Service Center Analytics section provides a full view of slow application accesses by web screens, WAP screens, web services or timers, as well as slow database queries or integrations using extensions or web services. This will allow you to immediately identify bottlenecks in your system and applications, and focus analysis efforts on those items to decrease troubleshooting times.

### Remarks

This is usually the starting point for all optimization needs.

## Avoid long-running timers and batch jobs

### Description

Background processing shouldn't be done all in a row. Use control tables and process data in chunks, rescheduling itself at the end of a run.

### Solution

Avoid having timers running for long periods. Timers should take at most 20 minutes and then reschedule themselves to start again immediately to continue the task at hand. This is accomplished by adding an explicit timeout inside the timer logic that when reached takes the necessary actions to properly terminate the current processing, store the current status of the process in such a way that when the process starts again it can seamlessly proceed from the point where it stopped. At the end of a run, the timer ends with a wake timer action for itself.

### Importance

Scenario: timer takes too long and is interrupted by a worker process recycle. Depending on the error and operation, this problem can cause data inconsistency if it is communicating with external systems. If the timer exceeds the platform's timer timeout threshold, it may result in endlessly reprocessing of the same data. Having timers awaken themselves after a short execution time will decrease the probability of (a) the timer being interrupted by a worker process recycle; (b) result in cause data inconsistency; and (c) endless reprocessing of the same data. Try to intersperse different timers to balance job processing.

### Remarks

Another good policy for long timers and batch jobs is to define them with checkpoints so that the timer could be killed and restarted with no impact to the data. At these checkpoints, consider executing partial commits to ensure that if something wrong happens with the timer, not all processed information is rolled back.

## Simplify screen preparations

### Description

Simplify the screen preparation actions to improve the screen loading speed.

### Solution

Make simple screen preparations (huge and complex preparations can make screens slow). Check if your preparations have one of the following situations:

1. Pre-processing of query results. Avoid adding new fields to the table and processing the data on the view operations. Do this instead on the write operation.

2. Queries inside for-each. Often (see topic Minimize the number of queries) executing only one complex query to obtain the required information is better than executing a simple query in a for-each loop.

3. Complex permission validations that are executed in every page. Check if they can be done once in the session start and stored in session or in the database.

### Importance

Long page loads have a high impact on the user experience. You should limit at any cost the time it takes for a page to render thus minimizing the database read operations if you can. Also, in order to improve rendering time you should not (unless strictly necessary) execute database write operations within the preparation of screens.

## Using Site properties

### Description

Avoid using site properties as logic variables that frequently change.

### Solution

Site properties should be seen as constants, not variables. If you change them on each screen or on each session consider using a database table to store site property information and the corresponding backoffice functionality for its management.

### Importance

Changing a Site Property will lead to an immediate database change which takes longer and in order to have the applications to immediately acknowledge the new value, a TenantInvalidateCache operation must be issued, causing a reload of the application's cache. Doing this constantly will increase application and database overhead.

### Remarks

If you change a site property concurrently in high load you might even see something like "transaction has been chosen as deadlock victim".

## Use Session Variables wisely

### Description

Place as little information in session as possible and especially avoid large records and record lists.

### Solution

Avoid using large session variables. An increase of the number of concurrent users in the application compounds the impact of using large session variables. An alternative to the use of session variables (for storing filters for instance) is to store it in the database table with the user id as the primary key. The difference is that instead of being fetched in every request, data is only fetched for the pages where the data is used.

### Importance

Each web request loads the current session context. If the session variables are large text strings then each request will take longer to get the session variables from the session data model which can cause contention in the session data model further impacting other application users. This is even more important when you use AJAX as it will increase the number of requests.

### Remarks

If you have a sessionless service then removing the session altogether (setting session as InPRoc) is a good way of increasing the performance.

## Avoid using isolated Aggregates

### Description

Never create an action to execute an Aggregate. When the Aggregate is isolated in an action by itself the platform can't optimize the number of fields to be retrieved.

### Solution

The compiler optimizer automatically detects the used fields. However, when calling an action whose output is an entity or record list, the entity (or entities in the case of a record list) will be fetched entirely from the DB.

### Importance

This is true even if the action doesn’t read all the fields of the entity/record list. This will cause unnecessary database overload and unnecessary memory usage.

### Remarks

Please note that this invalidates code reusability - so, in some cases (especially when the output is not large) it is better to have isolated Aggregates instead of a lot of similar ones.

## Avoid using queries inside ‘If’ branches in preparation

### Description

Avoid using the conditional queries in preparation to prevent the extra query data from going to the viewstate.

### Solution

When using refresh screens, take care with conditional queries (queries in "if" branches). To prevent this, declare a local screen variable and assign it from the query and with an empty record list in the beginning of the preparation.

### Importance

If a query is only executed inside an "if" branch in the preparation and used outside that branch (in a table records, for example), its data need to be always in viewstate. This is because the query is only executed sometimes. This increases the page viewstate size.

### Remarks

Note that this only happens if we use the submit action that terminates with an end node (causing the screen to refresh). For AJAX actions and for destinations, this does not apply.

