Things that need done to Outbound.js to make it more capable

## Reloading the queue on restart with cluster

Currently on a restart, with cluster enabled, the parent process reads the queue (directory) and sets itself up to send all the mails in the queue. We should have the parent process farm out that task to each child in a round-robin fashion.

NOTE: This is now implemented. When mail is queued it contains the PID in the filename. When that PID dies the parent redistributes queued mail with that PID to other children. When Haraka starts it scans all the PIDs and distributes mail for each PID to a unique child (using modulus(pid)).

## Nominate a specific child to handle destination domain traffic

Using something like node-hash-ring to decide which Haraka child handles outbound traffic for a destination domain would make it easier to limit concurrency and pool connections by domain and would make running a queue for a specific domain easier (this could be done via IPC from the master process.

## Rate limits

Implement outbound rate limiting a-la plugins/rate_limit plugin.

## Implement connection pooling

e.g. pool connections by [destination MX | IP address | domain name] 

## Tools

Implement tools for flushing the queue (force every item to be sent now), emptying the queue, and listing the queue.

## Improve exponential back-off algorithm on 4xx failures.

We currently just do a "multiply by two" approach to this. We should do something a bit smarter.

## Supporting and delegating to multiple nodes

Support having multiple load balanced nodes (cluster), and preferably distribute the queue between the nodes.
To support dynamic scaling of the number of nodes there should be some kind of manager on top that makes sure that all queue items are handled somewhere