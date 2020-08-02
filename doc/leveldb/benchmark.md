---
title: LevelDB Benchmarks
---

LevelDB Benchmarks
==================

Google, July 2011

------------------------------------------------------------------------

In order to test LevelDB\'s performance, we benchmark it against other
well-established database implementations. We compare LevelDB (revision
39) against [SQLite3](http://www.sqlite.org/) (version 3.7.6.3) and
[Kyoto Cabinet\'s](http://fallabs.com/kyotocabinet/spex.html) (version
1.2.67) TreeDB (a B+Tree based key-value store). We would like to
acknowledge Scott Hess and Mikio Hirabayashi for their suggestions and
contributions to the SQLite3 and Kyoto Cabinet benchmarks, respectively.

Benchmarks were all performed on a six-core Intel(R) Xeon(R) CPU X5650 @
2.67GHz, with 12288 KB of total L3 cache and 12 GB of DDR3 RAM at 1333
MHz. (Note that LevelDB uses at most two CPUs since the benchmarks are
single threaded: one to run the benchmark, and one for background
compactions.) We ran the benchmarks on two machines (with identical
processors), one with an Ext3 file system and one with an Ext4 file
system. The machine with the Ext3 file system has a SATA Hitachi
HDS721050CLA362 hard drive. The machine with the Ext4 file system has a
SATA Samsung HD502HJ hard drive. Both hard drives spin at 7200 RPM and
have hard drive write-caching enabled (using \`hdparm -W 1
\[device\]\`). The numbers reported below are the median of three
measurements.

#### Benchmark Source Code

We wrote benchmark tools for SQLite and Kyoto TreeDB based on LevelDB\'s
[db_bench]{.code}. The code for each of the benchmarks resides here:

-   **LevelDB:**
    [db/db_bench.cc](http://code.google.com/p/leveldb/source/browse/trunk/db/db_bench.cc).
-   **SQLite:**
    [doc/bench/db_bench_sqlite3.cc](http://code.google.com/p/leveldb/source/browse/#svn%2Ftrunk%2Fdoc%2Fbench%2Fdb_bench_sqlite3.cc).
-   **Kyoto TreeDB:**
    [doc/bench/db_bench_tree_db.cc](http://code.google.com/p/leveldb/source/browse/#svn%2Ftrunk%2Fdoc%2Fbench%2Fdb_bench_tree_db.cc).

#### Custom Build Specifications

-   LevelDB: LevelDB was compiled with the
    [tcmalloc](http://code.google.com/p/google-perftools) library and
    the [Snappy](http://code.google.com/p/snappy/) compression library
    (revision 33). Assertions were disabled.
-   TreeDB: TreeDB was compiled using the
    [LZO](http://www.oberhumer.com/opensource/lzo/) compression library
    (version 2.03). Furthermore, we enabled the TSMALL and TLINEAR
    options when opening the database in order to reduce the footprint
    of each record.
-   SQLite: We tuned SQLite\'s performance, by setting its locking mode
    to exclusive. We also enabled SQLite\'s [write-ahead
    logging](http://www.sqlite.org/draft/wal.html).

1. Baseline Performance
-----------------------

This section gives the baseline performance of all the databases.
Following sections show how performance changes as various parameters
are varied. For the baseline:

-   Each database is allowed 4 MB of cache memory.
-   Databases are opened in *asynchronous* write mode. (LevelDB\'s sync
    option, TreeDB\'s OAUTOSYNC option, and SQLite3\'s synchronous
    options are all turned off). I.e., every write is pushed to the
    operating system, but the benchmark does not wait for the write to
    reach the disk.
-   Keys are 16 bytes each.
-   Value are 100 bytes each (with enough redundancy so that a simple
    compressor shrinks them to 50% of their original size).
-   Sequential reads/writes traverse the key space in increasing order.
-   Random reads/writes traverse the key space in random order.

### A. Sequential Reads

+-----------------------+-----------------------+-----------------------+
| LevelDB               | 4,030,000 ops/sec     | ::: {.bldb            |
|                       |                       |  style="width:350px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+
| Kyoto TreeDB          | 1,010,000 ops/sec     | ::: {.bkc             |
|                       |                       | t style="width:95px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+
| SQLite3               | 383,000 ops/sec       | ::: {.bsq             |
|                       |                       | l style="width:33px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+

### B. Random Reads

+-----------------------+-----------------------+-----------------------+
| LevelDB               | 129,000 ops/sec       | ::: {.bldb            |
|                       |                       |  style="width:298px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+
| Kyoto TreeDB          | 151,000 ops/sec       | ::: {.bkct            |
|                       |                       |  style="width:350px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+
| SQLite3               | 134,000 ops/sec       | ::: {.bsql            |
|                       |                       |  style="width:310px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+

### C. Sequential Writes

+-----------------------+-----------------------+-----------------------+
| LevelDB               | 779,000 ops/sec       | ::: {.bldb            |
|                       |                       |  style="width:350px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+
| Kyoto TreeDB          | 342,000 ops/sec       | ::: {.bkct            |
|                       |                       |  style="width:154px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+
| SQLite3               | 48,600 ops/sec        | ::: {.bsq             |
|                       |                       | l style="width:22px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+

### D. Random Writes

+-----------------------+-----------------------+-----------------------+
| LevelDB               | 164,000 ops/sec       | ::: {.bldb            |
|                       |                       |  style="width:350px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+
| Kyoto TreeDB          | 88,500 ops/sec        | ::: {.bkct            |
|                       |                       |  style="width:188px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+
| SQLite3               | 9,860 ops/sec         | ::: {.bsq             |
|                       |                       | l style="width:21px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+

LevelDB outperforms both SQLite3 and TreeDB in sequential and random
write operations and sequential read operations. Kyoto Cabinet has the
fastest random read operations.

2. Write Performance under Different Configurations
---------------------------------------------------

### A. Large Values

For this benchmark, we start with an empty database, and write 100,000
byte values (\~50% compressible). To keep the benchmark running time
reasonable, we stop after writing 1000 values.

#### Sequential Writes

+-----------------------+-----------------------+-----------------------+
| LevelDB               | 1,100 ops/sec         | ::: {.bldb            |
|                       |                       |  style="width:234px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+
| Kyoto TreeDB          | 1,000 ops/sec         | ::: {.bkct            |
|                       |                       |  style="width:224px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+
| SQLite3               | 1,600 ops/sec         | ::: {.bsql            |
|                       |                       |  style="width:350px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+

#### Random Writes

+-----------------------+-----------------------+-----------------------+
| LevelDB               | 480 ops/sec           | ::: {.bldb            |
|                       |                       |  style="width:105px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+
| Kyoto TreeDB          | 1,100 ops/sec         | ::: {.bkct            |
|                       |                       |  style="width:240px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+
| SQLite3               | 1,600 ops/sec         | ::: {.bsql            |
|                       |                       |  style="width:350px"} |
|                       |                       |                       |
|                       |                       | :::                   |
+-----------------------+-----------------------+-----------------------+

LevelDB doesn\'t perform as well with large values of 100,000 bytes
each. This is because LevelDB writes keys and values at least twice:
first time to the transaction log, and second time (during a compaction)
to a sorted file. With larger values, LevelDB\'s per-operation
efficiency is swamped by the cost of extra copies of large values.

### B. Batch Writes

A batch write is a set of writes that are applied atomically to the
underlying database. A single batch of N writes may be significantly
faster than N individual writes. The following benchmark writes one
thousand batches where each batch contains one thousand 100-byte values.
TreeDB does not support batch writes and is omitted from this benchmark.

#### Sequential Writes

+-----------------+-----------------+-----------------+-----------------+
| LevelDB         | 840,000         | :               | (1.08x          |
|                 | entries/sec     | :: {.bldb style | baseline)       |
|                 |                 | ="width:350px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| SQLite3         | 124,000         | ::: {.bsql styl | (2.55x          |
|                 | entries/sec     | e="width:52px"} | baseline)       |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+

#### Random Writes

+-----------------+-----------------+-----------------+-----------------+
| LevelDB         | 221,000         | :               | (1.35x          |
|                 | entries/sec     | :: {.bldb style | baseline)       |
|                 |                 | ="width:350px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| SQLite3         | 22,000          | ::: {.bsql styl | (2.23x          |
|                 | entries/sec     | e="width:34px"} | baseline)       |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+

Because of the way LevelDB persistent storage is organized, batches of
random writes are not much slower (only a factor of 4x) than batches of
sequential writes.

### C. Synchronous Writes

In the following benchmark, we enable the synchronous writing modes of
all of the databases. Since this change significantly slows down the
benchmark, we stop after 10,000 writes. For synchronous write tests,
we\'ve disabled hard drive write-caching (using \`hdparm -W 0
\[device\]\`).

-   For LevelDB, we set WriteOptions.sync = true.
-   In TreeDB, we enabled TreeDB\'s OAUTOSYNC option.
-   For SQLite3, we set \"PRAGMA synchronous = FULL\".

#### Sequential Writes

+-----------------+-----------------+-----------------+-----------------+
| LevelDB         | 100 ops/sec     | :               | (0.003x         |
|                 |                 | :: {.bldb style | baseline)       |
|                 |                 | ="width:350px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| Kyoto TreeDB    | 7 ops/sec       | ::: {.bkct styl | (0.0004x        |
|                 |                 | e="width:27px"} | baseline)       |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| SQLite3         | 88 ops/sec      | :               | (0.002x         |
|                 |                 | :: {.bsql style | baseline)       |
|                 |                 | ="width:315px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+

#### Random Writes

+-----------------+-----------------+-----------------+-----------------+
| LevelDB         | 100 ops/sec     | :               | (0.015x         |
|                 |                 | :: {.bldb style | baseline)       |
|                 |                 | ="width:350px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| Kyoto TreeDB    | 8 ops/sec       | ::: {.bkct styl | (0.001x         |
|                 |                 | e="width:29px"} | baseline)       |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| SQLite3         | 88 ops/sec      | :               | (0.009x         |
|                 |                 | :: {.bsql style | baseline)       |
|                 |                 | ="width:314px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+

Also see the `ext4` performance numbers below since synchronous writes
behave significantly differently on `ext3` and `ext4`.

### D. Turning Compression Off

In the baseline measurements, LevelDB and TreeDB were using light-weight
compression ([Snappy](http://code.google.com/p/snappy/) for LevelDB, and
[LZO](http://www.oberhumer.com/opensource/lzo/) for TreeDB). SQLite3, by
default does not use compression. The experiments below show what
happens when compression is disabled in all of the databases (the
SQLite3 numbers are just a copy of its baseline measurements):

#### Sequential Writes

+-----------------+-----------------+-----------------+-----------------+
| LevelDB         | 594,000 ops/sec | :               | (0.76x          |
|                 |                 | :: {.bldb style | baseline)       |
|                 |                 | ="width:350px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| Kyoto TreeDB    | 485,000 ops/sec | :               | (1.42x          |
|                 |                 | :: {.bkct style | baseline)       |
|                 |                 | ="width:239px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| SQLite3         | 48,600 ops/sec  | ::: {.bsql styl | (1.00x          |
|                 |                 | e="width:29px"} | baseline)       |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+

#### Random Writes

+-----------------+-----------------+-----------------+-----------------+
| LevelDB         | 135,000 ops/sec | :               | (0.82x          |
|                 |                 | :: {.bldb style | baseline)       |
|                 |                 | ="width:296px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| Kyoto TreeDB    | 159,000 ops/sec | :               | (1.80x          |
|                 |                 | :: {.bkct style | baseline)       |
|                 |                 | ="width:350px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| SQLite3         | 9,860 ops/sec   | ::: {.bsql styl | (1.00x          |
|                 |                 | e="width:22px"} | baseline)       |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+

LevelDB\'s write performance is better with compression than without
since compression decreases the amount of data that has to be written to
disk. Therefore LevelDB users can leave compression enabled in most
scenarios without having worry about a tradeoff between space usage and
performance. TreeDB\'s performance on the other hand is better without
compression than with compression. Presumably this is because TreeDB\'s
compression library (LZO) is more expensive than LevelDB\'s compression
library (Snappy).

### E. Using More Memory

We increased the overall cache size for each database to 128 MB. For
LevelDB, we partitioned 128 MB into a 120 MB write buffer and 8 MB of
cache (up from 2 MB of write buffer and 2 MB of cache). For SQLite3, we
kept the page size at 1024 bytes, but increased the number of pages to
131,072 (up from 4096). For TreeDB, we also kept the page size at 1024
bytes, but increased the cache size to 128 MB (up from 4 MB).

#### Sequential Writes

+-----------------+-----------------+-----------------+-----------------+
| LevelDB         | 812,000 ops/sec | :               | (1.04x          |
|                 |                 | :: {.bldb style | baseline)       |
|                 |                 | ="width:350px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| Kyoto TreeDB    | 321,000 ops/sec | :               | (0.94x          |
|                 |                 | :: {.bkct style | baseline)       |
|                 |                 | ="width:138px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| SQLite3         | 48,500 ops/sec  | ::: {.bsql styl | (1.00x          |
|                 |                 | e="width:21px"} | baseline)       |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+

#### Random Writes

+-----------------+-----------------+-----------------+-----------------+
| LevelDB         | 355,000 ops/sec | :               | (2.16x          |
|                 |                 | :: {.bldb style | baseline)       |
|                 |                 | ="width:350px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| Kyoto TreeDB    | 284,000 ops/sec | :               | (3.21x          |
|                 |                 | :: {.bkct style | baseline)       |
|                 |                 | ="width:280px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| SQLite3         | 9,670 ops/sec   | ::: {.bsql styl | (0.98x          |
|                 |                 | e="width:10px"} | baseline)       |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+

SQLite\'s performance does not change substantially when compared to the
baseline, but the random write performance for both LevelDB and TreeDB
increases significantly. LevelDB\'s performance improves because a
larger write buffer reduces the need to merge sorted files (since it
creates a smaller number of larger sorted files). TreeDB\'s performance
goes up because the entire database is available in memory for fast
in-place updates.

3. Read Performance under Different Configurations
--------------------------------------------------

### A. Larger Caches

We increased the overall memory usage to 128 MB for each database. For
LevelDB, we allocated 8 MB to LevelDB\'s write buffer and 120 MB to
LevelDB\'s cache. The other databases don\'t differentiate between a
write buffer and a cache, so we simply set their cache size to 128 MB.

#### Sequential Reads

+-----------------+-----------------+-----------------+-----------------+
| LevelDB         | 5,210,000       | :               | (1.29x          |
|                 | ops/sec         | :: {.bldb style | baseline)       |
|                 |                 | ="width:350px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| Kyoto TreeDB    | 1,070,000       | ::: {.bkct styl | (1.06x          |
|                 | ops/sec         | e="width:72px"} | baseline)       |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| SQLite3         | 609,000 ops/sec | ::: {.bsql styl | (1.59x          |
|                 |                 | e="width:41px"} | baseline)       |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+

#### Random Reads

+-----------------+-----------------+-----------------+-----------------+
| LevelDB         | 190,000 ops/sec | :               | (1.47x          |
|                 |                 | :: {.bldb style | baseline)       |
|                 |                 | ="width:144px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| Kyoto TreeDB    | 463,000 ops/sec | :               | (3.07x          |
|                 |                 | :: {.bkct style | baseline)       |
|                 |                 | ="width:350px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| SQLite3         | 186,000 ops/sec | :               | (1.39x          |
|                 |                 | :: {.bsql style | baseline)       |
|                 |                 | ="width:141px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+

As expected, the read performance of all of the databases increases when
the caches are enlarged. In particular, TreeDB seems to make very
effective use of a cache that is large enough to hold the entire
database.

### B. No Compression Reads

For this benchmark, we populated a database with 1 million entries
consisting of 16 byte keys and 100 byte values. We compiled LevelDB and
Kyoto Cabinet without compression support, so results that are read out
from the database are already uncompressed. We\'ve listed the SQLite3
baseline read performance as a point of comparison.

#### Sequential Reads

+-----------------+-----------------+-----------------+-----------------+
| LevelDB         | 4,880,000       | :               | (1.21x          |
|                 | ops/sec         | :: {.bldb style | baseline)       |
|                 |                 | ="width:350px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| Kyoto TreeDB    | 1,230,000       | ::: {.bkct styl | (3.60x          |
|                 | ops/sec         | e="width:88px"} | baseline)       |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| SQLite3         | 383,000 ops/sec | ::: {.bsql styl | (1.00x          |
|                 |                 | e="width:27px"} | baseline)       |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+

#### Random Reads

+-----------------+-----------------+-----------------+-----------------+
| LevelDB         | 149,000 ops/sec | :               | (1.16x          |
|                 |                 | :: {.bldb style | baseline)       |
|                 |                 | ="width:300px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| Kyoto TreeDB    | 175,000 ops/sec | :               | (1.16x          |
|                 |                 | :: {.bkct style | baseline)       |
|                 |                 | ="width:350px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+
| SQLite3         | 134,000 ops/sec | :               | (1.00x          |
|                 |                 | :: {.bsql style | baseline)       |
|                 |                 | ="width:268px"} |                 |
|                 |                 |                 |                 |
|                 |                 | :::             |                 |
+-----------------+-----------------+-----------------+-----------------+

Performance of both LevelDB and TreeDB improves a small amount when
compression is disabled. Note however that under different workloads,
performance may very well be better with compression if it allows more
of the working set to fit in memory.

Note about Ext4 Filesystems
---------------------------

The preceding numbers are for an ext3 file system. Synchronous writes
are much slower under [ext4](http://en.wikipedia.org/wiki/Ext4) (LevelDB
drops to \~31 writes / second and TreeDB drops to \~5 writes / second;
SQLite3\'s synchronous writes do not noticeably drop) due to ext4\'s
different handling of [fsync]{.code} / [msync]{.code} calls. Even
LevelDB\'s asynchronous write performance drops somewhat since it
spreads its storage across multiple files and issues [fsync]{.code}
calls when switching to a new file.

Acknowledgements
----------------

Jeff Dean and Sanjay Ghemawat wrote LevelDB. Kevin Tseng wrote and
compiled these benchmarks. Mikio Hirabayashi, Scott Hess, and Gabor
Cselle provided help and advice.
