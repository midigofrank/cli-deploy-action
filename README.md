# OpenFn GitHub Deploy Action

Deploys an OpenFn project workspace to Lightning using `openfn project deploy`,
then commits the updated project state back to the branch.

The repository must contain an `openfn.yaml` workspace (as created by
`openfn project pull` or by Lightning's GitHub sync). Commit the `.openfn/`
folder: deploy uses it to detect changes made in Lightning since the last sync.

`endpoint_input` is optional. Without it, the CLI uses the endpoint the
workspace already tracks for the project, then `OPENFN_ENDPOINT`, then
app.openfn.org.

> `v1` of this action uses the legacy `config.json` / `openfn deploy` format.
> Pin to `@v1` if your repo still uses it.

```yaml
on:
  push:
    branches:
      - main
  workflow_dispatch:
    inputs:
      force:
        type: boolean
        default: false

jobs:
  deploy-to-lightning:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: openfn deploy
        uses: openfn/cli-deploy-action@v2
        with:
          secret_input: ${{ secrets.OPENFN_API_KEY }}
          project_id_input: <project uuid>
          endpoint_input: https://app.openfn.org
          force_input: ${{ inputs.force || false }}
          workspace_input: projects/my-project # optional, defaults to the repo root
```
