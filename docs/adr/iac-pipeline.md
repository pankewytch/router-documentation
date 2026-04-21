# IaC Pipeline

## Push
### Overview
On every push, the RPZ blocklist pipeline runs regardless of what 
changed. This ensures blocklist currency without requiring a full 
deployment, and validates all generated zone files before they can reach Bind9.

### Why On Every Push
Blocklist updates are content changes. They are high frequency, low blast radius, appropriate for automation. Running validation on every push regardless of what changed means the validation is a guarantee, not a sometimes-true assertion. By the time a tag is cut, all generated files are known good.

### Steps
1. Fetch current blocklists from sources defined in data/rpz-sources.txt
2. Generate one RPZ zone file per source list
3. Run named-checkzone validation against each generated file
4. Fail the push if any zone file is invalid — generated files never reach Bind9 if validation fails
5. Alert on failure via webhook

### Generated Files
RPZ zone files are build artifacts and are not committed to the 
repository. They are generated at pipeline runtime and deployed 
directly to Bind9. The gitignore excludes all generated RPZ output.

## Tag

## Overview

On a tag, the repository should be built and deployed to the router. During the deployment, each of the configs being deployed should be verified. The services will be restarted when a change is made to any config files used in that service.

### Why Tags for Deployment
Configuration changes carry high blast radius — a broken firewall 
ruleset or DNS config can take down the entire network. Requiring a tag means no deployment happens accidentally or automatically. Every deployment is intentional and traceable.

### Steps
1. Build the tarball, excluding the .git and .github directories.
2. Sign tarball with private key on the git runner Pi
3. Verify signature immediately after signing
4. Upload signed artifact and signature to GitHub as release artifacts
5. SCP artifact and signature to Sol via transfer user
   - Transfer restricted via validate-scp.sh
6. SSH trigger to deploy user on Sol
   - ForceCommand runs verify-artifact.sh
7. verify-artifact.sh checks TPM hash of public key
8. openssl verifies artifact signature against trusted public key
9. Artifact extracted to staging, previous deployment backed up
10. deploy.sh runs from extracted artifact
11. Services restarted for any changed configs