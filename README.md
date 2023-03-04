# mysql-trawl

This is intended as an alternative to mysqlslap while providing a 

This program uses [`trawler`](https://github.com/jonhoo/trawler) to
generate a workload that is similar to that of the
[lobste.rs](https://lobste.rs) news aggregator. It is the same benchmark
that was used in §8.1 in the [OSDI'18 Noria
paper](https://jon.tsp.io/papers/osdi18-noria.pdf).

This program closely follows 
[`lobsters`](https://github.com/mit-pdos/noria/tree/master/applications/lobsters), but with less of a focus on
accurately reflecting the [lobsters.rs](https://lobste.rs) workflow and more on slapping MySQL.


nchmark issues SQL queries from the [Lobsters code
base](https://github.com/lobsters/lobsters) according to the production
traffic patterns reported [here](https://lobste.rs/s/cqnzl5/). It can
run in one of three modes:

Compile the benchmark with

```console
$ cargo b --release --bin mysql-trawl
```

Then start the backend you want to slap, you must first decide what
target load (`reqscale`) you want to test. The `reqscale` is a scaling
factor relative to that seen by the production Lobsters deployment. See
the paper linked above for good values to try.

Then, run, for example, with a reqscale run:

```console
$ target/release/mysql-trawl \
   --reqscale <target> \
   --warmup 20 \
   --runtime 30 \
   --issuers 24 \
   "mysql://lobsters:soup@127.0.0.1/lobsters"
```