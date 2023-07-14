# publish-contracts

Publishes contracts to Gable.

This action is a thin wrapper around the `gable` [Python CLI](https://pypi.org/project/gable/)

```
Usage: gable contract publish [OPTIONS] [CONTRACT_FILES]...

  Publishes data contracts to Gable

Options:
  --help  Show this message and exit.

  Examples:

  gable contract publish contract1.yaml

  gable contract publish **/*.yaml
```