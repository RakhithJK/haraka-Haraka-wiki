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


### Current ideas (_smf_)

Rate/concurrency limits and different queue timers per target are hard because currently Haraka handles each message on the same PID that processed the message.

Ideally - we want to distribute the outbound load amongst each cluster PID, but to control rate/concurrency, we would also want traffic to a specific destination to always be handled my the same PID.

This had me thinking that we could take all of the IPs from an MX lookup for a domain and then generate a CRC32 which we could modulo against the number of workers  e.g. `crc32(mx_ips) % modulo(cluster_workers)`, that way you would always end up with the same CRC32 for a given set of IP addresses and they would always be handled by the same cluster PID, so rate/concurrency becomes easier as you'd set it by the CRC32 value.   You can also maintain a retry queue for each CRC32 value, so flushing the queue also becomes much easier.

Rough Example code to generate the CRC32:
`````
'use strict';
var dns = require('dns');
var async = require('async');
var crc = require('crc');

exports.dns_get_a_aaaa_records = function (label, cb) {
    var err = {};
    async.parallel({
        a: function (callback) {
            dns.resolve4(label, function (err, res) {
                if (err) err.a = err;
                callback(null, res);
            });
        },
        aaaa: function (callback) {
            dns.resolve6(label, function (err, res) {
                if (err) err.aaaa = err;
                callback(null, res)
            });
        }}, 
        function (err, res) {
            var results = [];
            if (Array.isArray(res.a) && res.a.length) {
                results = results.concat(res.a);
            }
            if (Array.isArray(res.aaaa) && res.aaaa.length) {
                results = results.concat(res.aaaa);
            }
            if (results.length) {
                return cb(null, results);
            }
            else {
                return cb(new Error('no results returned'));
            }
        }
    );
}

exports.dns_get_mx_records = function (domain, cb) {
    dns.resolveMx(domain, function (err, res) {
        if (err) return cb(err);
        if (res) {
            var mxes = [];
            for (var i=0; i<res.length; i++) {
                mxes.push(res[i].exchange);
            }
            async.map(mxes, exports.dns_get_a_aaaa_records, function (err, results) {
                if (err) return cb(err);
                // Flatten addresses into a single array
                var ips = [];
                if (results) {
                    for (var i=0; i<results.length; i++) {
                        ips = ips.concat(results[i]);
                    }
                    return cb(null, ips);
                }
                cb(null);
            });
        }
        else {
            return cb(null);
        }
    });
};

exports.dns_get_mx_records('google.com', function (err, results) {
    if (err) {
        console.log(err);
    }
    else {
        console.log('results: ' + JSON.stringify(results));
        console.log('crc32: ' + crc.crc32(results));
        var sorted = results.sort();
        console.log('crc32 (sorted): ' + crc.crc32(sorted));
    }
});
`````
