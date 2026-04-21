# pys3

`pys3` is a lightweight, interactive S3 client written entirely in standard Python 3. 
It requires **no external dependencies** (no `boto3`, no `requests`) and is designed to work with Ceph RadosGW, and OpenShift Object Bucket Claims (OBC).

It features an `ncftp`-like interactive shell with tab completion, filesystem-like navigation (`cd`, `ls`), bandwidth tracking, and the ability to execute commands directly from the terminal.

## Features
* **Zero Dependencies:** Runs on any standard Python 3 environment.
* **Interactive Shell:** Tab-completion for remote S3 objects and local files.
* **Filesystem Simulation:** Use `cd`, `mkdir`, `ls -lh`, and `find` just like a local terminal.
* **OpenShift Integration:** Automatically extract the credentials and endpoints directly from an OpenShift OBC. Still you need to update the "route".
* **Ceph RadosGW Compatible:** Fully supports standard AWS Signature V4.

## Installation

Simply download the script and make it executable:

```bash
chmod +x pys3
mv pys3 /usr/local/bin/pys3
