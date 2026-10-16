# Local Tokio patch

Based on the Tokio 1.53.3 release (`a85c3f5baa298a6b554fd3700daf0342289881af`).

The current-thread scheduler reserves space for 8,192 `Notified` entries when
constructed, rather than 64. On a 64-bit target this reserves 64 KiB at startup.
The ready queue is still a growable `VecDeque`; this does not cap runnable tasks,
discard wakeups, or change the multi-thread scheduler.

The reservation is carried forward from the server's Tokio 1.53.2 patch to the
current release. It moves one predictable allocation ahead of connection
allocations; no throughput or memory improvement is claimed for this version
update without a comparable measurement.
