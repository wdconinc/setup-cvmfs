# GitHub Action: eic/setup-cvmfs
This GitHub Action sets up CernVM-FS for use in GitHub Workflows.

## Instructions
You can use this GitHub Action in a workflow in your own repository by with `uses: eic/setup-cvmfs@main`.

For example, the file `.github/workflows/tests.yml` could include the following stanza:
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: eic/setup-cvmfs@main
```

## Optional Parameters
The following parameters are supported:
- `cvmfs_repositories` (optional, defaults to `atlas.cern.ch,atlas-condb.cern.ch,grid.cern.ch`): the list of repositories to load.
- `cvmfs_http_proxy` (optional, defaults to `DIRECT`): the http proxy to use with cvmfs.

## Minimal Example

The following minimal example, which is also a workflow in this repository at [.github/workflows/minimal-usage.yml](https://github.com/eic/setup-cvmfs/tree/main/.github/workflows/minimal-usage.yml), setups up CernVM-FS and lists the contents of the `/cvmfs/eic.opensciencegrid.org` directory.
```yaml
name: Test setup-cvmfs action
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - uses: eic/setup-cvmfs@main
    - name: Setup CernVM-FS
      run: cat /etc/cvmfs/default.local && ls /cvmfs/eic.opensciencegrid.org/
```

## What Does This Action Do?

This GitHub Action installs the [CernVM-FS debian package](https://ecsft.cern.ch/dist/cvmfs/cvmfs-release/cvmfs-release-latest_all.deb), and configures it with the `CVMFS_REPOSITORIES` and `CVMFS_HTTP_PROXY` settings optionally specified as arguments. It configures autofs to automatically mount the cvmfs repositories that are accessed.

## Limitations

This GitHub Action makes no attempt at caching. Frequent use may incur overhead on the CernVM-FS servers you are accessing.

This GitHub Action is only expected to work in workflows that [run on](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on) ubuntu targets (and even then likely only `ubuntu-latest`). This exludes the `macos` and `windows` targets.
