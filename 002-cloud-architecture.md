# OpenStack cloud architecture for local storage in cinder

```mermaid
block-beta
columns 2
block:node1["node 1"]:1
  columns 4
  i1(["instance 1"]):1 space:1 i2(["instance 2"]):1 space:1
  space:4
  b1[("volume 1")] b2[("volume 2")] b3[("volume 3")] b4[("volume 4")]
  b1 --- i1
  b2 --- i1
  b3 --- i2
end
block:node2["node 2"]:1
  columns 4
  i3(["instance 3"]):1 space:3
  space:4
  b5[("volume 4")] b6[("volume 5")] b7[("volume 6")] space:1
  b5 --- i3
  b6 --- i3
  b7 --- i3
end
b4 --- b5
classDef remote stroke-dasharray: 5 5;
classDef node fill:#ffdead;
class b5 remote
class node1, node2 node
```

* `cinder-volume` service on every hypervisor
  * security: DB credentials on every hypervisor
* volumes need to move transparently between hypervisors

---

[prev](001-motivation.md) [TOC](000-toc.md) [next](003-dm-clone.md)
