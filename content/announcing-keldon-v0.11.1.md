---
title: "Announcing Keldon v0.11.1: The First Public Version"
date: 2026-06-30
author: "Imad El Hassani"
authorURL: "https://www.linkedin.com/in/elhimad"
summary: "Keldon v0.11.1 is the first public version of the Kubernetes operator for Apache Cloudberry — the foundation for making MPP analytics easier to deploy, operate, and recover in cloud-native environments."
---

**Today we are announcing Keldon v0.11.1, the first public version of Keldon.**


## Why Keldon exists

The project started from a simple observation: Apache Cloudberry is powerful, but running and maintaining it by hand is hard. Anyone who has operated Greenplum-family databases knows that the database engine is only one part of the problem. The real challenge is everything around it — bootstrapping the cluster, wiring the nodes together, managing storage, handling SSH trust, configuring mirrors and standby nodes, recovering from failures, and keeping the system healthy over time.

Kubernetes gives us many of the right primitives for this kind of system. StatefulSets provide stable identities. PersistentVolumes provide durable storage. Services provide networking. Secrets provide a place for credentials and keys. Controllers give us reconciliation. But primitives alone are not enough. A distributed analytical database needs more than generic Kubernetes objects — it needs database-aware automation.

That is why Keldon exists.

## How it works

With Keldon, you describe the Apache Cloudberry cluster you want, and the operator works to make the real cluster match that desired state. You define the database image, the number of segments, the storage configuration, whether mirrors are enabled, whether a standby coordinator should exist, and how recovery should behave. Keldon then handles the operational work required to bring that cluster online and keep it aligned with the declared spec.

The goal is not to hide Apache Cloudberry. The goal is to make it operationally manageable on Kubernetes. Cloudberry's architecture still matters. The coordinator is still the SQL entry point. Segments still store and process distributed data. Mirrors still protect segment availability. The standby coordinator still protects the catalog and control plane of the database. Keldon gives those concepts a native place inside Kubernetes instead of forcing operators to manage them through manual scripts and fragile runbooks.

## What ships in v0.11.1

Keldon v0.11.1 introduces the first public version of the core API. It includes custom resources for defining database images, reusable cluster topologies, database configuration, database clusters, backups, and restores. These resources are designed to make Apache Cloudberry feel like a Kubernetes-native system, while still respecting the operational reality of an MPP database.

The most important resource is `DatabaseCluster`. It represents the actual running Cloudberry deployment: coordinator, standby, segments, mirrors, storage, and operational behavior. From that resource, Keldon creates the Kubernetes objects needed to run the cluster, prepares the database topology, establishes trust between pods, initializes the system, and continues watching for changes.

High availability is part of the foundation, not an afterthought. Keldon supports standby coordinators and mirror segments so clusters can be built with resilience from the beginning. When the coordinator fails, a standby can be promoted. When a segment fails and mirroring is enabled, the mirror can take over. These are not optional concerns for distributed databases. In Kubernetes, failure is normal — pods restart, nodes disappear, and workloads move. Keldon is designed around that reality.

## Built for platform teams

This release is also built with platform teams in mind. Running one database cluster manually is possible. Running many clusters manually becomes a problem. Teams need repeatable patterns, standard topologies, consistent configuration, and clear separation between platform defaults and application-level requests.

Keldon's resources such as `DatabaseClusterClass` and `DatabaseConfig` are a step toward that model — platform teams can define approved shapes, and application teams can consume them through Kubernetes APIs.

## Why v0.11.1, not v1.0

We are calling this version `v0.11.1` because it is still early. The first public release is not the same thing as a production maturity claim. APIs may evolve, workflows will be improved, and more real-world testing is needed across different Kubernetes environments. But the foundation is ready to be shared, tested, broken, discussed, and improved in the open.

## What's next

The next phase of Keldon will focus on stronger day-two operations, safer upgrades, better backup and restore workflows, deeper observability, improved recovery behavior, production-ready defaults, and clearer multi-cluster management. The long-term direction is simple: make Apache Cloudberry easier to run, easier to recover, and easier to operate on Kubernetes.

Keldon v0.11.1 is now public. Try it, deploy a cluster, inspect what it creates, read the status, check the logs, and open issues when something does not behave the way you expect. Infrastructure projects become better when they meet real environments, real failures, and real operators.

This is the first public version of Keldon. There is a lot more to build, but now the project is out in the open.