# Driver concept

## Snapshots

> [!NOTE]
> Snapshot support **not** yet implemented in PoC! Concept tested manually

<pre class="mermaid">
block-beta
columns 3
block:src_node:1
  columns 3
  space:3
  space:3
  src_real[("source-real\nlinear")] space:1 src_a[("source\nsnapshot-origin")]
  space:3
  src_snap[("snap\nsnapshot")] space:1 src_snap_cow[("snap-cow\nlinear")]
  src_a --> src_real
  src_real --> src_a
  src_snap --> src_snap_cow
  src_snap --> src_real
  src_real --> src_snap
end
space:1
block:dst_node:1
  columns 4
  origin[("origin\nsnapshot-origin")] space:1 clone[("real\nclone")]:1 snap[("snap\nsnapshot")]
  space:4
  src[("source")] md[("metadata\nlinear")] dst[("destination\nlinear")] snap_cow_clone[("snap-cow\nclone")]
  space:4
  snap_cow[("snap-cow\nlinear")] snap_cow_md[("snap-metadata\nlinear")] snap_cow_dst[("snap-cow-destination\nlinear")] space:1
  src --> clone
  md --> clone
  clone --> md
  dst --> clone
  clone --> dst
  snap_cow --> snap_cow_clone
  snap_cow_md --> snap_cow_clone
  snap_cow_clone --> snap_cow_md
  snap_cow_dst --> snap_cow_clone
  snap_cow_clone --> snap_cow_dst
  origin --> clone
  clone --> origin
  snap --> clone
  clone --> snap
  snap --> snap_cow_clone
end
src_a --"iSCSI / NVMeoF / ..."--> src
src_snap_cow --"iSCSI / NVMeoF / ..."--> snap_cow
classDef remote stroke-dasharray: 5 5;
classDef node fill:#ffdead;
class src,snap_cow remote
class src_node, dst_node node
</pre>

---

[prev](008-driver-concept-unsolved.md) [TOC](000-toc.md) [next](010-driver-concept-snap-2.md)

<script type="module">
	import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs';
	mermaid.initialize({
		startOnLoad: true,
		theme: 'default'
	});
</script>
