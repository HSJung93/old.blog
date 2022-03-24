---
title: "What is the CAP Theorem"
date: 2022-03-21T09:46:00+09:00
categories:
  - Memo
tags:
  - Consistency
  - Availability
  - Partition Tolerance
# header:
#   teaser: /assets/images/code.jpg
---

The CAP theorem(Brewer's theorem) states that any distributed data store can only provide 2 of the following 3 guarantees: Consistency, Availability, Partition tolerance. Consistency means every read receives the most recent write or an error. Availability means every request receives a (non-error) response, without the guarantee that it contains the most recent write. Partition tolerance means the system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes.

If there is a network partition, one has to choose between consistency and availability. When a network partition failure happens, it must be decided whether to cancel the operation and thus decrease the availability but ensure consistency or to proceed with the operation and thus provide availability but risk inconsistency.

When you choose to earn consistency and availability in your databases, RDBMS is your best choice. NoSQL databases such as MongoDB, Redis are in CP Category, and Dynamo, Cassandra are in AP Category.