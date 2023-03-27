# Bioconductor Archive

## Background

As of 2023, the main Bioconductor archive storage has been moved to an [Open Storage Network](https://www.openstoragenetwork.org/) s3-compatible [bucket](https://mghp.osn.xsede.org/bir190004-bucket01/index.html#archive.bioconductor.org/packages/).

## How to sync from the OSN archive

The instructions below can be used to copy the archive from the remote location to your local machine. While these instructions are using the AWS CLI for its S3 client, this can be replaced with any s3 client. For those familiar with s3, the following bucket information might be all you need:

| Endpoint  | https://mghp.osn.xsede.org                                |
|-----------|-----------------------------------------------------------|
| Full Path | bir190004-bucket01/archive.bioconductor.org/packages/3.xx |


We offer two ways of using the AWS CLI. If you have a docker-enabled machine, we recommend you use the AWS docker container rather than go through the installation. Otherwise, [follow the installation instructions for the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

### Using Docker

Below is a shell script showcasing how one can sync a singular version from the archive to the local storage. This variation requires you have Docker installed and running. If you do not, you may [follow the installation instructions from Docker](https://docs.docker.com/engine/install/). 

```
#!/bin/bash

ARCHIVE_VER=3.14
DEST_DIR=/tmp/my/location

mkdir -p $DEST_DIR

docker run -v $DEST_DIR:/mnt/biocdest --rm amazon/aws-cli:2.11.6 s3 sync --no-sign-request --delete --endpoint-url https://mghp.osn.xsede.org s3://bir190004-bucket01/archive.bioconductor.org/packages/$ARCHIVE_VER /mnt/biocdest/$ARCHIVE_VER

```

### Using local AWS CLI

Similarly, using the local AWS CLI, the `s3 sync` command can be used to sync an archived version to your local machine. This variation requires you to have the AWS CLI installed. If you don't, you may [follow the installation instructions from Amazon](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

```
#!/bin/bash

ARCHIVE_VER=3.14
DEST_DIR=/tmp/my/location

mkdir -p $DEST_DIR

aws s3 sync --no-sign-request --delete --endpoint-url https://mghp.osn.xsede.org s3://bir190004-bucket01/archive.bioconductor.org/packages/$ARCHIVE_VER $DEST_DIR/$ARCHIVE_VER

```
