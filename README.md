## Commands 
pnpm tsc --init
pnpm changeset init




for publish.yml
```
name: Publish
on:
  push:
    branches:
      - "master"

concurrency: ${{ github.workflow }}-${{ github.ref }}

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: pnpm/action-setup@v2
        with:
          version: 7
      - uses: actions/setup-node@v3
        with:
          node-version: 16.x
          cache: "pnpm"

      - run: pnpm install --frozen-lockfile
      - name: Create Release Pull Request or Publish
        id: changesets
        uses: changesets/action@v1
        with:
          publish: pnpm run build && pnpm publish --access public
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```


add test commit