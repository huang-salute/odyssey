<p align="center">
	<img src="documentation/odyssey.png" width="35%" height="35%" /><br>
</p>
<br>

## Odyssey

Advanced multi-threaded PostgreSQL connection pooler and request router.

### Design goals and main features

#### Multi-threaded processing

Odyssey can significantly scale processing performance by
specifying a number of additional worker threads. Each worker thread is
responsible for authentication and proxying client-to-server and server-to-client
requests. All worker threads are sharing global server connection pools.
Multi-threaded design plays important role in `SSL/TLS` performance.

#### Advanced transactional pooling

Odyssey tracks current transaction state and in case of unexpected client
disconnection can emit automatic `Cancel` connection and do `Rollback` of
abandoned transaction, before putting server connection back to
the server pool for reuse. Additionally, last server connection owner client
is remembered to reduce a need for setting up client options on each
client-to-server assignment.

#### Better pooling control

Odyssey allows to define connection pools as a pair of `Database` and `User`.
Each defined pool can have separate authentication, pooling mode and limits settings.

#### Authentication

Odyssey has full-featured `SSL/TLS` support and common authentication methods
like: `md5` and `clear text` both for client and server authentication.
Additionally it allows to block each pool user separately.

#### Logging

Odyssey generates universally unique identifiers `uuid` for client and server connections.
Any log events and client error responces include the id, which then can be used to
uniquely identify client and track actions. Odyssey can save log events into log file and
using system logger.

#### Architecture and internals

Odyssey has sophisticated asynchronous multi-threaded architecture which
is driven by custom made coroutine engine: [machinarium](https://github.yandex-team.ru/pmwkaa/odyssey/tree/master/third_party/machinarium).
Main idea behind coroutine design is to make event-driven asynchronous applications to look and feel
like being written in synchronous-procedural manner instead of using traditional
callback approach.

One of the main goal was to make code base understandable for new developers and
to make an architecture easily extensible for future development.

More information: [Architecture and internals](documentation/internals.md).

#### Build instructions

```sh
git clone git://github.yandex-team.ru/pmwkaa/odyssey.git
cd odyssey
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
make
```

### Configuration reference

##### Service

* [include](documentation/configuration.md#include-string)
* [daemonize](documentation/configuration.md#daemonize-yesno)
* [pid\_file](documentation/configuration.md#pid_file-string)

##### Logging

* [log\_file](documentation/configuration.md#log_file-string)
* [log\_format](documentation/configuration.md#log_format-string)
* [log\_to\_stdout](documentation/configuration.md#log_to_stdout-yesno)
* [log\_syslog](documentation/configuration.md#log_syslog-yesno)
* [log\_syslog\_ident](documentation/configuration.md#log_syslog_ident-string)
* [log\_syslog\_facility](documentation/configuration.md#log_syslog_facility-string)
* [log\_debug](documentation/configuration.md#log_debug-yesno)
* [log\_config](documentation/configuration.md#log_config-yesno)
* [log\_session](documentation/configuration.md#log_session-yesno)
* [log\_query](documentation/configuration.md#log_query-yesno)
* [log\_stats](documentation/configuration.md#log_stats-yesno)
* [stats\_interval](documentation/configuration.md#stats_interval-integer)

##### Performance

* [workers](documentation/configuration.md#workers-integer)
* [resolvers](documentation/configuration.md#resolvers-integer)
* [readahead](documentation/configuration.md#readahead-integer)
* [pipeline](documentation/configuration.md#pipeline-integer)
* [cache](documentation/configuration.md#cache-integer)
* [cache\_chunk](documentation/configuration.md#cache_chunk-integer)
* [cache\_coroutine](documentation/configuration.md#cache_coroutine-integer)
* [nodelay](documentation/configuration.md#nodelay-yesno)
* [keepalive](documentation/configuration.md#keepalive-integer)

##### Global limits

* [client\_max](documentation/configuration.md#client_max-integer)

##### Listen

* [host](documentation/configuration.md#host-string)
* [port](documentation/configuration.md#port-integer)
* [backlog](documentation/configuration.md#backlog-integer)
* [tls](documentation/configuration.md#tls-string)

##### Routing

* [overview](documentation/configuration.md#routing-rules)
* [console](documentation/configuration.md#admin-console)

##### Internals

* [overview](documentation/internals.md)
* [error codes](documentation/internals.md#client-error-codes)
