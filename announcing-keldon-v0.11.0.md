---
title: "Announcing Keldon v0.11.0"
date: 2026-04-18
author: "The Keldon Team"
summary: "Version v0.11.0 brings faster failover, tablespace support, and a rewritten backup engine."
---

Today we're releasing Keldon v0.11.0. This release is the result of six months of work focused on two themes: making failovers faster, and making backup storage more efficient.

## Faster failovers

Previously, promoting a replica to primary took between 20 and 45 seconds depending on cluster size. In 1.24 we reworked the promotion pipeline to run lease acquisition and WAL replay in parallel, which brings median failover time down to under 8 seconds on clusters we tested.

## Tablespaces

Tablespace support has been one of the most-requested features on our roadmap. You can now declare tablespaces directly in the `Cluster` spec, backed by separate `PersistentVolumeClaims`. This makes it practical to separate hot indexes onto faster storage without operating outside the operator model.

## New backup engine

The backup pipeline has been rewritten around streaming. Instead of buffering the entire base backup to local disk before uploading, it now streams straight to object storage. The net effect is roughly 40% less disk usage during backups and a noticeable reduction in completion time for larger clusters.

## Upgrading

The upgrade is seamless from 1.23. Helm users can run the usual `helm upgrade`; manifest users can apply the new release YAML. Existing clusters will continue running without interruption.

Thanks to everyone who contributed, tested, and reported issues along the way.
