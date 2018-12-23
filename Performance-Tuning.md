These are some pointers to improving Haraka performance when under heavy load (e.g. > 30 connection/sec or ~ 3 million connections per day).   

This is a work in progress and if you have any other tips, please feel free to add them here.

Note that all of my testing has been on recent versions of Linux, so other operating systems might perform differently, so YMMV.

## sysctl

I added the following to `/etc/sysctl.conf` and applied these by running `sysctl -p`

`````
net.core.rmem_max = 33554432
net.core.wmem_max = 33554432
net.ipv4.tcp_rmem = 4096 87380 33554432
net.ipv4.tcp_wmem = 4096 65536 33554432
net.ipv4.tcp_keepalive_time = 120 
`````

## libuv

I found additional performance by setting the following environment variables:

* `UV_TCP_SINGLE_ACCEPT=1` - this makes libuv sleep after calling `accept()` which helps with a thundering herd of connections.
* `UV_THREADPOOL_SIZE` - the default thread pool for libuv is 4 threads.  If you have node modules which call `uv__work_submit()` to do synchronous tasks, then increasing this value may help.  The maximum this can be set to is 128.

## node

### nodes

With `nodes=0` set in smtp.ini Haraka runs in a single process. If an error is encountered in Haraka or even a plugin, it can crash the entire mail server. Unless you're testing or bug hunting, this is not desirable. When `nodes=1` or higher is set, a supervisory "master" process is started. The master process takes care of making sure that there are *nodes* worker processes. If a worker crashes, the supervisor starts up a new one.

Production Haraka servers should certainly have nodes set to some value higher than zero. If the server is running only Haraka, setting `nodes=cpus` will spawn a worker for each CPU core. This will likely be very near the optimal number of workers your hardware can handle. However, beware that [cpu options impact memory use](/haraka/Haraka/issues/1442).

When running with `nodes=cpus`, setting the environment variable `NODE_CLUSTER_SCHED_POLICY=none` was found to significantly improve the amount of time before the SMTP banner was sent to a newly connected socket.   Without this it would sometimes take over 30 seconds before the SMTP banner was sent, which caused many clients to disconnect before this happened.

## Haraka

The `toobusy` plugin is recommended to prevent Haraka from becoming overwhelmed when under heavy load.

If you have any plugins that use Redis and store the Redis connection handle in `server.notes.redis`, then if you run `echo "MONITOR" | redis-cli`, you may find a lot of unexpected PUBLISH commands from Haraka.   This comes from `haraka-results`.  On high volume systems you will want to disable this behaviour by setting `redis_publish=false` in `config/results.ini`.