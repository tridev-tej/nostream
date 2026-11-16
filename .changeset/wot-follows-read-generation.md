---
---

fix(services): keep a stale follows read from overwriting a newer cached list (#1098)

`get_follows` cached what it read from storage after the await, so an `update_follow_list` that
landed during the read was overwritten by the older list, or an emptied list was put back, and
every later rebuild walked the stale follows. `update_follow_list` now bumps a generation counter
under the cache lock, and a read only caches its result when the counter has not moved.
