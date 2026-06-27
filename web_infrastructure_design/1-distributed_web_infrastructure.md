# Task 1 — Distributed web infrastructure

## What the task asks

Design a three-server web infrastructure hosting `www.foobar.com`.

Required additions over Task 0:

- 1 load balancer (HAProxy)
- 2 servers, each containing: 1 web server (Nginx), 1 application server, 1 set
  of application files, 1 database (MySQL)

You must be able to explain, for every added element, why it's there; the load
balancer's distribution algorithm; Active-Active vs Active-Passive; how a
Primary-Replica database cluster works; and the issues of this infrastructure.

## Diagram

![Distributed web infrastructure](diagrams/1-distributed_web_infrastructure.png)

## Explanations

| Added element | Why it's added |
|---------------|----------------|
| Load balancer (HAProxy) | Distributes traffic across both servers and hides a failed one |
| Second server | Redundancy — removes the single-server SPOF |
| Primary-Replica DB | Lets reads scale across replicas while keeping one consistent writer |

| Question | Answer |
|----------|--------|
| Distribution algorithm + how it works | Round Robin (HAProxy default): each new request goes to the next server in turn |
| Active-Active vs Active-Passive | Active-Active: all nodes serve traffic. Active-Passive: one serves, a standby takes over on failure. Here it's **Active-Active** |
| How Primary-Replica works | Writes go to the Primary, which logs changes to its binlog; each Replica replays that log to stay in sync and serves reads |
| Primary vs Replica for the app | The app writes to the Primary and reads from Replicas; the Replica is read-only to the app |

## Issues

| Issue | Why |
|-------|-----|
| SPOF | The load balancer is a single point; the Primary DB is the single writer |
| Security | No firewall, no HTTPS — traffic is open and in cleartext |
| Monitoring | None — failures and load are invisible |

## Answer file

`1-distributed_web_infrastructure` holds the URL of this diagram's screenshot.
