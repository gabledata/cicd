# validate-contracts

Syntax validation of Gable data contracts.

This action is a thin wrapper around the `gable` [Python CLI](https://pypi.org/project/gable/)

```
Usage: gable contract validate [OPTIONS] [CONTRACT_FILES]...

  Validates the syntax of the data contract files

Options:
  --help  Show this message and exit.

  Examples:

  gable contract validate contract1.yaml

  gable contract validate **/*.yaml
```