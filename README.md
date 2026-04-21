# pys3

pys3 is a lightweight, interactive S3 client written entirely in standard Python 3. 
It requires no external dependencies (no boto3, no requests).
It is designed to work flawlessly with Ceph RadosGW, and OpenShift Object Bucket Claims (OBC).

It features an ncftp-like interactive shell with tab completion, filesystem-like navigation (cd, ls), bandwidth tracking.

## Features

  * **Zero Dependencies:** Runs on any standard Python 3 environment.
  * **Interactive Shell:** Tab-completion for remote S3 objects and local files.
  * **Filesystem Simulation:** Use cd, mkdir, ls -lh, and find just like a local terminal.
  * **OpenShift Integration:** Automatically extract credentials and endpoints directly from an OpenShift OBC.
  * **Ceph RadosGW Compatible:** Fully supports standard AWS Signature V4.

## Installation

Simply download the script and make it executable from your terminal:

```bash
$ chmod +x pys3
$ mv pys3 /usr/local/bin/pys3
```

## Configuration

pys3 uses an INI-style configuration file located at ~/.pys3 to manage multiple bucket profiles.

To generate a default template, run the following command:

```bash
$ pys3 --generate-config > ~/.pys3
```


### Configuration File Format (\~/.pys3)

You can define multiple environments by creating new profile blocks inside the file. Here is an example of what your file should look like:

```bash
[default]
endpoint = https://s3.amazonaws.com
region = us-east-1
access_key = YOUR_ACCESS_KEY
secret_key = YOUR_SECRET_KEY
bucket = your-bucket-name

[radosgw]
endpoint = http://127.0.0.1:8080
region = default
access_key = 123123
secret_key = 456456
bucket = mybucket
```

### OpenShift OBC Integration

If you are logged into an OpenShift cluster via the oc CLI, pys3 can automatically read the Secret and ConfigMap of an Object Bucket Claim and generate the configuration block for you. Just run:

```bash
$ pys3 --from-obc my-bucket-claim >> ~/.pys3
```

And check that you have the profile name you'd like to use.


## Usage

### Interactive Shell Mode

Launch pys3 with a specific profile to drop into the interactive shell:

$ pys3 --profile radosgw

**Available shell commands:**

  * ls [-l, -lh] : List objects in the current prefix (directory).
  * find : Recursively list all objects under the current prefix.
  * cd \<dir\> : Navigate into a prefix boundary (or .. to go up).
  * mkdir \<dir\> : Create a new prefix/directory.
  * get \<remote\> [local] : Download an object.
  * put \<local\> [remote] : Upload a file.
  * cat \<remote\> : Print the contents of a text file to the terminal.
  * exit : Close the shell.

Note: You can use the TAB key at any time to auto-complete remote object names or local file paths.

### CLI / Standalone Mode

You can also pass commands directly to pys3 without entering the interactive shell. This is highly useful for automated bash scripts:

```bash
$ pys3 --profile radosgw ls -lh
$ pys3 --profile radosgw mkdir backups/
$ pys3 --profile radosgw put local-archive.tar.gz backups/archive.tar.gz
$ pys3 --profile radosgw get backups/archive.tar.gz
```
