# Deployment and upstream-update contract

This is a personal source fork of `yc-software/qm`. Keep `origin` pointed at
`iamtushar324/qm` and `upstream` at `yc-software/qm`.

The first installation uses Kubernetes on an existing two-node personal cluster.
Its private deployment repository owns hostnames, manifests, image locks, backups,
operator identity, and rollout evidence. Never copy that repository or its secrets
into this public fork. Consult `agent.md` before changing runtime behavior.

## Keep upstream easy to merge

Application source, lockfiles, Dockerfiles, and the bundled chart start unchanged.
Prefer existing environment settings, service contracts, and sandbox interfaces.
Keep deployment overlays outside upstream-owned files. Add a source patch only when
configuration cannot express the requirement, and isolate each patch by purpose.

Fetch upstream frequently and merge it into an integration branch. Preserve upstream
merge ancestry; do not squash source synchronization or rebase published history.
Review conflicts, migration changes, credential handling, image contracts, and
sandbox compatibility before promotion. Immediate integration is the objective;
unattended deployment of untested upstream code is not the release policy.

The deployed runtime and the fork's latest source may differ. Record the exact source
SHA and immutable image digests for each deployed release. A source merge does not
change running containers. For fork runtime patches, build all affected images from
one reviewed commit and record their digests; never label an upstream binary as a
custom build.

## Release gate

1. Preserve a consistent database, workspace, and encryption-key backup.
2. Rehearse schema changes, candidate deployment, and restart/restore checks using
   disposable state before promotion.
3. Deploy the pinned candidate and verify HTTPS, authenticated admin and chat,
   one real model response, its persisted transcript, and a file independently read
   from the agent's computer.
4. Replace application and database Pods; prove history and files survive.
5. Review the source change independently before merging to `main`.
6. Record the release, verification, limitations, and rollback in private operations docs.

An image rollback does not undo database migrations. When schema compatibility is
not proven, drain and stop core and workers before restoring a compatible database
and workspace checkpoint with its encryption key. Account explicitly for changes
since that checkpoint and reconcile external effects before resuming execution.
Restart the compatible release and repeat acceptance checks. Keep encryption keys
alongside the protected backup; encrypted credentials cannot be recovered without them.

## Availability stages

The initial core and sandbox have retained local storage and a single active owner.
This supports Pod replacement on the same host, not survival of that host's loss.
The web and portal services can be replicated across nodes after shared-state and
cross-replica sign-in tests. Keep a single core owner until workspace fencing and
sandbox recovery are implemented and tested.

Next: a native Kubernetes sandbox adapter using per-scope workloads and persistent
volumes, then separately deployable workers, ownership fencing, and partition tests.
Avoid assuming a generic Kubernetes Service understands session ownership. Shared
PostgreSQL queues/signals carry commands to the worker; sandbox ownership determines
where file operations execute.

Two nodes do not provide a three-member control-plane quorum. Full host-failure
availability also needs a resilient control plane, database replication with tested
promotion, recoverable workspace storage, a redundant ingress address, and spare
capacity. Never advertise high availability based only on replica count.
