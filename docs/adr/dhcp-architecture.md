# DHCP Architecture

NOTE: These are notes and musings, not a finalized file. Keeping track of things while the project crystalize into a fully functional system.

## Why leases in PostgreSQL

Doing leases in postgres to:
- Simplify config file
- Leverage DDNS to have readable and recognizable client names
- Postgres can be used for other packages/applications
- Allows me to write a better api for logging if I chose to