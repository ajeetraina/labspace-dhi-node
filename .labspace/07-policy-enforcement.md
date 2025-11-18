# Policy Enforcement & Blocking Builds

Learn how to enforce security policies that block builds not using DHI or containing vulnerabilities.

## Blocking Vulnerable Builds

Use Docker Scout to scan and block builds with critical vulnerabilities:

```yaml
- name: Docker Scout CVE Scan
  uses: docker/scout-action@v1
  with:
    command: cves
    image: myorg/myapp:latest
    only-severities: critical,high
    exit-code: true  # ⭐ Fails workflow if CVEs found
```

## Enforcing DHI Usage

Create a Docker Scout policy to only allow DHI base images:

1. Go to Docker Scout Dashboard
2. Create "Approved Base Images Policy"
3. Configure allowed repositories:
   - `$$orgname$$/dhi-*`
4. Enable policy enforcement

```yaml
- name: Check Base Image Policy
  uses: docker/scout-action@v1
  with:
    command: policy
    image: myorg/myapp:latest
    exit-code: true  # ⭐ Blocks non-DHI builds
```

## Try It Yourself

1. Try building with the original `node:24.9.0-trixie-slim` base
2. Watch the workflow fail the policy check
3. Switch to `$$orgname$$/dhi-node:24.9.0-debian13`
4. Build passes!

## VEX Integration

DHI images include VEX statements that reduce false positives:

```bash
# Extract VEX statements
docker scout vex get $$orgname$$/demo-node-dhi:v1 --output vex.json

# Use with external tools (Grype, Trivy)
grype $$orgname$$/demo-node-dhi:v1 --vex vex.json
```

VEX statements automatically mark non-exploitable vulnerabilities, reducing alert fatigue.
