[Talk List](./README.md)
# DOP328 - Five lessons from principal engineers on building reliable services

In this session, five Amazon principal engineers share hard-learned lessons from their experiences building reliable services at Amazon. Join Andrew Certain, Becky Weiss, Colm MacCarthaigh, David Yanacek, and Marc Brooker as they share personal stories that highlight a current Amazon best practice. The engineers discuss how Amazon uses timeouts, how we think about back-offs and retries, our approach to taking dependencies, how we measure performance, and how we use shuffle sharding.


## Monitoring with percentiles
- P-four-nines = 99.99th Percentile
    - Value that is >= to 99.99% Percentile
- Latency versus Abandon Rate
- If database was not adding latency, everything looked good
- If database was adding latency, overall abandon rate went up, even for fast loading pages
- No more than 1 in 10000 pages can be served with 6 seconds of latency
- Cloudwatch supports percentiles
- embedded-metric format log in Lambda

## Static stability
- Dependencies, we all have them
- Dependencies reduce availability
- Which is more important, control or data plane?
    - Data plane is required for EC2 to provide service, control plane is still important, but more for scaling up and down
- Which is more complex, control or data plane?
    - Control Plane is more complex
- Cache data from dependencies to add more consistency in face of dependency failure, allows for static stability\
    - Not getting updates, but can still work for static operations
- Takeaways
    - Consider the availability targets of your dependencies
    - Use static stability techniques to avoid changing behavior during an impairment of your dependency
    - Dependency != Destiny

## Shuffle sharding
- First class experience on shared (economies of scale) infrastructure
- horizontally scaled infrastructure, even distribution. 
- failure storm can cascade across other workers
- scope of impact is that every customer will be impacted
    - > scope = |customers|
- Ordinary sharding can work to solve this, keeping customers in specific shards
    - May have a customer taken out of service, but will be limited to a specific shard
    - > scope = |customers| / |shards|
- Shuffle sharding 
    - distribute customers to a randomly assigned pair of nodes
    - customer combination is unique to its pair of notes
    - application needs fault tolerance or retry logic
     - > scope = shardsize! / |nodes|!
    - nodes = 100 and shard size = 5 leads to blast radius probability of 0.0000013%
    - needs client that is fault tolerant
    - works for servers, queues, and other resources
    - needs a routing mechanism: Per-customer DNS names, per-resource DNS names, or a shuffle sharding aware router

## Retries, backoff, and jitter
- Failures happen, we want our system to be more available than a single node
- Resiliency through redundancy
- Retries
    - allow us to achieve higher availability through redundancy
    - problem is that they can also DoS your own service
    - solve for this by adding latency through exponential backoff (eg. `Thread#sleep()`)
    - problem is backed off clients that come at the same initial time will return at the same time
    - computers are very precise, so backed off clients are likely to return at same time
- Jitter
    - Adding small amount of randomness improves throughput without any coordination
    - > randint(0, base * 2 * attempt)
    - "Always" try to jitter when using backoff
    - "Always" jitter periodic work (timers, cron, etc.)
    - Consider adding jitter to all work
    - Single minute could look like performance is poor (and CPUs are hot)
    - millisecond timestamps showed that services were hammered in first milliscond of minute, then retries would keep CPU high
    - computers are precise and humans like round numbers, so spikes in performance are likely
    - some services will jitter their heap size (not just for time)

## Meta lesson: Learn from each other
- Written communication can be an advantage over a talk
    - Blog Posts
    - Papers
    - Articles
- Amazon Builders' Library
    - How Amazon builds and operates software
    - Architecture, software delivery, and operations
    - By Amazon's senior technical executives and engineers
    - Real world practices with detailed explainations
    - Content available for free on the website