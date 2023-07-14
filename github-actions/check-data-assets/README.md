# check-data-assets

Checks the data assets specified in the command against their contracts in Gable.

This action is a thin wrapper around the `gable` [Python CLI](https://pypi.org/project/gable/)

```
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
  Database Options:               Options for checking contract complaince for
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
                                  real data asset so we can look up any
                                  associated contracts.
    -p, --port INTEGER            The port of the production database. Despite
                                  not needing to connect to the production
                                  database, the port is  still needed to
                                  generate the unique resource name for the
                                  real data asset so we can look up any
                                  associated contracts.
    --db TEXT                     The name of the production database. Despite
                                  not needing to connect to the production
                                  database, the database  name is still needed
                                  to generate the unique resource name for the
                                  real data asset so we can look up any
                                  associated  contracts.
                                  
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
    -s, --schema TEXT             The schema of the production database where
                                  the table(s) you want to register are
                                  located. Despite not  needing to connect to
                                  the production database, the schema is still
                                  needed to generate the unique resource name
                                  for the real data asset so we can look up
                                  any associated contracts.'
                                  
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
    -t, --table TEXT              The table to check for contract violations.
                                  If no table is specified, all tables within
                                  the provided  schema will be checked.
                                  
                                  Table names in the proxy database instance
                                  must match the table names in the production
                                  database instance, even if the database or
                                  schema names are different.
    -ph, --proxy-host TEXT        The host string of the database instance
                                  that serves as the proxy for the production
                                  database. This is the  database that Gable
                                  will connect to when checking tables for
                                  contract violations in the CI/CD workflow.
    -pp, --proxy-port INTEGER     The port of the database instance that
                                  serves as the proxy for the production
                                  database. This is the  database that Gable
                                  will connect to when checking tables for
                                  contract violations in the CI/CD workflow.
    -pdb, --proxy-db TEXT         Only needed if the name of the database in
                                  the proxy instance is different than the
                                  name of the production database.
                                  
                                  If not specified, the value of --db will be
                                  used to generate the unique resource name
                                  for the data asset. For example, if your
                                  production database is 'prod_service_one',
                                  but your test database is
                                  'test_service_one', you would set --db to
                                  'prod_service_one' and --proxy-db to
                                  'test_service_one'.
    -ps, --proxy-schema TEXT      Only needed if the name of the schema in the
                                  proxy instance is different than the name of
                                  the schema in the production database.
                                  
                                  If not specified, the value of --schema will
                                  be used to generate the unique resource name
                                  for the data asset. For example, if your
                                  production schema is 'production', but your
                                  test database is 'test', you would set
                                  --schema to 'production' and --proxy-schema
                                  to 'test'.
    -pu, --proxy-user TEXT        The user that will be used to connect to the
                                  proxy database instance that serves as the
                                  proxy for the production  database. This is
                                  the database that Gable will connect to when
                                  checking tables for contract violations in
                                  the CI/CD workflow.
    -ppw, --proxy-password TEXT   If specified, the password that will be used
                                  to connect to the proxy database instance
                                  that serves as the proxy for  the production
                                  database. This is the database that Gable
                                  will connect to when checking tables for
                                  contract violations in  the CI/CD workflow.
  Protobuf & Avro options: [all_or_none]
                                  Options for checking Protobuf message(s),
                                  Avro record(s), or JSON schema object(s)for
                                  contract violations.
    --files TUPLE
  --help                          Show this message and exit.

  Example:

  gable data-asset check --source-type protobuf --files ./**/*.proto
```