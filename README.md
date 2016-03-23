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
bundle exec dingo-postgresql-clusterdata-backup restore list-service-ids
```

To restore a cluster's data from its backup location:

```
bundle exec dingo-postgresql-clusterdata-backup restore cloudfoundry_instance_id
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

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/dingotiles/dingo-postgresql-clusterdata-backup. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
