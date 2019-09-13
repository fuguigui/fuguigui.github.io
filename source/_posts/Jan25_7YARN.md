---
title: YARN
date: 2019-01-25
tags: [courses, database, big data]
categories: Big Data
---



JobTracker: 

## Responsibilities
- Resource Management (*RM*)
- Scheduling: like ten tasks per machine, who is in charge of reducing (*RM*)
- Monitoring (*NM*)
- Job lifecycle: if sth goes wrong/failure, it is to restart.
- Fault-tolerance (*AM*): it must ensure that the job still goes well, if sth is wrong.

## Issue
1. Scalability: the typical version 1 mapreduce can have up to **4,000 nodes**, and up tp **40,000** tasks.
2. Bottleneck: only one JobTracker
3. Jack of all trades: (the JobTracker is too busy) for scheduling, monitoring and so on.
4. Utilization: the task slots are set before the job starts. They all have the same size, static, fixed-size.
5. Not fungible: the number of maps and reduces should be decided before the job starts. You may underestimate or overestimate.

# YARN (version 2)
yet another resource negotiator
- Resource Manager: scheduling, application management
- Application Master: monitoring

Different from Version 1:  
- separation between scheduling and monitoring  
- scalability  
- availability  
- multi-tenancy

Number: **10,000** nodes, compared to **4,000** nodes in version 1; **100,000** tasks, compared to **40,000** tasks in version 1.

## Architecture
ResourceManager(Master ) -> NodeManager (Slave), including many **container**.

[the process of architecture](./pic/0701.png)

1. client -> ResoureManager: a job request
2. RM -> one of NMs: the schedules and **allocates an Application Master from containers**.
3. AM -> RM: resources needed. 
4. RM -> AM: allocated resources.
5. AM -> Containers: start tasks, execute monitor

## Resource Manager
responsibilities: cluster utilization, capacity guarantees, fairness, SLAs  
Pure scheduler, **not** monitor tasks, not restart upon failure, AM restarts that.

Communication with ...  
- clients: client service, admin service: only for administrator.   
- NMs: resource Tracker(how many? where?), liveliness, maintain the nodes list, (NM valid/invalid?)  
- AMs (application masters): application master service(registration, container requests), liveliness, maintain the AM lists and waiting list of applications. There are several AMs at the same time.

Authentication: using **Token**

### Scheduling strategies: pluggable scheduler
- FIFO
- capacity, the capacities of queues are given before.
- Fair scheduler: steady fair share, instantaneous fair share.  
How to compute allocations across **multiple resources**? 
- Dominant Resource Fairness: compute the dominant ratio of resource for each applications, and use this ratio as the reference to compute the final resource ratios. 

#### Common resoures
memory (X GB), CPU (Y TB), disk ( W cores, U GHz), network (Z MBps)

## Node Manager
one **per node**.  
Responsibilities: monitoring and report to RM. It packs resources to containers inside it.
## Application Master
One **per application**  
Responsibilities: fault tolerance

AM-> RM: negotiates resources  
AM-> NM: executes and **monitors**: monitors the containers and relaunch the failure ones.
# Abbreviation
YARN, 