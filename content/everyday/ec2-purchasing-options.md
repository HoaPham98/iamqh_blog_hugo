+++
date = 2022-05-17T09:00:00Z
tags = ["AWS"]
title = "EC2 Purchasing Options"

+++
# EC2 Purchasing Options

* On-Demand
* Reversed
* Spot
* Dedicated Hosts

| Metrics | On-Demand | Reserved | Spot |
| --- | --- | --- | --- |
| Discount | 0% | 40%-60% | 50%-90% |
| Description | There’s no commitment from you. You pay the most with this option. | 1-year or 3-year commitment from you. You save money from that commitment | Ridiculously inexpensive because there’s no commitment from the AWS side. |
| Use Case | Small or experimental project that do not anticipate the need of config to be used or used in a short time such as: <ul><li>Software testing</li><li>Build a system that runs seasonally</li><li>The system needs to be deployed quickly but can stop at any time <b>(actively)</b></li></ul> | A server for long-term purpose | <ul><li>Limited cost</li><li>Applications can stop at any time without affecting the system: Like jobs that need to be processed once requiring high processing speed, CI/CD, ...</li></ul> |

# EC2 Spot

## EC2 Spot vs On-Demand

| Metrics | Spot Instances | On-Demand Instances |
| --- | --- | --- |
| Launch time | Launches instantly if the Spot Request is active and capacity is available. | Launches instantly when you make a manual launch request and capacity is available. |
| Available capacity | Delivers launch requests until capacity becomes available. | Sends an insufficient capacity error when the request is made and no capacity is available. |
| Hourly price | Varies depending on supply and demand. | Remains the same. |
| Rebalance recommendation | Sends a warning signal when the instance is at high risk for interruption. | Continues to run until you terminate or hibernate the instance. |
| Instance interruption | Interruptible by AWS EC2 when capacity is no longer available, the prices exceed your budgeted max rate, or the demand for Spot instances increases. | Remains uninterrupted until you terminate or hibernate the instance. |

## Spot Fleets

Spot Fleets = set of Spot Instances + (optional) On-Demand Instances

The Spot Fleet will try to meet the target capacity with price constraints

* Define possible launch pools: instance type, OS, AZ
* Can have multiple launch pools, so that the fleet can choose the best and most appropriate launch pool
* When the spot fleet either reaches the budget or the capacity, then it will stops launching instances.

Strategies to allocate Spot Instances:

* lowestPrice: from the pool with the lowest price (cost optimization, short workload)
* deversified: distributed across all pools (great for availability, long workloads)
* capacityOptimized: pool with the optimal capacity for the number of instances

<u>Spot Fleets allow us to automatically request Spot Instances with the lowest price</u>