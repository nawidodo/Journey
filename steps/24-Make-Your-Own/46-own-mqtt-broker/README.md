# 24-46 · Own MQTT broker — pub/sub, QoS, the IoT backbone (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../45-own-registry-parser`](../45-own-registry-parser/README.md) · **Next:** [`../47-own-js-interpreter`](../47-own-js-interpreter/README.md)

## Objective
Smart homes, cars, factories: MQTT carries IoT. Build a broker: CONNECT/CONSUB/PUBLISH packet parse (the protocol discipline — pairs 24-17 HTTP, 24-34 SMTP), topic tree with wildcards, QoS 0/1/2 (the at-least-once/exactly-once machinery), retained messages, auth. Security tie-in: default-credential brokers, topic enumeration (the "who's listening" attack), and why MQTT TLS (pairs 20-07) matters.

## Tasks
- [ ] Packets: fixed/remaining-length encoding (varint — pairs 24-11), CONNECT/CONNACK, PUBLISH/SUBSCRIBE/UNSUB, PINGREQ
- [ ] Routing: topic tree (the data structure), `+`/`#` wildcards, retained messages, session state + clean sessions
- [ ] QoS: 0/1/2 flows with PUBACK/PUBREC/PUBREL/PUBCOMP — the exactly-once state machine; duplicate suppression
- [ ] Security lab: broker with no auth → enumerate topics (own clients); add username/password + TLS (reuse 20-07); ACL per-topic; the "unauthenticated broker census" writeup (pairs 21-07 hunting) — `labs/`
- [ ] Self-check: a real MQTT client (mosquitto_pub/sub) interoperates with your broker

## Resources
- MQTT 3.1.1/5 spec (the manual); mosquitto source (peer); your 24-17/24-34/20-07 notes

## Exit Criteria
- [ ] Broker routes QoS 0/1/2 with real client interop — `labs/`
- [ ] Topic-enumeration + auth/TLS/ACL lab — `labs/` + `notes/`

## Links
- [MQTT 3.1.1 spec](https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html)
- [mosquitto](https://mosquitto.org/)
