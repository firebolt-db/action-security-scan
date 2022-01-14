# Firebolt security scan

GitHub Action for repository security scanning, using Sonar Cloud and Fossa.

## Pre-requisites

1. [Sonar Cloud project](https://sonarcloud.io/projects) created.
1. Sonar Cloud `sonar-project.properties` file with at least `sonar.projectKey` and `sonar.organization` parameters specified. [Parameter List](https://docs.sonarcloud.io/advanced-setup/analysis-parameters/)
1. (Optional) `fossa.yml` to [customise Fossa behaviour](https://github.com/fossas/fossa-cli/blob/master/docs/references/files/fossa-yml.md).

## Inputs

### `github-key`
**Required:** [Github token](https://docs.github.com/en/actions/security-guides/automatic-token-authentication) required by the Sonar Cloud. Is set by default by GitHub in your repository as `secrets.GITHUB_TOKEN`.
### `fossa-key`
**Required:** [Fossa API key](https://docs.fossa.com/docs/api-reference#api-tokens).
### `sonar-key`
**Required:** Required this is the token used to authenticate access to SonarCloud. You can generate a token on your [Security page](https://sonarcloud.io/account/security/) in SonarCloud.

## Results

1. Sonar Cloud will post a comment on the PR with the scan result. It will also display in "checks". Clicking on any of the links in the comment as well as details in the check will bring you to the project's Sonar Cloud page.
1. Fossa failures will fail the action execution. You'll be able to find the link to the project in the logs.

## Known limitations

1. Sonar Cloud code coverage is not implemented and will report 0%. This will randomly pass/fail the scan.
1. Fossa PR decoration is not working. As a workaround fossa-test is run after initial scan that will fail the action execution if any issues are found.

## Example

```yml
jobs:
  security-scan:
    runs-on: ubuntu-latest
    steps:
      - name: "Checkout Code"
        uses: actions/checkout@v2

      - name: "Security Scan"
        uses: firebolt-db/action-security-scan@main
        with:
          github-key: ${{ secrets.GITHUB_TOKEN }}
          fossa-key: ${{ secrets.FOSSA_TOKEN }}
          sonar-key: ${{ secrets.SONARCLOUD_TOKEN }}
```