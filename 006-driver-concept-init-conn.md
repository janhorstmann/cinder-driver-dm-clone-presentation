# Driver concept

## Initialize Connection

* Return remote connector if requested through volume admin metadata
  * use configured target driver
  *  e.g. connecting the source of a clone target, host-assisted volume migration, backup, ...
* Return local connector if connector indicates connection from same host as volume
* Transfer volume to destination and return a local connector

```mermaid
sequenceDiagram
  box volume host
  participant a as cinder-volume
  end
  participant db as Database
  box connector host
  participant b as cinder-volume
  end
  a->>+a: ...<br/>driver.initialize_connection()
  opt connector host is not volume host
    a->>db: Create volume<br/> object for Host B
    a->>b: create_volume()
    b->>+b: ...<br/>driver.create_volume()
    b->>b: Create lv
    b->>b: Create linear target
    b->>db: get source volume by id from admin metadata
    b->>db: update admin metadata of source volume<br/> to request remote connection
    b->>a: update_attachment()<br/>Create attachment for source volume
    a->>+a: ...<br/>driver.initialize_connection()
    a->>-a: target_driver.initialize_connection()
    a->>b: return connection_info
    b-->>a: connector.connect_volume()
    b->>b: Create metadata lv
    b->>b: load clone target
    opt this is the first attachment
      b->>-b: enable hydration
    end
    loop until volume.status == "available"
      a->>db: refresh()
    end
    a->>db: switch volume name_id,<br/> attributes, attachments
  end
  a->>-a: return local connector
```

---

[prev](005-driver-concept.md) [TOC](000-toc.md) [next](007-driver-concept-term-conn.md)
