# Driver concept

## Terminate connection

* Local connections require no action
* Remote connections need correct termination by target driver
* Failed instance live-migrations require clean up of created volume and clone target
* Successful instance live-migrations require enabling hydration on destination volume

<pre class="mermaid">
sequenceDiagram
  box volume host
  participant a as cinder-volume
  end
  participant db as Database
  box source volume host
  participant b as cinder-volume
  end
  a->>+a: ...<br/>driver.terminate_connection()
  alt only a single remote attachment
    a->>a: target_driver.terminate_connection()
  else volume is a clone target and hydration has not started
    note over a,b: live-migration scenario
    alt host in connector is volume host
      note over a,b: live-migration has failed
      a->>a: load linear target
      a->>a: delete metadata
      a->>b: connector.disconnect_volume()<br/>(connection_info from attachment)
      a->>db: switch volume identities
      a->>a: delete_volume()
    else
      note over a,b: live-migration has succeded
      a->>-a: enable hydration
    end
  end
</pre>

---

[prev](006-driver-concept-init-conn.md) [TOC](000-toc.md) [next](008-driver-concept-unsolved.md)

<script type="module">
	import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs';
	mermaid.initialize({
		startOnLoad: true,
		theme: 'default'
	});
</script>
