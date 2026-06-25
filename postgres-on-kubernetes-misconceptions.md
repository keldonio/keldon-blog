---
title: "Three misconceptions about Postgres on Kubernetes"
date: 2026-03-27
author: "Nora Vasquez"
summary: "Running stateful workloads on Kubernetes still carries folklore from a decade ago. Let's retire some of it."
---

Every time we talk to teams adopting Keldon, a handful of the same concerns come up. Most of them trace back to advice that was accurate in 2016 but has not aged well. Here are three that we think deserve retiring.

## "Kubernetes networking adds unacceptable latency"

In practice, CNI plugins like Cilium and Calico add under a millisecond of overhead on modern hardware — often well under. For OLTP workloads where a single query touches the database for two or three milliseconds, that overhead is noise. The path-length math that used to matter simply does not anymore.

## "Persistent volumes are fragile"

Early PV implementations were genuinely rough. The CSI ecosystem has matured: volumes are snapshottable, resizable, and consistently attached across the same cluster reliably. The operator's job is to pair this with PostgreSQL-native replication, so even if a PV does misbehave, a replica takes over.

## "You need StatefulSets to handle state"

You don't. Keldon schedules instances as plain `Pods` with dedicated PVCs. StatefulSets are convenient when the ordering guarantees they give you match what you need, but they also make many operational tasks harder — rolling restarts, reordering promotions, surgical pod replacements. Treating instances as individuals gives us finer control and simpler recovery paths.

None of this is to say Kubernetes has become trivial. It hasn't. But the shape of the problems has changed, and the operators have caught up.
