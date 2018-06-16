# ORY Hydra Performance Benchmarks

In this document you will find benchmark results for different endpoints of ORY Hydra. All benchmarks are executed
using [rakyll/hey](https://github.com/rakyll/hey). Please note that these benchmarks run against the in-memory storage
adapter of ORY Hydra. These benchmarks represent what performance you would get with a zero-overhead database implementation.

We do not include benchmarks against databases (e.g. MySQL or PostgreSQL) as the performance greatly differs between
deployments (e.g. request latency, database configuration) and tweaking individual things may greatly improve performance.
We believe, for that reason, that benchmark results for these database adapters are difficult to generalize and potentially
deceiving. They are thus not included.

This file is updated on every push to master. It thus represents the benchmark data for the latest version.

All benchmarks run 10.000 requests in total, with 100 concurrent requests. All benchmarks run on Circle-CI with a
["2 CPU cores and 4GB RAM"](https://support.circleci.com/hc/en-us/articles/360000489307-Why-do-my-tests-take-longer-to-run-on-CircleCI-than-locally-)
configuration.

## BCrypt

ORY Hydra uses BCrypt to obfuscate secrets of OAuth 2.0 Clients. When using flows such as the OAuth 2.0 Client Credentials
Grant, ORY Hydra validates the client credentials using BCrypt which causes (by design) CPU load. CPU load and performance
depend on the BCrypt cost which can be set using the environment variable `BCRYPT_COST`. For these benchmarks,
we have set `BCRYPT_COST=8`.

## OAuth 2.0

This section contains various benchmarks against OAuth 2.0 endpoints

### Token Introspection

```

Summary:
  Total:	1.1495 secs
  Slowest:	0.0708 secs
  Fastest:	0.0002 secs
  Average:	0.0111 secs
  Requests/sec:	8699.6691
  
  Total data:	1550000 bytes
  Size/request:	155 bytes

Response time histogram:
  0.000 [1]	|
  0.007 [3095]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.014 [4119]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.021 [2238]	|■■■■■■■■■■■■■■■■■■■■■■
  0.028 [370]	|■■■■
  0.036 [115]	|■
  0.043 [31]	|
  0.050 [15]	|
  0.057 [12]	|
  0.064 [2]	|
  0.071 [2]	|


Latency distribution:
  10% in 0.0018 secs
  25% in 0.0059 secs
  50% in 0.0118 secs
  75% in 0.0147 secs
  90% in 0.0184 secs
  95% in 0.0220 secs
  99% in 0.0323 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0002 secs, 0.0708 secs
  DNS-lookup:	0.0001 secs, 0.0000 secs, 0.0171 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0227 secs
  resp wait:	0.0106 secs, 0.0002 secs, 0.0463 secs
  resp read:	0.0002 secs, 0.0000 secs, 0.0213 secs

Status code distribution:
  [200]	10000 responses



```

### Client Credentials Grant

This endpoint uses [BCrypt](#bcrypt).

```

Summary:
  Total:	24.5650 secs
  Slowest:	0.9037 secs
  Fastest:	0.0207 secs
  Average:	0.2385 secs
  Requests/sec:	407.0832
  
  Total data:	1570000 bytes
  Size/request:	157 bytes

Response time histogram:
  0.021 [1]	|
  0.109 [1050]	|■■■■■■■■■■■■■
  0.197 [2642]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.286 [3140]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.374 [2008]	|■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.462 [701]	|■■■■■■■■■
  0.550 [309]	|■■■■
  0.639 [97]	|■
  0.727 [45]	|■
  0.815 [4]	|
  0.904 [3]	|


Latency distribution:
  10% in 0.1074 secs
  25% in 0.1822 secs
  50% in 0.2115 secs
  75% in 0.2979 secs
  90% in 0.3850 secs
  95% in 0.4278 secs
  99% in 0.5926 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0001 secs, 0.0207 secs, 0.9037 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0056 secs
  req write:	0.0002 secs, 0.0000 secs, 0.0670 secs
  resp wait:	0.2377 secs, 0.0205 secs, 0.9030 secs
  resp read:	0.0003 secs, 0.0000 secs, 0.0673 secs

Status code distribution:
  [200]	10000 responses



```

## OAuth 2.0 Client Management

### Creating OAuth 2.0 Clients

This endpoint uses [BCrypt](#bcrypt) and generates IDs and secrets by reading from  which negatively impacts
performance. Performance will be better if IDs and secrets are set in the request as opposed to generated by ORY Hydra.

```
This test is currently disabled due to issues with /dev/urandom being inaccessible in the CI.
```

### Listing OAuth 2.0 Clients

```

Summary:
  Total:	0.7592 secs
  Slowest:	0.0342 secs
  Fastest:	0.0002 secs
  Average:	0.0074 secs
  Requests/sec:	13171.0952
  
  Total data:	2670000 bytes
  Size/request:	267 bytes

Response time histogram:
  0.000 [1]	|
  0.004 [1750]	|■■■■■■■■■■■■■■■■
  0.007 [2574]	|■■■■■■■■■■■■■■■■■■■■■■■■
  0.010 [4320]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.014 [1032]	|■■■■■■■■■■
  0.017 [218]	|■■
  0.021 [46]	|
  0.024 [15]	|
  0.027 [14]	|
  0.031 [23]	|
  0.034 [7]	|


Latency distribution:
  10% in 0.0023 secs
  25% in 0.0050 secs
  50% in 0.0082 secs
  75% in 0.0098 secs
  90% in 0.0110 secs
  95% in 0.0126 secs
  99% in 0.0177 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0002 secs, 0.0342 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0070 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0183 secs
  resp wait:	0.0052 secs, 0.0001 secs, 0.0168 secs
  resp read:	0.0011 secs, 0.0000 secs, 0.0166 secs

Status code distribution:
  [200]	10000 responses



```

### Fetching a specific OAuth 2.0 Client

```

Summary:
  Total:	0.5740 secs
  Slowest:	0.0347 secs
  Fastest:	0.0002 secs
  Average:	0.0056 secs
  Requests/sec:	17421.5874
  
  Total data:	2650000 bytes
  Size/request:	265 bytes

Response time histogram:
  0.000 [1]	|
  0.004 [3104]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.007 [4450]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.011 [1442]	|■■■■■■■■■■■■■
  0.014 [692]	|■■■■■■
  0.017 [136]	|■
  0.021 [88]	|■
  0.024 [39]	|
  0.028 [16]	|
  0.031 [10]	|
  0.035 [22]	|


Latency distribution:
  10% in 0.0009 secs
  25% in 0.0028 secs
  50% in 0.0050 secs
  75% in 0.0070 secs
  90% in 0.0105 secs
  95% in 0.0124 secs
  99% in 0.0202 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0002 secs, 0.0347 secs
  DNS-lookup:	0.0001 secs, 0.0000 secs, 0.0114 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0116 secs
  resp wait:	0.0018 secs, 0.0001 secs, 0.0239 secs
  resp read:	0.0019 secs, 0.0000 secs, 0.0138 secs

Status code distribution:
  [200]	10000 responses



```