These are some pointers to improving Haraka performance when under heavy load (e.g. > 30 connection/sec or ~ 3 million connections per day).   

This is a work in progress and if you have any other tips, please feel free to add them here.

Note that all of my testing has been on recent versions of Linux, so other operating systems might perform differently, so YMMV.

## syctl

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

