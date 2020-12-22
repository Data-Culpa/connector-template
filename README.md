# connector-template
Template for Data Culpa connector pipelines

This is currently just a README of the CLI interfaces for Data Culpa-provided pipeline connectors so we can try to standardize how things work across different connectors.


## CLI Parameters

```--init <filename>``` to create a blank YAML to fill in.

```--discover <filename>``` -- discover the objects in the database we could monitor and return a JSON structure to stdout for use in the Data Culpa UI.  Consider a ```--discover <filename> --yamlupdate```

```-e <env file>``` -- to override the .env default; useful in a multi-instance scenario. Default is to look for a .env file the current working directory. If no file is found, print out the absolute path of what wasn't found so folks know where to look.

```--test <filename>``` -- test connectivity, e.g., logging into a database.  If objects are configured, attempt to do a read of one record. The goal of this is to ensure we can connect and have permissions to read the desired objects. If no objects are configured, see if we can discover anything.


## Data Source Configuration

This lives in a YAML file; one YAML per connector instance.


## Secret Configuration

Secrets come in a .env file, narrowly scoped to just secrets; and of course can be inherited in the environment from a secrets wrapper.

## Implementation Architecture

- Kick off with cron or other orchestrator
- the parent orchestrator can subdivide workloads among hosts.
- connectors are read only and can work on replicas of data
- connectors should be implemented with the expectation that multiple running instances on the same host are allowed
- Instances may want to keep state, e.g., a cache of visited objects and related metadata; the YAML file should specify a unique working directory path to keep state information if the plan is to keep it in the local filesystem.

## Future Improvements

This framework is a guide for the many connectors we will provide for data-at-rest monitoring with Data Culpa. Clearly numerous changes are possible; you can get in touch with us by writing to hello@dataculpa.com or opening issues in this repository.
