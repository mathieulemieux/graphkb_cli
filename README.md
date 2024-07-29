# GraphKB CLI

### Install
Move to or make a new directory from where you can clone the GraphKB CLI repo.
```
cd ~/bin
git clone https://svn.bcgsc.ca/bitbucket/scm/dat/graphkb-cli.git gkb
cd gkb
```
Make gkb accessible in your path.
```
GKB_WORKINGDIR=$(pwd)
echo "export PATH=$GKB_WORKINGDIR:\$PATH" >> ~/.bashrc
source ~/.bashrc
gkb --version
```

### Dependency to jq
Since the CLI have a dependency to [jq](https://jqlang.github.io/jq/), make sure this program is also accessible in your path.
```
jq --version
```
With sudo access you can install jq globally:
```
sudo apt-get install jq
jq --version
```
or you can install it locally beside gkb with the following:
```
cd ~/bin
wget -q https://github.com/jqlang/jq/releases/download/jq-1.7.1/jq-linux-amd64 -O jq
chmod +x jq

JQ_PATH=$(pwd)
echo "export PATH=$JQ_PATH:\$PATH" >> ~/.bashrc
source ~/.bashrc
jq --version
```

### Config
Available APIs and default API can be configured in [./config](./config) file.

### Environment Variables

Authentification
```
# Default username, with fallback to $(whoami) if not present
# Can be overridden with --username
export GKB_USER=<username>

# Default password.
# Can be overridden with --password
export GKB_PASSWORD=<password>
```

### Usage
Each command correspond to a specific API endpoint as described in [GraphKB API's Swagger/OpenAPI specifications](https://pori-demo.bcgsc.ca/graphkb-api/api/spec)
If any, general options MUST be placed BEFORE the invoked command.
If any, command-specific options MUST be placed AFTER the command and BEFORE any positional arguments.
```
gkb [...GENERAL_OPTIONS] <COMMAND> [...COMMAND_OPTIONS] [...COMMAND_POS_ARGS]
```

Help and examples
```
# General help, with a list of available commands.
gkb --help
gkb --examples

# Command-specific help
gkb <COMMAND> --help
gkb <COMMAND> --examples
```
### Examples
Here's some specific wworking examples for each command

###### No Auth Needed
```
# Metadata
gkb version
gkb spec > spec.json
gkb schema | jq '.schema | keys'

# Variant notation parser
gkb parse 'EGFR:c.2573T>G'
```

###### Auth Needed
Token (JWT) is automatically refreshed before every request using the API's token endpoint.
```
# Querying database
gkb query User name graphkb_importer
gkb query --filter name CONTAINSTEXT importer User

# CRUD operations
gkb GET User 29:0
gkb DELETE Disease 133:0
gkb PATCH --body '{"comment":"test"}' Statement 133:0
gkb POST --body '{"in":"#160:266","out":"#159:1169"}' Infers
gkb POST --file ./examples/POST_PositionalVariant.json PositionalVariant

# Querying external APIs through GraphKB API
gkb extensions clinicaltrialsgov NCT03478891
gkb extensions gene 1956
gkb extensions pubmed 28244479

# Statistics
gkb stats Disease,Therapy
gkb stats --groupBy relevance Statement | jq --sort-keys .result.Statement
```

### Contact
Contribute, report bugs or suggest new features by sending an email to mlemieux@bcgsc.ca.
