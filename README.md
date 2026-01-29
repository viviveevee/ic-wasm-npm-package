# ic-wasm npm Package

This repository contains the npm package distribution system for `ic-wasm` using the **bundled binaries per-architecture** approach.

## Repository Structure

- **`ic-wasm/`** - Main wrapper package with binary wrapper script and programmatic API
- **`ic-wasm-{platform}-{arch}/`** - Platform-specific packages containing pre-compiled binaries
- **`scripts/`** - Build and deployment automation

## How It Works

1. **Platform-specific packages** (`ic-wasm-darwin-arm64`, etc.) each contain a pre-compiled binary for their respective platform
2. **Main package** (`ic-wasm`) uses `optionalDependencies` to automatically install only the correct platform package
3. **Wrapper script** (`ic-wasm/bin/ic-wasm.js`) finds and executes the platform-specific binary

## Quick Start

### 1. Download Binaries

Download the pre-compiled binaries for all platforms:

```bash
./scripts/download-binaries.sh 0.9.11
```

Or manually download from [ic-wasm releases](https://github.com/dfinity/ic-wasm/releases) and place them in the respective `bin/` directories.

### 2. Verify Binaries

```bash
./scripts/verify-binaries.sh
```

### 3. Test Locally

```bash
./scripts/test-docker.sh
```

### 4. Update Version (if needed)

```bash
./scripts/update-package-json.sh 0.9.11
```

This will update the version in all packages and their dependencies.

### 5. Publish to npm

First, make sure you're logged in to npm:

```bash
npm login
```

Then publish all packages:

```bash
./scripts/publish-all.sh 0.9.11
```

**Important:** Platform packages must be published before the main package!

## Usage After Publishing

Users can install the package globally:

```bash
npm install -g ic-wasm
```

Or locally in their project:

```bash
npm install ic-wasm
```

Then use it:

```bash
ic-wasm --help
```

Or programmatically:

```javascript
const icWasm = require('ic-wasm');
console.log('Binary location:', icWasm.binaryPath);
```

## Maintenance

### Releasing a New Version

1. **Trigger Version Bump workflow**  
   Go to Actions → Version Bump → Run workflow  
   Enter the new version (e.g., `0.9.11`)  
   This downloads binaries, verifies them, updates all package.json files, and creates a PR

2. **Review and merge the PR**  
   The Test workflow runs automatically to validate the changes

3. **Publish to npm**  
   Push a tag to trigger publishing:
   ```bash
   git tag v0.9.11
   git push origin v0.9.11
   ```
