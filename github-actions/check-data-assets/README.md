# Gable: Check Data Assets

Checks the data assets specified in the command against their contracts in Gable.

This action is a thin wrapper around the `gable` [Python CLI](https://pypi.org/project/gable/)

## Inputs

| parameter | description | required | default |
| --- | --- | --- | --- |
| gable-api-endpoint | Gable API Endpoint in the format `https://api.<organization>.gabledata.com` | `true` |  |
| gable-api-key | Gable API Key | `true` |  |
| gable-version | Gable Version | `false` | latest |
| allow-gable-pre-release | Whether or not to install pre-release versions of Gable | `false` | false |
| data-asset-options | Options passed in to the 'gable data-asset check' command  | `true` |  |
| github-token | Github token used to comment on PR | `false` | ${{ github.token }} |

## CLI Help (for version 0.6.0)

```bash
Usage: gable data-asset check [OPTIONS]

  Checks data asset(s) against a contract

Options:
  --source-type [postgres|mysql|avro|protobuf|json_schema]
                                  The type of data asset.
                                  
                                  For databases (postgres, mysql) the check
                                  will be performed for all tables within the
                                  database.
                                  
                                  For protobuf/avro/json_schema the check will
                                  be performed for all file(s)  [required]
  -o, --output [text|json|markdown]
                                  Format of the output. Options are: text
                                  (default), json, or markdown which is
                                  intended to be used as a PR comment
  Database Options:               Options for checking contract compliance for
                                  tables in a relational database. The check
                                  will be performed for any tables that have a
                                  contract associated with them.
                                  
                                  Gable relies on having a proxy database that
                                  mirrors your production database, and will
                                  connect to the proxy database to perform the
                                  check in order to perform the check as part
                                  of the CI/CD process before potential
                                  changes in the PR are merged.
    -h, --host TEXT               The host name of the production database,
                                  for example 'service-one.xxxxxxxxxxxx.us-
                                  east-1.rds.amazonaws.com'. Despite not
                                  needing to connect to the production
                                  database, the host is still needed to
                                  generate the unique resource  name for the
                                  real database tables (data assets).
    -p, --port INTEGER            The port of the production database. Despite
                                  not needing to connect to the production
                                  database, the port is  still needed to
                                  generate the unique resource name for the
                                  real database tables (data assets).
    --db TEXT                     The name of the production database. Despite
                                  not needing to connect to the production
                                  database, the database  name is still needed
                                  to generate the unique resource name for the
                                  real database tables (data assets).
                                  
                                  Database naming convention frequently
                                  includes the environment
                                  (production/development/test/staging) in the
                                  database name, so this value may not match
                                  the name of the database in the proxy
                                  database instance. If this is  the case, you
                                  can set the --proxy-db value to the name of
                                  the database in the proxy instance, but
                                  we'll use the  value of --db to generate the
                                  unique resource name for the data asset.
                                  
                                  For example, if your production database is
                                  'prod_service_one', but your test database
                                  is 'test_service_one',  you would set --db
                                  to 'prod_service_one' and --proxy-db to
                                  'test_service_one'.
    -s, --schema TEXT             The schema of the production database
                                  containing the table(s) to check. Despite
                                  not needing to connect to  the production
                                  database, the schema is still needed to
                                  generate the unique resource name for the
                                  real database tables (data assets).
                                  
                                  Database naming convention frequently
                                  includes the environment
                                  (production/development/test/staging) in the
                                  schema name, so this value may not match the
                                  name of the schema in the proxy database
                                  instance. If this is  the case, you can set
                                  the --proxy-schema value to the name of the
                                  schema in the proxy instance, but we'll use
                                  the  value of --schema to generate the
                                  unique resource name for the data asset.
                                  
                                  For example, if your production schema is
                                  'production', but your test database is
                                  'test',  you would set --schema to
                                  'production' and --proxy-schema to 'test'.
    -t, --table, --tables TEXT    A comma delimited list of the table(s) to
                                  check. If no table(s) are specified, all
                                  tables within the provided schema will be
                                  checked.
                                  
                                  Table names in the proxy database instance
                                  must match the table names in the production
                                  database instance, even if the database or
                                  schema names are different.
    -ph, --proxy-host TEXT        The host string of the database instance
                                  that serves as the proxy for the production
                                  database. This is the  database that Gable
                                  will connect to when checking tables in the
                                  CI/CD workflow.
    -pp, --proxy-port INTEGER     The port of the database instance that
                                  serves as the proxy for the production
                                  database. This is the  database that Gable
                                  will connect to when checking tables in the
                                  CI/CD workflow.
    -pdb, --proxy-db TEXT         Only needed if the name of the database in
                                  the proxy instance is different than the
                                  name of the production database. If not
                                  specified, the value of --db will be used to
                                  generate the unique resource name for  the
                                  data asset.
                                  
                                  For example, if your production database is
                                  'prod_service_one', but your test database
                                  is 'test_service_one',  you would set --db
                                  to 'prod_service_one' and --proxy-db to
                                  'test_service_one'.
    -ps, --proxy-schema TEXT      Only needed if the name of the schema in the
                                  proxy instance is different than the name of
                                  the schema in the production database. If
                                  not specified, the value of --schema will be
                                  used to generate the unique resource name
                                  for  the data asset.
                                  
                                  For example, if your production schema is
                                  'production', but your test database is
                                  'test', you would set --schema to
                                  'production' and --proxy-schema to 'test'.
    -pu, --proxy-user TEXT        The user that will be used to connect to the
                                  proxy database instance that serves as the
                                  proxy for the production  database. This is
                                  the database that Gable will connect to when
                                  checking tables in the CI/CD workflow.
    -ppw, --proxy-password TEXT   If specified, the password that will be used
                                  to connect to the proxy database instance
                                  that serves as the proxy for  the production
                                  database. This is the database that Gable
                                  will connect to when checking tables in the
                                  CI/CD workflow.
  Protobuf & Avro options:        Options for checking Protobuf message(s),
                                  Avro record(s), or JSON schema object(s)for
                                  contract violations.
    --files TUPLE                 Space delimited path(s) to the contracts to
                                  check, with support for glob patterns.
  Global Options: 
    --endpoint TEXT               Customer API endpoint for Gable, in the
                                  format https://api.company.gable.ai/. Can
                                  also be set with the GABLE_API_ENDPOINT
                                  environment variable.  [required]
    --api-key TEXT                API Key for Gable. Can also be set with the
                                  GABLE_API_KEY environment variable.
                                  [required]
    --debug                       Enable debug logging
    --trace                       Enable trace logging. This is the most
                                  verbose logging level and should only be
                                  used for debugging.
  --help                          Show this message and exit.

  Example:

  gable data-asset check --source-type protobuf --files ./**/*.proto
```
