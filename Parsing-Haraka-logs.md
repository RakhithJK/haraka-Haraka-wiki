Here's a basic example of how parse summary data from Haraka logs using only core NodeJS modules:

`````javascript
#!/usr/bin/env node
var readline = require('readline');
var sum = {};
var re = /^(\S+ \d+) .* haraka\[.* disconnect .* relay=(\S) .* txns=(\d+) rcpts=(\d+)\/(\d+)\/(\d+) msgs=(\d+)\/(\d+)\/(\d+) bytes=(\d+)/;

var rl = readline.createInterface({
    input: process.stdin,
    terminal: false
});

rl.on('line', function (line) {
    var match = re.exec(line);
    if (!match) return;
    if (!sum[match[1]]) {
        sum[match[1]] = {
            in_conn:          0,
            in_txns:          0,
            in_rcpts_accept:  0,
            in_rcpts_defer:   0,
            in_rcpts_reject:  0,
            in_msgs_accept:   0,
            in_msgs_defer:    0,
            in_msgs_reject:   0,
            in_bytes:         0,
            out_conn:         0,
            out_txns:         0,
            out_rcpts_accept: 0,
            out_rcpts_defer:  0,
            out_rcpts_reject: 0,
            out_msgs_accept:  0,
            out_msgs_defer:   0,
            out_msgs_reject:  0,
            out_bytes:        0,
        };
    }

    // Count relay=Y as outbound regardless of destination
    var dir = 'in';
    if (match[2] === 'Y') dir = 'out';

    sum[match[1]][dir + '_conn']++;
    sum[match[1]][dir + '_txns']         += parseInt(match[3]);
    sum[match[1]][dir + '_rcpts_accept'] += parseInt(match[4]);
    sum[match[1]][dir + '_rcpts_defer']  += parseInt(match[5]);
    sum[match[1]][dir + '_rcpts_reject'] += parseInt(match[6]);
    sum[match[1]][dir + '_msgs_accept']  += parseInt(match[7]);
    sum[match[1]][dir + '_msgs_defer']   += parseInt(match[8]);
    sum[match[1]][dir + '_msgs_reject']  += parseInt(match[9]);
    sum[match[1]][dir + '_bytes']        += parseInt(match[10]);
});

rl.on('close', function () {
    console.log(JSON.stringify(sum, null, '\t'));
});
`````

### Example output

`````
[root@mail1-ec2 Haraka]# node harakalogsumm.js  < /var/log/maillog-20150614
{
	"Jun 10": {
		"in_conn": 2140,
		"in_txns": 1671,
		"in_rcpts_accept": 671,
		"in_rcpts_defer": 11,
		"in_rcpts_reject": 1050,
		"in_msgs_accept": 544,
		"in_msgs_defer": 5,
		"in_msgs_reject": 107,
		"in_bytes": 31548622,
		"out_conn": 460,
		"out_txns": 460,
		"out_rcpts_accept": 461,
		"out_rcpts_defer": 0,
		"out_rcpts_reject": 0,
		"out_msgs_accept": 460,
		"out_msgs_defer": 0,
		"out_msgs_reject": 0,
		"out_bytes": 1198425
	},
	"Jun 11": {
		"in_conn": 2534,
		"in_txns": 2305,
		"in_rcpts_accept": 483,
		"in_rcpts_defer": 249,
		"in_rcpts_reject": 1953,
		"in_msgs_accept": 359,
		"in_msgs_defer": 11,
		"in_msgs_reject": 108,
		"in_bytes": 20668289,
		"out_conn": 494,
		"out_txns": 494,
		"out_rcpts_accept": 502,
		"out_rcpts_defer": 0,
		"out_rcpts_reject": 0,
		"out_msgs_accept": 494,
		"out_msgs_defer": 0,
		"out_msgs_reject": 0,
		"out_bytes": 1368407
	},
	"Jun 12": {
		"in_conn": 2446,
		"in_txns": 2330,
		"in_rcpts_accept": 370,
		"in_rcpts_defer": 70,
		"in_rcpts_reject": 1916,
		"in_msgs_accept": 252,
		"in_msgs_defer": 6,
		"in_msgs_reject": 101,
		"in_bytes": 13878023,
		"out_conn": 531,
		"out_txns": 531,
		"out_rcpts_accept": 535,
		"out_rcpts_defer": 0,
		"out_rcpts_reject": 0,
		"out_msgs_accept": 531,
		"out_msgs_defer": 0,
		"out_msgs_reject": 0,
		"out_bytes": 743335
	},
	"Jun 13": {
		"in_conn": 844,
		"in_txns": 768,
		"in_rcpts_accept": 238,
		"in_rcpts_defer": 20,
		"in_rcpts_reject": 512,
		"in_msgs_accept": 103,
		"in_msgs_defer": 0,
		"in_msgs_reject": 128,
		"in_bytes": 11109869,
		"out_conn": 442,
		"out_txns": 442,
		"out_rcpts_accept": 442,
		"out_rcpts_defer": 0,
		"out_rcpts_reject": 0,
		"out_msgs_accept": 442,
		"out_msgs_defer": 0,
		"out_msgs_reject": 0,
		"out_bytes": 294966
	},
	"Jun 14": {
		"in_conn": 271,
		"in_txns": 81,
		"in_rcpts_accept": 21,
		"in_rcpts_defer": 0,
		"in_rcpts_reject": 60,
		"in_msgs_accept": 6,
		"in_msgs_defer": 0,
		"in_msgs_reject": 14,
		"in_bytes": 440636,
		"out_conn": 78,
		"out_txns": 78,
		"out_rcpts_accept": 78,
		"out_rcpts_defer": 0,
		"out_rcpts_reject": 0,
		"out_msgs_accept": 78,
		"out_msgs_defer": 0,
		"out_msgs_reject": 0,
		"out_bytes": 51468
	}
}
`````