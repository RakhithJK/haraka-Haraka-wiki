Things that need done to Outbound.js to make it more capable

## Reloading the queue on restart with cluster

Currently on a restart, with cluster enabled, the parent process reads the queue (directory) and sets itself up to send all the mails in the queue. We should have the parent process farm out that task to each child in a round-robin fashion.

## Rate limits

Implement outbound rate limiting a-la plugins/rate_limit plugin.

## Tools

Implement tools for flushing the queue (force every item to be sent now), emptying the queue, and listing the queue.

## Improve exponential back-off algorithm on 4xx failures.

We currently just do a "multiply by two" approach to this. We should do something a bit smarter.
