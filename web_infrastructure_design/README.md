# Web infrastructure design

Whiteboarding project. Four diagrams of the same website (`www.foobar.com`),
each adding redundancy, security, observability, or scale to the previous one.
Each task has its own markdown file (requirements + diagram + explanations) and
a Holberton answer file holding the link to the diagram screenshot.

## What this covers

| Component | Role |
|-----------|------|
| Domain name | Human-friendly name mapped to a server IP via DNS |
| DNS `A` record | Maps a name directly to an IPv4 address (`www` → `8.8.8.8`) |
| Server | A machine/program serving clients over the network — a role, not a box |
| Web server (Nginx) | Handles HTTP, serves static files, reverse-proxies dynamic requests |
| Application server | Runs the codebase to generate dynamic pages |
| Application files | The codebase the application server executes |
| Database (MySQL) | Stores persistent data, queried over SQL |
| Load balancer (HAProxy) | Distributes requests across servers, health-checks backends |
| Primary-Replica | One writable DB (Primary) + read-only copies (Replicas) |
| Firewall | Allows/denies traffic by rule (IP, port, direction) |
| SSL certificate / HTTPS | Encrypts traffic; authenticates the server; protects integrity |
| Monitoring client | Agent collecting metrics/logs and pushing them to a service |

## What each task adds

| Task | What it adds over the previous one |
|------|------------------------------------|
| [0 — Simple web stack](task-0.md) | One server running web + app + code + DB (LAMP). Baseline; everything is a SPOF |
| [1 — Distributed](task-1.md) | HAProxy load balancer + a second server + DB Primary-Replica (redundancy + read scaling) |
| [2 — Secured and monitored](task-2.md) | 3 firewalls + SSL certificate (HTTPS) + 3 monitoring clients |
| [3 — Scale up](task-3.md) | A second HAProxy (cluster) + components split onto their own servers |

## Tasks

### [Task 0 — Simple web stack](task-0.md)
![Simple web stack](diagrams/0-simple_web_stack.png)

### [Task 1 — Distributed web infrastructure](task-1.md)
![Distributed web infrastructure](diagrams/1-distributed_web_infrastructure.png)

### [Task 2 — Secured and monitored](task-2.md)
![Secured and monitored web infrastructure](diagrams/2-secured_and_monitored_web_infrastructure.png)

### [Task 3 — Scale up](task-3.md)
![Scale up](diagrams/3-scale_up.png)

## Acronyms

| Acronym | Meaning |
|---------|---------|
| LAMP | Linux, Apache, MySQL, PHP/Perl/Python (here Nginx replaces Apache → LEMP) |
| SPOF | Single Point Of Failure — a component whose sole failure downs everything |
| QPS | Queries Per Second — the load metric |
| HA | High Availability — staying up despite failures, via redundancy |

## Repository layout

| Path | Holds |
|------|-------|
| `README.md` | This overview |
| `task-0.md` … `task-3.md` | Per-task requirements, diagram, and explanations |
| `0-simple_web_stack` | Answer file — screenshot URL for Task 0 |
| `1-distributed_web_infrastructure` | Answer file — screenshot URL for Task 1 |
| `2-secured_and_monitored_web_infrastructure` | Answer file — screenshot URL for Task 2 |
| `3-scale_up` | Answer file — screenshot URL for Task 3 |
| `diagrams/` | Diagram sources (`.svg`), rendered `.png`, and `make_diagrams.py` |

## How to submit

1. Upload each `diagrams/<task>.png` to an image host (e.g. imgur).
2. Paste each image link into the matching answer file (`0-simple_web_stack`, …),
   replacing the placeholder URL.
3. Push to GitHub and put the answer file's link into the Holberton URL box.

## Regenerating the diagrams

Diagrams are generated from `diagrams/make_diagrams.py` (SVG) and rendered to PNG
with `rsvg-convert`:

```
cd diagrams
python3 make_diagrams.py
for f in *.svg; do rsvg-convert -z 2 "$f" -o "${f%.svg}.png"; done
```
