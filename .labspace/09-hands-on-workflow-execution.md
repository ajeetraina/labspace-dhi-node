# Hands-On: Running the DHI Workflows

This section provides step-by-step instructions to set up and execute the GitHub Actions workflows for your webinar demonstration.

## Prerequisites

- GitHub account with access to the repository
- Terminal/command line access
- Git installed

## Step 1: Clone and Setup Repository

Create a new project directory and clone the repository:

```bash
mkdir newproj
cd newproj
git clone https://github.com/ajeetraina/labspace-dhi-node
cd labspace-dhi-node
```

## Step 2: Switch to Solution Branch

**IMPORTANT:** All workflows are configured to run on the `solution` branch.

```bash
git checkout solution
```

Verify you're on the correct branch:
```bash
git branch
# Should show: * solution
```

## Step 3: Install GitHub CLI

Update package lists and install GitHub CLI:

```bash
sudo apt update
sudo apt install gh
```

Verify installation:
```bash
gh --version
# Should show: gh version x.x.x (date)
```

## Step 4: Authenticate with GitHub

Authenticate GitHub CLI with your account:

```bash
gh auth login
```

Follow the prompts:
1. Select: **GitHub.com**
2. Select: **HTTPS**
3. Select: **Login with a web browser** (recommended)
4. Copy the one-time code shown
5. Press Enter to open browser
6. Paste code and authorize

Verify authentication:
```bash
gh auth status
# Should show: ✓ Logged in to github.com as <your-username>
```

## Step 5: Set Default Repository

Set the current repository as default for gh commands:

```bash
gh repo set-default
```

Select: **ajeetraina/labspace-dhi-node**

## Step 6: Explore the Workflows

View available workflows:

```bash
ls .github/workflows/
```

You should see:
```
demo-bad-build.yml
dhi-basic-build.yml
dhi-full-pipeline.yml
dhi-policy-enforcement.yml
dhi-scan-and-block.yml
publish-labspace.yaml
```

## Step 7: Run Your First Workflow - Bad Build Demo

Trigger the non-DHI demonstration workflow:

```bash
gh workflow run demo-bad-build.yml --ref solution
```

**Expected Output:**
```
✓ Created workflow_run
```

## Step 8: Monitor Workflow Execution

Watch the workflow run in real-time:

```bash
gh run watch
```

Or view workflow runs:
```bash
gh run list --workflow=demo-bad-build.yml
```

Or open in browser:
```bash
gh workflow view demo-bad-build.yml --web
```

## Step 9: View Results

Check the workflow results:

```bash
# List recent runs
gh run list --limit 5

# View specific run details
gh run view <run-id>
```

Or view in GitHub UI:
1. Go to: https://github.com/ajeetraina/labspace-dhi-node/actions
2. Click on the latest workflow run
3. Expand job details
4. Review the summary

## What to Look For in demo-bad-build Results

The workflow demonstrates vulnerabilities in standard images:

✅ **Expected Findings:**
- **2 High-Severity CVEs** in glob package
- **CVE Scan Failed** - Vulnerabilities detected
- **Policy Check Failed** - Non-approved base image
- **Image Size:** ~101 MB
- **Packages:** ~804 packages

✅ **Workflow Summary Shows:**
```
⚠️ Non-DHI Build Demonstration
❌ CVE Scan Failed - Critical/High vulnerabilities detected
❌ Policy Check Failed - Non-approved base image

Solution: Use Docker Hardened Images for:
- Zero known vulnerabilities
- 90% smaller attack surface
- Built-in attestations (SBOM, VEX, SLSA)
- Compliance ready (FIPS, STIG)
```

## Step 10: Run DHI Workflows

Now trigger the DHI workflows to see the comparison:

### Option A: Trigger Individual Workflows
```bash
# Basic DHI build
gh workflow run dhi-basic-build.yml --ref solution

# Scan and block
gh workflow run dhi-scan-and-block.yml --ref solution

# Policy enforcement
gh workflow run dhi-policy-enforcement.yml --ref solution

# Full pipeline (requires fixes first)
gh workflow run dhi-full-pipeline.yml --ref solution
```

### Option B: Trigger Multiple via Push
```bash
# This triggers all workflows configured for 'solution' branch
git commit --allow-empty -m "Demo: Trigger all DHI workflows"
git push origin solution
```

## Step 11: Compare Results

After workflows complete, compare the results:

| Metric | Standard Node | DHI Node |
|--------|--------------|----------|
| Base Image | node:24-alpine | dockerdevrel/dhi-node:24.9.0 |
| CVEs | 2+ High | 0 ✅ |
| Image Size | 101 MB | ~15-20 MB |
| Packages | 804 | ~40-50 |
| Policy | Failed ❌ | Passed ✅ |
| SBOM | No | Yes ✅ |
| Provenance | No | SLSA L3 ✅ |
| VEX | No | Yes ✅ |

## Troubleshooting

### Workflow not triggering?
- Ensure you're on `solution` branch: `git branch`
- Check authentication: `gh auth status`
- Verify repo access: `gh repo view`

### Authentication errors?
- Re-authenticate: `gh auth login --web`
- Check token scopes: `gh auth status`

### Workflow fails?
- Check secrets are configured:
  - `DOCKERHUB_USERNAME`
  - `DOCKERHUB_TOKEN`
- View logs: `gh run view <run-id> --log`

### Can't see workflow runs?
- Open in browser: `gh workflow view --web`
- Check Actions tab: https://github.com/ajeetraina/labspace-dhi-node/actions

## Quick Reference Commands

```bash
# List all workflows
gh workflow list

# Run specific workflow
gh workflow run <workflow-name> --ref solution

# Watch current run
gh run watch

# List recent runs
gh run list --limit 10

# View run details
gh run view <run-id>

# Open workflow in browser
gh workflow view <workflow-name> --web

# Cancel a run
gh run cancel <run-id>

# Re-run a failed workflow
gh run rerun <run-id>
```

## Next Steps

After successfully running the workflows:
1. Review each workflow's job summary
2. Download and examine SBOM/VEX artifacts (from full pipeline)
3. Compare vulnerability reports
4. Prepare screenshots for webinar
5. Practice the demo flow

## Webinar Demo Flow Recommendation

1. **Start with demo-bad-build** (show the problem)
2. **Show vulnerability details** (glob command injection CVEs)
3. **Explain attack vectors** (CI/CD compromise scenario)
4. **Run dhi-basic-build** (show the solution)
5. **Compare results** (side-by-side metrics)
6. **Demonstrate blocking** (dhi-scan-and-block)
7. **Show policy enforcement** (dhi-policy-enforcement)
8. **Full pipeline** (complete SDLC with SBOM/VEX)

## Pro Tips for Webinar

- ✅ **Run all workflows at least once before webinar** to ensure everything works
- ✅ **Take screenshots of key results** as backup slides
- ✅ **Have both terminal and GitHub UI ready** for different views
- ✅ **Prepare the comparison table** with actual numbers from your runs
- ✅ **Queue up the Actions page** before starting to minimize navigation
- ✅ **Test your screen sharing** to ensure both terminal and browser are visible
