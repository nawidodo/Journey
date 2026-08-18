# 24-52 · Own message queue — pub/sub, delivery guarantees, at-least-once (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../51-own-time-series-db`](../51-own-time-series-db/README.md)

## Objective
Every distributed system is glued by a queue (Kafka, RabbitMQ, SQS). Build one: topics/partitions, producer/consumer protocol (wire format — pairs 24-17/24-46), at-least-once + exactly-once-ishes (offsets, acks, redelivery), persistence (reuse 24-42 AOF ideas), consumer groups. The security tie: queues are the new network perimeter — auth, TLS (20-07), and message-poisoning (the payload that kills the consumer).

## Tasks
- [ ] Broker: topics + partitions, append-only log (pairs 24-42 persistence, 24-26 LSM), retention
- [ ] Protocol: produce/consume/ack frames, offsets + commit; at-least-once with redelivery (the duplicate-handling demo)
- [ ] Consumer groups: partition assignment, rebalancing (the failure mode you must handle)
- [ ] Security lab: unauthenticated broker → anyone consumes/produces; add auth + TLS; poison message (malformed payload) → consumer crashes → quarantine + retry policy (pairs 24-46, 21-02) — `labs/`
- [ ] Self-check: N producers/M consumers, crash a consumer → group rebalances, no message lost (at-least-once holds)

## Resources
- Kafka design docs (the manual); the "log-structured" posts; your 24-42/24-26/24-46 notes

## Exit Criteria
- [ ] Queue with groups + rebalance + at-least-once semantics — `labs/`
- [ ] Auth/TLS + poison-message quarantine lab — `labs/` + `notes/`

## Links
- [Kafka design](https://kafka.apache.org/documentation/#design)
- [RabbitMQ](https://www.rabbitmq.com/)
