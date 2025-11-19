# DHI in GitHub Actions CI/CD

This repository contains 6 comprehensive GitHub Actions workflows demonstrating Docker Hardened Images (DHI) in a complete SDLC pipeline.

## Available Workflows

### 1. **demo-bad-build.yml** - Non-DHI Demonstration
Shows what happens when NOT using DHI (for comparison):
- Uses standard `node:24-alpine` base image
- Demonstrates CVE vulnerabilities
- Shows policy failures
- Perfect for "before/after" comparisons

### 2. **dhi-basic-build.yml** - Basic DHI Build
Simple DHI build with essential security features:
- Uses DHI Node.js base image
- Generates SBOM attestations
- Generates SLSA provenance
- Demonstrates clean builds

### 3. **dhi-scan-and-block.yml** - Scan and Block
Build, scan, and block on security issues:
- Scans for critical/high CVEs
- Blocks on policy violations
- Demonstrates security gates

### 4. **dhi-policy-enforcement.yml** - Policy Enforcement
Enforces approved base images:
- Checks approved-base-images policy
- Blocks non-DHI builds
- Compares with baseline

### 5. **dhi-full-pipeline.yml** - Complete Production Pipeline
Full SDLC with all security gates:
- Build → Scan → Policy → Attestation → Deploy
- Extracts SBOM and VEX statements
- Verifies attestations
- Uploads compliance artifacts

### 6. **publish-labspace.yaml** - Labspace Publishing
Publishes Docker Compose Labspace

## Getting Started

Before running workflows, you need to:

1. **Clone the repository**
2. **Switch to solution branch** (workflows are configured for this branch)
3. **Install GitHub CLI**
4. **Authenticate with GitHub**
5. **Trigger workflows**

Follow the detailed steps in the next section: "Hands-On: Running the Workflows"

## What You'll Learn

- How to block builds not using DHI
- How to scan images and block on vulnerabilities
- How to extract and use SBOM/VEX statements
- How to enforce security policies in CI/CD
- How to implement complete SDLC security gates
