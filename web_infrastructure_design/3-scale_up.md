# Task 3 — Scale up

## What the task asks

Add capacity and remove the load balancer as a single point of failure.

Required additions over Task 2:

- 1 extra server
- 1 load balancer (HAProxy), configured as a cluster with the existing one
- Split the components (web server, application server, database) so each runs
  on its own server

You must be able to explain, for every added element, why it's there.

## Diagram

![Scale up](3-scale_up.png)

## Explanations

| Added element | Why it's added |
|---------------|----------------|
| Second HAProxy (cluster) | Removes the load balancer as a SPOF — e.g. Active-Passive with a floating VIP and a heartbeat: the standby takes over (failover) if the active LB dies |
| Split web / app / database servers | Each tier scales independently, stops contending for resources, and can run on hardware tuned to its role |

## Notes for the defense

- This is a **horizontal** split: tiers are separated so each can be duplicated
  on its own. It is not the same as scaling up a single box's hardware.
- A remaining SPOF after this task is the single write-capable database Primary.

## Answer file

`3-scale_up` holds the URL of this diagram's screenshot.
