# Gable: Validate Contracts

Syntax validation of Gable data contracts.

## Inputs

| parameter | description | required | default |
| --- | --- | --- | --- |
| gable-api-endpoint | Gable API Endpoint in the format `https://api.<organization>.gabledata.com` | `true` |  |
| gable-api-key | Gable API Key | `true` |  |
| gable-version | Gable Version | `false` | latest |
| allow-gable-pre-release | Whether or not to install pre-release versions of Gable | `false` | false |
| contract-paths | White space delimited path(s) to the contracts to validate, with support for glob patterns. Example:    `"service1/**/*.yml contracts/service2/example_contract.yml"`. This input may also be specified as a multiline string, with each line representing a path to contract(s) to publish. | `true` |  |
