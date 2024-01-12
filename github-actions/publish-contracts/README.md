# Gable: Publish Contracts

Publishes new or updated contracts to Gable. Publishing a contract is idempotent - if no update has been made to the contract the
publish will be a no-op, so it's safe to publish all contracts on every run.

## Inputs

| parameter | description | required | default |
| --- | --- | --- | --- |
| gable-api-endpoint | Gable API Endpoint in the format `https://api.<organization>.gabledata.com` | `true` |  |
| gable-api-key | Gable API Key | `true` |  |
| gable-version | Gable Version | `false` | latest |
| allow-gable-pre-release | Whether or not to install pre-release versions of Gable | `false` | false |
| contract-paths | Space delimited path(s) to the contracts to publish, with support for glob patterns. Example:    `"service1/**/*.yml contracts/service2/example_contract.yml"`. This input may also be specified as a multiline string, with each line representing a path to contract(s) to publish. | `true` |  |
