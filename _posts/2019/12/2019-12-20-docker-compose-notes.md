---
categories: [tech, DevOps]
tags: [docker]
---

Docker-compose is a client for arranging docker resources.

Here are some notes:

### depends_on

- This param define the compose **run** sequence, **Not** the build sequence
- **run** sequence do not mean the **ready** swquence. Some image need 10+ or more seconds to be ready (like mysql, many engines need to run)

  Generally speaking, it's almost useless

### networks

- network's ```aliases``` make alternativle hostnames which can be used in other containers that under same network
- ```pod``` works like aliases, every service may define only one pid, so it's not as good as ```aliases```
- network can use driver, it's bind the basic networks which include ```host, bridge, none```
- networks driver's behavior are different on different system, as docker runs under virtualbox on MAC or WIN
- if the network want to bind and publish ports to the host network on MAC or WIN, you should use ```bridge```. But use ```host``` on LINUX
