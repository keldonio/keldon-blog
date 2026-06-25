---
title: "this is a test article to test layout"
date: 2026-06-26
author: "Imad keldon"
summary: "its not about The operator pattern isn't a workaround for Kubernetes' gaps — it's how stateful systems should have been managed all along."
---

When the operator pattern was first proposed, a common reaction was that it was a clever compensation for Kubernetes not handling state well. That framing gets it backwards.

Operators aren't filling a gap. They're encoding operational knowledge — the kind that used to live in runbooks and ops engineers' heads — in a form the platform can act on automatically. That's a much older idea than Kubernetes, and it's one that stateful systems have always needed more than stateless ones.

## The runbook problem

For years, running PostgreSQL in production meant maintaining a library of runbooks: how to add a replica, how to perform a controlled failover, how to rotate credentials, how to upgrade a minor version without breaking replication. These documents drifted out of sync with reality the moment they were written.

An operator captures those procedures as executable code. When the rules change, you ship a new operator version and every cluster in your fleet picks up the new behavior. That is a meaningfully different posture than "hope everyone reads the updated page."

## What Keldon encodes

Inside the Keldon controller are decisions that used to require a human:

- When to promote which replica
- How to reconfigure streaming replication after a topology change
- When to archive WAL and when to take a base backup
- How to stage a PostgreSQL version upgrade safely

None of these are novel to PostgreSQL operators. What's novel is that they're now part of the platform, not tribal knowledge.
