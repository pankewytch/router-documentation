# DNS Architecture


NOTE: These are notes and musings, not a finalized file. Keeping track of things while the project crystalize into a fully functional system.

## Why dual layer
 
Dual layer DNS server. Internal is Bind9, external is Unbound.

- Unbound better with DNSSEC
- RPZ can manage ban lists better
- Unbound does not need to know anything about the network or clients