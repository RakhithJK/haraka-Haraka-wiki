Things that need done to Outbound.js to make it more capable

## Reloading the queue on restart with cluster

Currently on a restart, with cluster enabled, the parent process reads the queue (directory) and sets itself up to send all the mails in the queue. We should have the parent process farm out that task to each child in a round-robin fashion.

*NOTE:* This is now implemented. When mail is queued it contains the PID in the filename. When that PID dies the parent redistributes queued mail with that PID to other children. When Haraka starts it scans all the PIDs and distributes mail for each PID to a unique child (using modulus(pid)).

## Rate limits

Implement outbound rate limiting a-la plugins/rate_limit plugin.

The idea to implement this is a plugin that hooks `send_email`, `delivered` and `deferred`, along with a redis based counter. Simply increment hmail.todo.domain in `send_email` and decrement in the other hooks, and in `send_email` if the limit for a given domain is reached, return `constants.delay`.

## Implement connection pooling

e.g. pool connections by [destination MX | IP address | domain name]

*DONE* in #1466

## Tools

*DONE:* Implement tools for flushing the queue (force every item to be sent now), emptying the queue, and listing the queue.

* `haraka -c <path> --qlist`
* `haraka -c <path> --qunstick <domain>`
* TODO: `haraka -c <path> --qdelete <domain>`
* Emptying the queue is done by `rm -f <path>/queue/*`

## Improve exponential back-off algorithm on 4xx failures.

We currently just do a "multiply by two" approach to this. We should do something a bit smarter.

## Supporting and delegating to multiple nodes

Support having multiple load balanced nodes (cluster), and preferably distribute the queue between the nodes.
To support dynamic scaling of the number of nodes there should be some kind of manager on top that makes sure that all queue items are handled somewhere

## Supporting the same outgoing IP stack (IPv4 and IPv6) for common sender and recipient

Support with multiple IP stacks, to provide Google and other mail Provideren for the same recipients and senders in the same IP address for communicating. Because some mail providers have very strict guidelines here, especially with respect to the IPv6 addresses.

For this I would recommend the modulo on a CRC32 hash always choose from a pool based on either IPv4 or IPv6, depending on which destination address is selected at random. See __smf__ his example.
`crc32(array[IPv4 or v6 from MXs) + sender(from) + recipients) % modulo(outbound pool of IPv4 or v6)`

*NOTE:* This could easily be implemented in a plugin. The `get_mx` hook allows one of the return parameters (when returning an object) to be `bind`. It shouldn't be a core feature.

Manual Config e.g.:
```
[outbound_ipv4]
123.123.123.100
123.123.123.101
123.123.123.105

[outbound_ipv4]
fe80::d250:99ff:fe74:56aa
fe80::d250:99ff:fe74:57aa
fe80::d250:99ff:fe74:58aa
```
