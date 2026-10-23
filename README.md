# Awesome-Messaging-Libraries

## Top Messaging Libraries Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on AMQP Protocol Clients, Multi-Transport Abstraction & Task Queue Integration*  
**Last updated: March 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **Messaging Libraries**. These tools provide the client-side layer between your application and a message broker — handling AMQP handshakes, connection recovery, channel management, and serialization — so your code can publish and consume messages without implementing the protocol from scratch.

**Examples** include Pika, Kombu, Celery, and the managed broker services (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active project for self-hosting, transport abstraction, and transparent connection handling — ideal for Python developers who need reliable messaging without locking into a single broker vendor.

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **CloudAMQP** — **Managed RabbitMQ hosting.** The largest hosting provider of RabbitMQ and a proud sponsor of Celery . Provides managed RabbitMQ clusters with monitoring, backups, and support. **Best for teams wanting RabbitMQ's AMQP semantics** without operating the broker themselves.

- **Amazon SQS** — AWS's fully managed message queuing service. **Native support in Celery as a message transport**  and in Kombu as a virtual transport . **Best for AWS-native teams** wanting managed queuing without broker operations.

- **Redis Cloud** — Managed Redis with pub/sub and list-based queue capabilities. **Redis is a feature-complete transport for Celery**  and a built-in virtual transport in Kombu . **Best for teams already using Redis** for caching who want to reuse it for lightweight queuing.

- **Google Cloud Pub/Sub** — GCP's managed messaging service. Accessible from Python via Google's client libraries rather than Pika or Kombu.

## Open-Source GitHub Projects

- **[Pika](https://github.com/pika/pika)** — **The pure-Python AMQP 0-9-1 client library for RabbitMQ.** MPL/GPL dual licensed, **3,700+ GitHub stars** . **Stays fairly independent of the underlying network support library** — it doesn't require threads but doesn't forbid them, and works with direct sockets, asyncore, Twisted, or plain `select()` . **Multiple connection adapters**: `SelectConnection` (fast asynchronous, recommended), `BlockingConnection` (synchronous for simple uses), `TornadoConnection` (Tornado IOLoop), and `AsyncoreConnection` (legacy) . **Fully asynchronous at its core** — async AMQP commands take a callback for completion notification, while synchronous commands buffer other calls on the same channel . **Best for developers wanting direct RabbitMQ/AMQP control** without Celery's task abstraction layer.

- **[Kombu](https://github.com/celery/kombu)** — **The messaging library for Python that abstracts over multiple transports.** BSD-3-Clause licensed [citation:5]. **Provides an idiomatic high-level interface for AMQP** while also offering proven solutions to common messaging problems . **Pluggable transports**: native AMQP via py-amqp or qpid-python, plus virtual transports for Redis, Amazon SQS, ZooKeeper, MongoDB, SoftLayer MQ, Pyro, and in-memory for unit testing . **Supports automatic encoding, serialization, and compression of message payloads** with consistent exception handling across transports . **The transport layer underneath Celery** — Celery is usually used with Kombu as its messaging backbone . **The critical detail**: **queue name separators must match across workers** — using different `sep` values in `broker_transport_options` between workers causes `ValueError('not enough values to unpack (expected 3, got 1)')` during sync [citation:3]. **Best for teams wanting broker flexibility** — write against Kombu's abstraction and switch between RabbitMQ, Redis, and SQS without rewriting application code.

- **[Celery](https://github.com/celery/celery)** — **The distributed task queue built on Kombu.** BSD-3-Clause licensed. **The reference implementation for Python background jobs** — a single Celery process can process **millions of tasks a minute** with sub-millisecond round-trip latency using RabbitMQ and optimized settings . **Message transports**: RabbitMQ, Redis, and Amazon SQS are the supported options . **Concurrency models**: Prefork, Eventlet, gevent, and solo (single-threaded) . **Result stores** include AMQP, Redis, memcached, SQLAlchemy, Django ORM, Apache Cassandra, IronCache, and Elasticsearch . **Serialization**: pickle, json, yaml, msgpack with zlib/bzip2 compression and cryptographic signing . **Framework integration** with Django, Flask, FastAPI (no integration package needed), Pyramid, Pylons, and Tornado . **Celery v5.6.0** adds support for **quorum queues** in RabbitMQ via `task_create_missing_queue_type` configuration [citation:12]. **Best for teams needing a complete task queue** with scheduling, retries, and result backends — not just raw message passing.

### Additional Strong Open-Source Options

- **kombu-asyncio** — **Exploratory asyncio-native rewrite of Kombu** (pre-release, version 6.0.0a2) [citation:5]. **All operations are async/await, no sync API** — a completely breaking change from the original Kombu [citation:5]. Transports: Redis, AMQP (via aio-pika), Memory, Filesystem [citation:5]. **Not production-ready** and not affiliated with the Celery project — for experimentation only [citation:5].
- **aio-pika** — Asyncio AMQP client for Python. The AMQP transport used by kombu-asyncio [citation:5]. Best for async-first RabbitMQ applications.

**Frameworks for building custom systems**: Combine **Pika** for direct RabbitMQ control with `BlockingConnection` for simple use cases or `SelectConnection` for async performance, **Kombu** for broker-agnostic messaging that switches between RabbitMQ, Redis, and SQS, and **Celery** for full task queue semantics with retries and result backends — to build a messaging stack tailored to your need for raw protocol control versus high-level task orchestration.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- Messaging libraries differ significantly in abstraction level, transport support, and operational requirements. **Pika is the low-level AMQP client** — you handle connection recovery and channel management yourself, but you get direct control . **Kombu abstracts over multiple brokers** but **queue name separators must match across all workers** — mismatched `sep` values in `broker_transport_options` cause `ValueError` during sync [citation:3]. **Celery is a task queue, not a messaging library** — it uses Kombu underneath and adds scheduling, retries, and result backends, but brings its own operational complexity . **Celery does not officially support Windows** — it may work, but issues related to that platform are not supported . **Redis and RabbitMQ are feature-complete Celery transports**; other transports are experimental .
- Self-hosted open-source solutions require proper broker configuration (RabbitMQ or Redis), connection retry policies, and awareness that message durability depends on broker settings, not just the client library.

---

**Made for Python developers building reliable messaging and task queue systems.**
Let's make messaging libraries more open, transport-flexible, and broker-agnostic.
