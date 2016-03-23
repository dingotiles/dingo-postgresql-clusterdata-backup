# Dingo::Postgresql::Clusterdata::Backup

This project includes `dingo-postgresql-clusterdata-backup` executable which can upload and download the current cluster data for a Dingo PostgreSQL cluster to a backup location.

The backup location can be any blob store/object store/bucket supported by the https://fog.io library.

The intent of this project is to avoid coupling Dingo PostgreSQL to AWS-specific tooling.

## Usage

To backup a cluster's data, pass the data to the executable via `STDIN`:

```
echo '{"instance_id": "cloudfoundry_instance_id", ...}' | bundle exec dingo-postgresql-clusterdata-backup backup
```

To get a list of backed up instance_ids, run:

```
bundle exec dingo-postgresql-clusterdata-backup list-service-ids
```

To restore a cluster's data from its backup location pass the instance ID via `STDIN`:

```
echo "cloudfoundry_instance_id" | bundle exec dingo-postgresql-clusterdata-backup restore
```

The `STDOUT` will solely contain the original cluster data:

```json
{"instance_id": "cloudfoundry_instance_id", }
```

To configure `dingo-postgresql-clusterdata-backup` subcommands `backup` and `restore` provide a configuration file via the `FOG_RC` environment variables. The top-level key `default` can be overridden with the `FOG_CREDENTIAL` environment variable.

For Amazon S3, the config file will look like:

```yaml
default:
  provider: AWS
  aws_access_key_id: AWS_KEY
  aws_secret_access_key: AWS_SECRET
  region: us-east-1
  bucket_name: our-dingo-postgresql-clusterdata-backups
```

The `list-service-ids` and `restore` commands can be combined to restore all backed up JSON for all instance IDs:

```
export FOG_RC=path/to/fog.yml
bundle exec exe/dingo-postgresql-clusterdata-backup list-service-ids | bundle exec exe/dingo-postgresql-clusterdata-backup restore
```

Will return one JSON per STDOUT, and errors on STDERR:

```json
{"instance_id":"TESTID-1458682941","service_id":"beb5973c-e1b2-11e5-a736-c7c0b526363d","plan_id":"b96d0936-e423-11e5-accb-93d374e93368","organization_guid":"","space_guid":"","parameters":null,"node_count":1,"node_size":20,"allocated_port":"33000"}
{"instance_id":"TESTID-1458684292","service_id":"beb5973c-e1b2-11e5-a736-c7c0b526363d","plan_id":"1545e30e-6dc3-11e5-826a-6c4008a663f0","organization_guid":"","space_guid":"","parameters":null,"node_count":1,"node_size":20,"allocated_port":"33007"}
{"instance_id":"TESTID-1458685135","service_id":"beb5973c-e1b2-11e5-a736-c7c0b526363d","plan_id":"b96d0936-e423-11e5-accb-93d374e93368","organization_guid":"","space_guid":"","parameters":null,"node_count":1,"node_size":20,"allocated_port":"33000"}
```

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/dingotiles/dingo-postgresql-clusterdata-backup. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

## Copyright

Copyright (c) 2016 WorldProtect Investments Limited
