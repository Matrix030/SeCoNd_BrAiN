---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["Selecting a Scheduling Algorithm"]
---

# Selecting an Algorithm

1) The first problem is defining the criteria to be used in selecting an algorithm. As we saw in [[> Scheduling Criteria|Section 5.2]], criteria are often defined in terms of CPU utilization, response time, or throughput. To select an algorithm, we must first define the relative importance of these elements. Our criteria may include several measures, such as:
	- Maximizing CPU utilization under the constraint that the maximum response time is 1 second
	- Maximizing throughput such that turnaround time is (on average) linearly proportional to total execution time
2) Once the selection criteria have been defined, we want to evaluate the algorithms under consideration. We next describe the various evaluation methods we can use.

## Related

- [[> Algorithm Evaluation]] — back to the sub-topic MOC
- [[Deterministic Modeling]] — the first evaluation method
- [[> Scheduling Criteria]] — where the selection criteria come from
