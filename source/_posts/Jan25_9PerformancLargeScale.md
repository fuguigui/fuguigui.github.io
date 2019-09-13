---
title: Performance at large scale
date: 2019-01-25
tags: [courses, database, big data]
categories: Big Data
---



# Measurements

Prefixes: mili(m, 0.001,3), micro(\mu, 0.000 001, 6), nano(n, 9), pico(p, 12), femto(f, 15), atto(a, 18), zepto(z, 21), yocto(y, 24)

## Speedup
- Amdahl's law: SpeedUp=$\frac{1}{1-p+p/s$. Constant problem size.  
- Gustafson's law: SpeedUp=$1-p+sp$. Constant computing power.

# Tuning
- scale out  
- scale up: memory, disk, CPU, network ... ** an easy but last resort** 
- code:
	- look for large **loops**
	- avoid exception catching
	- avoid polymorphism
	- avoid virtual function
	- go low level if needed
- size of chunks: make the size smaller for liquidity, but not too much for latency issues.
- storage format: syntax VS binary format
- network usage: 
	- keep **shuffling** to a minimum
	- push down **projection and selection** as close as possible to the source.
- architecture

# Tail latency
## Causes
sharing resources, queues, garbage collection, energy management...
## Improvements
- keep the low-level queues short
- break splits and queries further down
- manage background activities
- latency-induced probation: we shut down a machine that is too slow.
- completely drop the last 0.01% that takes too long...
- other measures
### Hedge requests
basic idea: **duplicate**

task duplicates, first done wins.
#### Deferred hedge request
when 95% percentile reached, launch duplicate tasks.

### Tied request
basic idea **cancel request**. A task is duplicated and waiting in several queues. Cancel other requests when one of them starts.
