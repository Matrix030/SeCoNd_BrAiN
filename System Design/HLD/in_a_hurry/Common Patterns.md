## Pushing Realtime Updates

1) In many systems, you'll need to be able to make updates to the user in real-time. 
2) For synchronous APIs, this is as simple as returning a response once the request is completed. 
3) For other systems like chat applications, notifications, or live dashboards, you'll need to be able to push updates to the user as they happen.

There are a lot of decisions to make when implementing realtime updates. First, you'll need to choose a protocol.
1) Simple HTTP polling is the simplest option, but it's not the most efficient.
2) Server-sent events (SSE) and websockets are purpose-built for realtime updates, but the infrastructure can be tricky to get right.
3) For the server side of realtime updates, you again have more options, Pub/Sub services are a common way to decouple the publisher and subscriber, while stateful servers in a consistent hash ring or other configuration can be used for situations where processing is heavier.
![[Pasted image 20260417222829.png]]

## Managing Long-Running Tasks
Many operations in distributed systems take too long for synchronous processing - video encoding, report generation, bulk operations, or any task that takes more than a few seconds.

1) When users submit heavy tasks, your web server instantly validates the request, pushes a job to a queue (like Redis or Kafka), and returns a job ID within milliseconds.
2) Separate worker processes continuously pull jobs from the queue and execute the actual work.
3) This provides fast user response times, independent scaling of web servers and workers, and fault isolation.

> [!warning]
> It's a bad decision to pull the trigger on pushing their processing behind a queue, If you have short-running jobs, returning the status of the job synchronously with the request simplifies your architecture dramatically providing clearer back-pressure and better user experience.

The key technologies are message queues for job coordination and worker pools for processing. You'll need to handle job status tracking, retries, and failure scenarios like dead letter queues for poison messages.

![[Pasted image 20260417224020.png]]

## Dealing with Contention

1) When multiple users try to access the same resource simultaneously, like booking the last concert ticket or bidding on an auction item, you need mechanisms to prevent race conditions and ensure data consistency. This pattern addresses coordination challenges in distributed systems.
2) The key is understanding when to use atomicity and transactions versus explicit locking strategies.
3) For distributed systems, you might need distributed locks, two-phase commit protocols, or queue-based serialization.
4) Trade-offs include performance versus consistency guarantees, and simple database solutions versus complex distributed coordination.

> [!tip]
> Databases are built around problems of contention. When you separate your data into multiple databases, you're taking on all of the challenges that the database systems were originally designed to solve. In some cases this can be completely appropriate, but be careful about doing it prematurely.

![[Pasted image 20260417230024.png]]


## Scaling Reads

1) As your application grows from hundreds to millions of users, read traffic often becomes the first bottleneck. 
2) While writes create data, reads consume it - and read traffic typically grows much faster than write traffic. 
3) The Scaling Reads pattern addresses high-volume read requests through database optimization, horizontal scaling, and intelligent caching.
4) For most applications, the read-to-write ratio starts at 10:1 but often reaches 100:1 or higher.
5) Consider Instagram: when you open the app, you see dozens of photos requiring hundreds of database queries for metadata, user info, and engagement data. Meanwhile, you might only post once per day - a single write operation.
6) The **solution** follows a natural progression: optimize read performance within your database through indexing and denormalization, scale horizontally with read replicas, then add external caching layers like Redis and CDNs.
7) Key considerations include managing cache invalidation, handling replication lag in read replicas, and dealing with hot keys where millions of users request the same popular content simultaneously.
![[Pasted image 20260417232307.png]]


## Scaling Writes

1) As your application grows from hundreds to millions of writes per second, individual database servers and storage systems hit hard limits. 
2) The Scaling Writes pattern addresses write bottlenecks through [sharding](https://www.hellointerview.com/learn/system-design/core-concepts/sharding), batching, and intelligent load management.
3) The core strategies are horizontal [sharding](https://www.hellointerview.com/learn/system-design/core-concepts/sharding) (distributing data across multiple servers), vertical partitioning (separating different types of data), and handling write bursts through queues and load shedding.
4) Key considerations include selecting good partition keys that distribute load evenly while keeping related data together.
5) For burst handling, you can use write queues to buffer temporary spikes or implement load shedding to prioritize important writes during overload.
6) Batching techniques help reduce per-operation overhead by grouping multiple writes together.

![[Pasted image 20260417233159.png]]


## Handling Large Blobs

1) Large files like videos, images, and documents need special handling in distributed systems. 
2) Instead of routing gigabytes through your application servers, this pattern uses direct client-to-storage transfers with presigned URLs and CDN delivery.
3) Your application server generates temporary, scoped credentials (presigned URLs) that let clients upload directly to blob storage like S3. 
4) Downloads come from CDNs with signed URLs for access control.
5) This eliminates your servers as bottlenecks while providing resumable uploads, progress tracking, and global distribution.
6) **Key challenges** include state synchronization between your database metadata and blob storage, handling upload failures, and managing the lifecycle of large files. 
7) Event notifications from storage services help keep your application state consistent.

## Multi-Step Processes

1) Complex business processes often involve multiple services and long-running operations that must survive failures, retries, and external dependencies.
2) This pattern provides reliable coordination for workflows like order fulfillment, user onboarding, or payment processing.
3) Solutions range from simple single-server orchestration to sophisticated workflow engines and durable execution systems.
4) Event sourcing provides a distributed approach where each step emits events that trigger subsequent steps. 
5) Modern workflow systems like Temporal or AWS Step Functions handle state management, failure recovery, and retry logic automatically.
6) The **key insight** is moving from scattered state management and manual error handling to declarative workflow definitions where the system guarantees exactly-once execution and maintains complete audit trails.
![[Pasted image 20260417234954.png]]

## Proximity-Based Services

1) Several systems like [Design Uber](https://www.hellointerview.com/learn/system-design/problem-breakdowns/uber) or [Design Gopuff](https://www.hellointerview.com/learn/system-design/problem-breakdowns/gopuff) will require you to search for entities by location.
2) Geospatial indexes are the key to efficiently querying and retrieving entities based on geographical proximity. 
3) These services often rely on extensions to commodity databases like [PostgreSQL with PostGIS extensions](https://postgis.net/) or [Redis' geospatial data type](https://redis.io/docs/latest/develop/data-types/geospatial/), or dedicated solutions like Elasticsearch with geo-queries enabled.
4) The architecture typically involves dividing the geographical area into manageable regions and indexing entities within these regions.
5) This allows the system to quickly exclude vast areas that don't contain relevant entities, thereby reducing the search space significantly.

> [!tip]
> While geospatial indexes are great, they're only really necessary when you need to index hundreds of thousands or millions of items. If you need to search through a map of 1,000 items, you're better off scanning all of the items than the overhead of a purpose-built index or service.

## Pattern Selection

1) These patterns often work together to solve complex system design challenges. A video platform might use **Large Blobs** for video uploads, **Long-Running Tasks** for transcoding, **Realtime Updates** for progress notifications, and **Multi-Step Processes** to coordinate the entire workflow.
2) The key is recognizing which patterns apply to your specific problem and understanding their trade-offs. 
3) Start with simpler approaches (polling, single-server orchestration) and only add complexity when you have specific requirements that demand it.
4) In system design interviews, proactively identifying and applying these patterns demonstrates architectural maturity and helps you focus on the most important aspects of your design rather than getting bogged down in implementation details.