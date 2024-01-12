# Github Actions

Gable publishes several Github Actions to make it easy to integrate the Gable platform in your Github Actions workflows.
These Actions are thin interfaces to expose [Gable's Command Line Interface](https://pypi.org/project/gable/) and make sure
the right exit codes are returned based on your configuration in Gable.

* [Check Data Assets](check-data-assets/): Check if data assets are in compliance with existing data contracts.
* [Publish Contracts](publish-contracts/): Publish data contracts to the Gable platform.
* [Register data assets](register-data-assets/): Register and update data assets with the Gable platform.
* [Validate Contracts](validate-contracts/): Validate data contracts are well formed.

## Usage

There are a few inputs that every Action requires.

* `gable-api-endpoint`: The URL of the Gable API endpoint. You can find instructions on finding this at `/docs/api_keys` in your company's Gable application.
* `gable-api-key`: The API key to use to authenticate with the Gable API. You can find instructions on finding this at `/docs/api_keys` in your company's Gable application.
