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
| contract-paths | Space delimited path(s) to the contracts to publish, with support for glob patterns. Example:    "service1/**/*.yml contracts/service2/example_contract.yml" This input may also be specified as a multiline string, with each line representing a path to contract(s) to publish. Example:   contract-paths: |     service1/**/*.yml     service2/example_contract.yml  | `true` |  |

## CLI Help (for version 0.6.0)

```bash
Usage: gable contract publish [OPTIONS] [CONTRACT_FILES]...

  Publishes data contracts to Gable

Options:
  Global Options: 
    --endpoint TEXT  Customer API endpoint for Gable, in the format
                     https://api.company.gable.ai/. Can also be set with the
                     GABLE_API_ENDPOINT environment variable.  [required]
    --api-key TEXT   API Key for Gable. Can also be set with the GABLE_API_KEY
                     environment variable.  [required]
    --debug          Enable debug logging
    --trace          Enable trace logging. This is the most verbose logging
                     level and should only be used for debugging.
  --help             Show this message and exit.

  Examples:

  gable contract publish contract1.yaml

  gable contract publish **/*.yaml
```
