# Driver concept

## Unsolved problems

* The nova scheduler unaware of available storage space
  * Instances may repeatedly not be created/migrated
  * Custom Scheduler filter could take available space into account
* Volume not attachable if it exceeds available space on instance's host
  * No transparent communication to user
  * Only mitigatable on operational level
    * Monitor disk space and extended or move instances once threshold is reached
    * Limit volume size for this driver
      * `per_volume_gigabytes` not available per volume type
* Some volume actions will fail **while a volume is being hydrated**, e.g.:
    * Attaching the volume on another host (e.g. attachment/detachment to/from another instance)
      * would require chaining of multiple clone targets
    * Extending volume
      * Unsupported by device mapper clone target
    * live-migration
    * ...
  * No transparent communication to user
  * Set volume to `maintenance` until hydration finishes?
    * Cinder sets `status` after create/attach
    * Detachment would not be allowed, although technically possible
      * expectation is that detachment is always possible, e.g. server rebuild (?)
  * Currently volume is set to `maintenance` on first unsupported action

---

[prev](007-driver-concept-term-conn.md) [TOC](000-toc.md) [next](009-driver-concept-snap.md)
