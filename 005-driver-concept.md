# Driver concept

## Design goals

* Stay within cinder driver framework
* Build on top of cinder LVM driver for volume management

## Key concept for volume transfer between hosts

* Create new volume on attaching host via RPC, based on dm-clone target
* Switch volume identities using `_name_id` property
* Immediately return local connector on behalf of remote volume service
* Supervise dm-clone target in a periodic job
  * Clean-up when transfer is done
    * Load linear target
    * Remove connection and delete source volume

* RPC can only carry limited information => side-channel required
  * currently using volume admin metadata

---

[prev](004-dm-clone-2.md) [TOC](000-toc.md) [next](006-driver-concept-init-conn.md)
