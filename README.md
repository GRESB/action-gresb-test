# action-gresb-test

GitHub Action to install and execute gresb-test.
This is a composite action that combines other actions, like:

- actions/setup-java@v3
- awalsh128/cache-apt-pkgs-action@v1.1.2
- actions/cache@v3

This action is used to install and execute
the [GRESB integration test tool called `gresb-test`](https://github.com/GRESB/test-automation).

This repository has no CI/CD workflows because the repository is public and the workflows would need GRESB credentials to execute. 
Therefore in order to test changes to this action, do it from a private repository that uses it.
To do that pull the action from a branch.
```yaml
    - uses: GRESB/action-gresb-test@<branch-name>
```

## Inputs

| Input                              | Description                                                                                                     | Required | Default                    |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------|----------|----------------------------|
| gresb-test-version                 | The version of gresb-test to use.                                                                               | true     |                            |
| cmd-args                           | The arguments passed to gresb-test.                                                                             | true     | "-V"                       |
| installation-github-pat            | The GitHub token used for downloading the installation script.                                                  | true     |                            |
| installation-aws-access-key-id     | The AWS access key id used to install gresb-test. Needs access to the gresb-application-versions S3 bucket.     | false    |                            |
| installation-aws-secret-access-key | The AWS secret access key used to install gresb-test. Needs access to the gresb-application-versions S3 bucket. | false    |                            |
| installation-aws-role-arn          | The AWS IAM role used to install gresb-test. Needs access to the gresb-application-versions S3 bucket.          | false    |                            |
| installation-aws-role-session-name | The session name used when getting the token.                                                                   | false    | "github-action-gresb-test" |
| test-aws-access-key-id             | The AWS access key id used to run tests that require interaction with AWS APIs.                                 | false    |                            |
| test-aws-secret-access-key         | The AWS secret access key used to run tests that require interaction with AWS APIs.                             | false    |                            |
| test-aws-role-arn                  | The AWS IAM role arn used to run tests that require interaction with AWS APIs.                                  | false    |                            |
| test-aws-role-session-name         | The session name used when getting the token.                                                                   | false    | "github-action-gresb-test" |
| test-mailtrap-token                | The Mailtrap token used to run tests that require interaction with Mailtrap APIs.                               | false    |                            |
| test-slack-token                   | The Slack token used to run tests that require interaction with Slack APIs.                                     | false    |                            |
| test-gresb-portal-user-password    | The password for the default test user in the portal.                                                           | false    |                            |
| test-cluster-api-key               | The Kafka cluster api key                                                                                       | false    |                            |
| test-cluster-api-secret            | The Kafka cluster api secret                                                                                    | false    |                            |
| test-datadog-api-key               | The Datadog API key.                                                                                            | false    |                            |
| test-datadog-app-key               | The Datadog APP key.                                                                                            | false    |                            |
| test-datadog-site                  | The Datadog site.                                                                                               | false    | "datadoghq.eu"             |
| test-github-token                  | The GitHub token used by the tests.                                                                             | false    |                            |

## Outputs

| Output | Description   |
|--------|---------------|
| log    | The test log. |


## Usage

The following workflow configuration installs and executes gresb-test on every pull reuqets commit.

```yaml
name: Continues Integration


on:
  pull_request:
    branches:
      - '*'


jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: test
        id: test
        uses: ./
        with:
          gresb-test-version: v0.30.0
          cmd-args: '-a portal -i main -m browser'
          installation-github-pat: ${{ secrets.BOT_PAT }}
          installation-aws-access-key-id: ${{ secrets.INSTALL_AWS_ACCESS_KEY_ID }}
          installation-aws-secret-access-key: ${{ secrets.INSTALL_AWS_SECRET_ACCESS_KEY }}
          test-gresb-portal-user-password: ${{ secrets.PORTAL_USER_PASSWORD }}
          cluster-api-key: ${{ secrets.KAFKA_CLUSTER_API_KEY }}
          cluster-api-secret: ${{ secrets.KAFKA_CLUSTER_API_SECRET }}
```
