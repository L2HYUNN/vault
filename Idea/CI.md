`pnpm`을 이용한 CI

```yaml
name: CI

on:
  pull_request:
    branches: [main, dev]
  push:
    branches: [main]
env:
  CACHED_DEPENDENCY_PATHS: ${{ github.workspace }}/node_modules
  CACHED_BUILD_PATHS: ${{ github.workspace }}/build
  BUILD_CACHE_KEY: ${{ github.sha }}
  NODE_VERSION: '16'

jobs:
  job_install_dependencies:
    name: Install Dependencies
    runs-on: ubuntu-latest
    timeout-minutes: 15
    steps:
      - name: Check out current commit (${{ github.sha }})
        uses: actions/checkout@v3

      - name: Set up node v${{ env.NODE_VERSION }}
        uses: actions/setup-node@v3
        with:
          node-version: ${{ env.NODE_VERSION }}

      - name: Install pnpm
        uses: pnpm/action-setup@v2
        with:
          version: 7
          run_install: false

      - name: Compute dependency cache key
        id: compute_lockfile_hash
        run: echo "::set-output name=hash::${{ hashFiles('pnpm-lock.yaml') }}"

      - name: Check dependency cache
        uses: actions/cache@v3
        id: cache_dependencies
        with:
          path: ${{ env.CACHED_DEPENDENCY_PATHS }}
          key: ${{ steps.compute_lockfile_hash.outputs.hash }}

      - name: Install dependencies
        if: steps.cache_dependencies.outputs.cache-hit == ''
        run: pnpm install
    outputs:
      dependency_cache_key: ${{ steps.compute_lockfile_hash.outputs.hash }}

  job_continuous_integration:
    name: Check prettier & eslint
    runs-on: ubuntu-latest
    needs: [job_install_dependencies]
    steps:
      - name: Check out current commit (${{ github.sha }})
        uses: actions/checkout@v3

      - name: Set up node
        uses: actions/setup-node@v3
        with:
          node-version: ${{ env.DEFAULT_NODE_VERSION }}

      - name: Install pnpm
        uses: pnpm/action-setup@v2
        with:
          version: 7
          run_install: false

      - name: Check dependency cache
        uses: actions/cache@v3
        with:
          path: ${{ env.CACHED_DEPENDENCY_PATHS }}
          key: ${{ needs.job_install_dependencies.outputs.dependency_cache_key }}

      - name: Check prettier
        run: pnpm format:fix

      - name: Check eslint
        run: pnpm lint:fix
```
