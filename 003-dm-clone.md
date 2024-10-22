# Device-mapper clone target

```mermaid
block-beta
columns 3
block:src_node["node 1\n\n\n\n\n\n\n\n"]:1
  columns 3
  space:3
  space:3
  space:2 src_a[("source")]
end
space:1
block:dst_node["node 2\n\n\n\n\n\n\n\n"]:1
  columns 3
  space:2
  clone[("clone")]:1
  space:3
  src[("source")]
  md[("metadata")]
  dst[("destination")]
  src --> clone
  md --> clone
  clone --> md
  dst <--> clone
  clone --> dst
end
src_a --"iSCSI / NVMeoF / ..."--> src
classDef remote stroke-dasharray: 5 5;
classDef node fill:#ffdead;
class src remote
class src_nodel, dst_node node
```

* multi-attachment for live-migration?
* failure modes
  * failure of source node + network partitions
    * IO errors => FS read-only, data unavailable
    * dm-clone retries indefinitly => recoverable
  * failure of destination node
    * data unavailable until recovery
    * driver should reconstruct the dm-clone target

---

[prev](002-cloud-architecture) [TOC](000-toc.md) [next](004-dm-clone-2.md)
