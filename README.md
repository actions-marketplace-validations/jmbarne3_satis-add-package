# Satis - Add Package
Adds a package to the satis repository

## Inputs

| Input        | Description                                                                                             | Required                     |
|--------------|---------------------------------------------------------------------------------------------------------|------------------------------|
| token        | GitHub token that will be used for composer when invoked by satis.<br/>Currently auth is static for "github-oauth" | Yes                          |
| package_url  | The url of the package to add | Yes |
| package_name | The name of the package to add | Yes |

## Outputs
None

## Usage

Example `.github/workflow/satis-add-package.yaml`
```yaml
# eg. Can be used to host your internal composer package registry on github pages.
name: Satis Add Package

on:
  - workflow_dispatch

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: jmbarne3/satis-add-package@v1
        with:
          token: ${{ GITHUB_TOKEN }} # App/OAuth token, PAT
          package_url: ${{ inputs.package_url }}
          package_name: ${{ inputs.package_name }}
      - env:
          GIT_EMAIL: bot@github.com
          GIT_NAME: cool-bot
        run: |
          git config user.name $GIT_NAME
          git config user.email $GIT_EMAIL
          git add docs/
          git commit -m "Re-built repository assets"
          git push
```

Example `satis.json`
```json
{
  "name": "my-cool/repository",
  "homepage": "https://where-are-you/",
  "repositories": [
    {
      "packagist.org": false
    },
    {
      "type": "vcs",
      "url": "https://github.com/my-cool-org/the-repository"
    }
  ],
  "require-all": true,
  "output-dir": "docs"
}
```
