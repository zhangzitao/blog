---
categories: [tech, DevOps]
tags: [k8s]
---

After a complely setting, I finally run the k8s on a aws free host, which has one cpu core and 1G memory, with ```--vm-driver=none``` option. 

But serveral hours later, the service was down. I tried many ways to turn on the service.

Finding some containers runnning cann't be stopped, I forced remove ```rm -f``` the container. Then restart the service.

Every thing looks fan but servaral hours later, error happened again...

Because golang has GC, maybe the 2core 4G is the least configuration to run k8s.

Hoping the container service could be re-write by any non-GC language such as rust.
