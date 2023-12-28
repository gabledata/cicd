# Gable Gitlab Integration

This document serves as a guide for integrating Gable's CI/CD functionality into your GitLab runner scripts. By following these instructions, you will be able to extend and customize various jobs within your CI/CD pipeline, enhancing your project's testing, publishing, and data asset management capabilities.

These jobs help to:
* Validate data contracts
* Check for violations of data assets
* Publish data contracts to Gable's platform
* Register new or updated data assets with Gable's platform

## Setting Up the Integration

The following are one-time steps that will need to be done in order to 

### Gable API Endpoint and API Key

In order to have the CI/CD jobs communicate with Gable's platform, you will need to get the following information:

1. **Gable API Endpoint**: Each organization is assigned a unique API endpoint for connecting your jobs to Gable's platform.

2. **Gable API Key**: An API key, corresponding to your organization's endpoint, is required for authentication and secure communication with Gable's platform.

To locate your organization's API Endpoint and API Key:

1. Navigate to the `Settings -> API Keys` section within the Gable platform.
2. Here, you will find the API Endpoint at the top of the page and a list of API keys associated with your organization.
3. Click on `View` next to the desired API key to reveal it.

### Setting up GitLab Access Token

In order to use the Gitlab integration, you will need to create a Gitlab access token.  This step will need to be done once at setup time for a repository


### Storing API Endpoint, API Key, and Access Token as Variables

In order to set up communication between your GitLab CI/CD jobs and the Gable platform, it is essential to define and store a set of variables within your GitLab configuration. These variables include:

1. **`GABLE_API_ENDPOINT`**: This variable holds the unique API endpoint associated with your organization on Gable's platform. It serves as the primary URL through which your GitLab jobs will interact with Gable.

2. **`GABLE_API_KEY`**: The `GABLE_API_KEY` is a crucial variable storing the API key necessary for authenticating your GitLab jobs with the Gable platform.

3. **`GABLE_MR_COMMENT_TOKEN`**: This variable stores the GitLab access token, a key component for operations that involve merge request (MR) commenting and other GitLab-specific interactions.

**NOTE**: Treat these variables, especially the `GABLE_API_KEY` and `GABLE_MR_COMMENT_TOKEN`, with high confidentiality. They should be stored securely and not exposed in logs or publicly accessible files.

## Incorporating 

### Including Hidden Jobs

To begin, you need to include the external job definitions from Gable's CI/CD repository. This is accomplished with the `include` keyword, which fetches the specified file and integrates its contents into your current CI/CD configuration.

```yaml
include:
  - remote: 'https://raw.githubusercontent.com/gabledata/cicd/main/gitlab/gable-ci-include.yml'
```

## Job Definitions

Below are detailed instructions for each job definition:

### 1. Validate Contracts

This job extends the `.validate-contracts` job defined in the `gable-ci-include.yml` file. It sets a variable `GABLE_CONTRACT_PATHS` to specify the path of your contract files.

```yaml
validate-contracts:
  extends: 
    - .validate-contracts
  stage: test
  variables:
    GABLE_CONTRACT_PATHS: ./contracts/*.yaml
```

### 2. Publish Contracts

Similar to validation, this extends the `.publish-contracts` job for publishing contracts, with the same path variable.

```yaml
publish-contracts:
  extends: 
    - .publish-contracts
  stage: test
  variables:
    GABLE_CONTRACT_PATHS: ./contracts/*.yaml
```

### 3. Register Data Assets

These jobs extend the `.register-data-assets` job for different types of data assets:

- **Protobuf**:

  ```yaml
  register-data-assets-protobuf:
    extends: 
      - .register-data-assets
    stage: test
    variables:
      GABLE_DATA_ASSET_REGISTER_OPTIONS: |
        --source-type protobuf 
        --files ./event_schemas/*.proto
  ```

- **Avro**:

  ```yaml
  register-data-assets-avro:
    extends: 
      - .register-data-assets
    stage: test
    variables:
      GABLE_DATA_ASSET_REGISTER_OPTIONS: |
        --source-type avro 
        --files ./event_schemas/*.avsc
  ```

- **Postgres**:

  Extends the `.go-test` job to run migrations and then register data assets for Postgres.

  ```yaml
  register-data-assets-postgres:
    extends: 
      - .register-data-assets
    services:
      - postgres:14
    stage: test
    variables:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: tutorial
      GABLE_DATA_ASSET_REGISTER_OPTIONS: |
          --source-type postgres 
          --host prod.store.com 
          --port 5432 
          --db tutorial 
          --schema public 
          --proxy-host postgres 
          --proxy-port 5432 
          --proxy-user postgres 
          --proxy-password postgres
  ```

### 4. Check Data Assets

These jobs extend the `.check-data-assets` job for different data formats and are designed to run only on merge requests:

- **Protobuf**:

  ```yaml
  check-data-assets-protobuf:
    extends: 
      - .check-data-assets
    stage: test
    variables:
        GABLE_DATA_ASSET_CHECK_OPTIONS: |
            --source-type protobuf 
            --files ./event_schemas/*.proto
    only:
      - merge_requests
  ```

- **Avro**:

  ```yaml
  check-data-assets-avro:
    extends: 
      - .check-data-assets
    stage: test
    variables:
        GABLE_DATA_ASSET_CHECK_OPTIONS: |
          --source-type avro 
          --files ./event_schemas/*.avsc
    only:
      - merge_requests
  ```

- **Postgres**:

  Extends the `.go-test` job for Postgres-specific checks on merge requests.

  ```yaml
  check-data-assets-postgres:
    extends: 
      - .check-data-assets
    services:
      - postgres:14
    stage: test
    variables:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: tutorial
      GABLE_DATA_ASSET_CHECK_OPTIONS: |
          --source-type postgres 
          --host prod.store.com 
          --port 5432 
          --db tutorial 
          --schema public 
          --proxy-host postgres 
          --proxy-port 5432 
          --proxy-user postgres 
          --proxy-password postgres
    only:
      - merge_requests
  ```

## Conclusion

By incorporating these job definitions into your GitLab CI/CD pipeline, you ensure a more streamlined and automated workflow, especially for testing and

