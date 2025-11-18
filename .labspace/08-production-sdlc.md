# Production SDLC Workflow

Putting it all together: A complete secure SDLC workflow with DHI.

## Complete Pipeline Example

```yaml
name: Production SDLC with DHI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  security-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Build with DHI
        uses: docker/build-push-action@v6
        with:
          context: .
          load: true
          tags: myapp:test
          provenance: mode=max
          sbom: true
      
      - name: Scout CVE Scan
        uses: docker/scout-action@v1
        with:
          command: cves
          image: myapp:test
          exit-code: true
      
      - name: Policy Check
        uses: docker/scout-action@v1
        with:
          command: policy
          image: myapp:test
          exit-code: true
      
      - name: Extract SBOM
        run: |
          docker scout sbom myapp:test --format spdx --output sbom.json
      
      - name: Upload SBOM Artifact
        uses: actions/upload-artifact@v4
        with:
          name: sbom
          path: sbom.json

  deploy:
    needs: security-scan
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to production
        run: echo "Deploying secure DHI-based image"
```

## Key Benefits

✅ **Automated Security**: Every build is scanned  
✅ **Policy Enforcement**: Non-DHI builds are blocked  
✅ **Attestations**: SBOM and provenance attached  
✅ **Audit Trail**: Complete supply chain visibility  
✅ **VEX Integration**: Reduced false positives  

## Real-World Results

Switching to DHI in your SDLC provides:
- **100% CVE reduction** in base images
- **90% attack surface reduction**
- **40% smaller image sizes**
- **Compliance ready** (FIPS, STIG, FedRAMP)
- **Supply chain security** (SLSA provenance)

## Try the Complete Workflow

1. Copy the complete workflow to `.github/workflows/production.yml`
2. Commit and push to GitHub
3. Watch the security gates in action
4. View SBOM artifacts in the Actions tab
