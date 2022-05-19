# action-next-semver
Return the version incremented by the release type obtained from pull request tags.
Аvailable tags:
- major
- premajor
- minor
- preminor
- patch
- prepatch
- prerelease

## Usage
```yaml
name: PR release version

on:
  pull_request:
    types: [opened]

jobs:
  replace-title:
    runs-on: ubuntu-latest
    steps:
      - name: "Checkout repository"
        uses: actions/checkout@v2
        with:
          fetch-depth: 0

      - name: get current version
        id: package-version
        uses: martinbeentjes/npm-get-version-action@master

      - name: get next version
        id: next-package-version
        uses: KazanExpress/action-next-semver@1.0.0
        with:
          version: ${{ steps.package-version.outputs.current-version}}

      - name: replace pr title
        uses: KazanExpress/action-replace-pr-title@1.0.0
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          find: "^.*$"
          replace: "Release v${{ steps.next-package-version.outputs.next-version }}"
```
