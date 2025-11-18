# DHI in GitHub Actions CI/CD

Learn how to integrate Docker Hardened Images into your GitHub Actions workflows for automated security scanning and build blocking.

## Building with DHI in CI/CD

When using DHI in your CI/CD pipeline, you should:
1. Build with DHI base images
2. Add SBOM and provenance attestations
3. Scan for vulnerabilities
4. Block builds with critical issues

## Example: Basic DHI Build

Create a workflow file `.github/workflows/dhi-build.yml`:

```yaml
name: Build with DHI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      
      - name: Build with DHI
        uses: docker/build-push-action@v6
        with:
          context: .
          push: true
          tags: $orgname$/demo-node-dhi:latest
          provenance: mode=max
          sbom: true
```

## Try It Yourself

1. Create the workflow file in your repository
2. Update the Dockerfile to use DHI base:
  
   ```dockerfile
   FROM $$orgname$$/dhi-node:24.9.0-debian13
   ```

3. Push to GitHub and watch the action run

## What Gets Added?

With `provenance: mode=max` and `sbom: true`, your image automatically includes:
- SLSA provenance attestation
- SBOM (Software Bill of Materials)
- Build parameters and environment details
- Source code commit information

These attestations are cryptographically signed and can be verified later.
