---
categories: [tech, DevOps]
tags: [docker]
---

Vscode has a great extension called "remote container". I made some trial work.

As far as I know, it has this basic process below:

- use docker-compose to pull and build images from the ```docker-compose.yml``` file
- use docker-compose to up those images
- extension then install some plugins in the **main** service which defined in the ```devcontainer.json```
- thoes extension including vscode server expose some ports, so Host Vscode extension can communicate with the **main** service

### Strengths

- copy to anywhere, feel painless to debug your projects without environment configuration
- developing many project in different containers, you never meet ports occupied or environment variables problems
- fast in linux

### Weaknesses

- docker runs with virtualbox or hyperv in MacOS or Windows, so many system resources are wasted. That makes devcontainer runs very slow
