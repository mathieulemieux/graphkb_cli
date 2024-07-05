# GraphKB CLI

### Install
Move to or make a new directory from where you can clone the GraphKB CLI repo.
```
git clone ./...
cd gkb
GKB_WORKINGDIR=$(pwd)
```
Make gkb accessible in your path.
```
echo "export PATH=$GKB_WORKINGDIR:\$PATH" >> ~/.bashrc
source ~/.bashrc
gkb --version
```

### jq
Since many of the CLI functionalities have a dependency to jq, you may want to make sure this program is also accessible in your path.
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
mkdir ../jq && cd ../jq
wget -q https://github.com/jqlang/jq/releases/download/jq-1.7.1/jq-linux-amd64
chmod +x jq
JQ_PATH=$(pwd)

echo "export PATH=$JQ_PATH:\$PATH" >> ~/.bashrc
source ~/.bashrc
jq --version
```

### Contact
Contribute, report bugs or inquiry for new features by sending an email to mlemieux@bcgsc.ca
