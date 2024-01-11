# Gable: Register Data Assets

Checks data assets defined in a product repository against any contracts defined in Gable.

## Inputs

| parameter | description | required | default |
| --- | --- | --- | --- |
| gable-api-endpoint | Gable API Endpoint in the format https://api.<organization>.gabledata.com | `true` |  |
| gable-api-key | Gable API Key | `true` |  |
| gable-version | Gable Version | `false` | latest |
| allow-gable-pre-release | Whether or not to install pre-release versions of Gable | `false` | false |
| data-asset-options | Options passed in to the 'gable data-asset register' command  | `true` |  |
| github-token | Github token used to comment on PR | `false` | ${{ github.token }} |

## CLI Help (for version 0.6.0)

```bash
Usage: gable data-asset register [OPTIONS]

  Registers a data asset with Gable

Options:
  --source-type [postgres|mysql|avro|protobuf|json_schema]
                                  The type of data asset.
                                  
                                  For databases (postgres, mysql) a data asset
                                  is a table within the database.
                                  
                                  For protobuf/avro/json_schema a data asset
                                  is message/record/schema within a file.
  Database Options:               Options for registering database tables as
                                  data assets. Gable relies on having a proxy
                                  database that mirrors your  production
                                  database, and will connect to the proxy
                                  database to register tables as data assets.
                                  This is to ensure that  Gable does not have
                                  access to your production data, or impact
                                  the performance of your production database
                                  in any way.  The proxy database can be a
                                  local Docker container, a Docker container
                                  that is spun up in your CI/CD workflow, or a
                                  database instance in a test/staging
                                  environment. The tables in the proxy
                                  database must have the same schema as your
                                  production database for all tables to be
                                  correctly registered. The proxy database
                                  must be accessible from the  machine that
                                  you are running the gable CLI from.
                                  
                                  If you're registering tables in your CI/CD
                                  workflow, it's important to only register
                                  from the main branch, otherwise  you may end
                                  up registering tables that do not end up in
                                  production.
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
                                  containing the table(s) to register. Despite
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
                                  register. If no table(s) are specified, all
                                  tables within the provided schema will be
                                  registered.
                                  
                                  Table names in the proxy database instance
                                  must match the table names in the production
                                  database instance, even if the database or
                                  schema names are different.
    -ph, --proxy-host TEXT        The host string of the database instance
                                  that serves as the proxy for the production
                                  database. This is the  database that Gable
                                  will connect to when registering tables in
                                  the CI/CD workflow.
    -pp, --proxy-port INTEGER     The port of the database instance that
                                  serves as the proxy for the production
                                  database. This is the  database that Gable
                                  will connect to when registering tables in
                                  the CI/CD workflow.
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
                                  registering tables in the CI/CD workflow.
    -ppw, --proxy-password TEXT   If specified, the password that will be used
                                  to connect to the proxy database instance
                                  that serves as the proxy for  the production
                                  database. This is the database that Gable
                                  will connect to when registering tables in
                                  the CI/CD workflow.
  Protobuf & Avro options:        Options for registering a Protobuf message,
                                  Avro record, or JSON schema object as data
                                  assets. These objects  represent data your
                                  production services produce, regardless of
                                  the transport mechanism.
                                  
                                  If you're registering Protobuf messages,
                                  Avro records, or JSON schema objects in your
                                  CI/CD workflow, it's important  to only
                                  register from the main branch, otherwise you
                                  may end up registering records that do not
                                  end up in production.
    --files TUPLE                 Space delimited path(s) to the contracts to
                                  register, with support for glob patterns.
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

  gable data-asset register --source-type postgres \     --host
  prod.pg.db.host --port 5432 --db transit --schema public --table routes \
  --proxy-host localhost --proxy-port 5432 --proxy-user root --proxy-password
  password
```
