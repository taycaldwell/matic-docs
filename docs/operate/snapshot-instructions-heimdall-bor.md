---
id: snapshot-instructions-heimdall-bor
title: Heimdall and Bor Snapshots
sidebar_label: Heimdall & Bor Snapshots
description: Heimdall and Bor snapshot Instructions for faster syncing.
keywords:
  - docs
  - matic
  - polygon
  - binary
  - node
  - validator
  - sentry
  - heimdall
  - bor
  - snapshots
image: https://wiki.polygon.technology/img/polygon-wiki.png
---

import useBaseUrl from '@docusaurus/useBaseUrl';

When setting up a new sentry, validator, or full node server, it is recommended that you use snapshots for faster syncing without having to sync over the network. Using snapshots will save you several days for both Heimdall and Bor.

:::tip

For the latest snapshot, please visit [<ins>Polygon Chains Snapshots</ins>](https://snapshot.polygon.technology/).

:::

## Heimdall/Bor Snapshots

First, you need to set up your node with **prerequisites** as per the node setup guide. Before you start any services, follow the steps below to download and extract your snapshot data for faster bootstrapping. In our example, we're using an Ubuntu Linux m5d.4xlarge machine with an 8TB Block device attached:

```
// install decompression and pipe viewer dependencies
sudo apt-get update -y
sudo apt-get install -y zstd pv aria2

// use screen session to prevent accidental download halts
screen -S posv1_bootstrap

// replace <network> with either mainnet/mumbai depending on your use case
network=`echo <network>`

// download compiled incremental snapshot files list
aria2c -x6 -s6 https://snapshot-download.polygon.technology/heimdall-$network-incremental-compiled-files.txt
aria2c -x6 -s6 https://snapshot-download.polygon.technology/bor-$network-incremental-compiled-files.txt

// download all incremental files, includes automatic checksum verification per increment
aria2c -c -m 0 -x6 -s6 -i heimdall-$network-incremental-compiled-files.txt --max-concurrent-downloads=1
aria2c -c -m 0 -x6 -s6 -i bor-$network-incremental-compiled-files.txt --max-concurrent-downloads=1

function extract_files() {
    extract_dir=$1
    compiled_files=$2
    mkdir $extract_dir
    while read -r line; do
        if [[ "$line" == checksum* ]]; then
            continue
        fi
        filename=`echo $line | awk -F/ '{print $NF}'`
        if echo "$filename" | grep -q "bulk"; then
            pv $filename | tar -I zstd -xf - -C $extract_dir
        else
            pv $filename | tar -I zstd -xf - -C $extract_dir --strip-components=3
        fi
    done < $compiled_files
}

// extract files for Heimdall
extract_files heimdall_extract heimdall-$network-incremental-compiled-files.txt

// extract files for Bor
extract_files bor_extract bor-$network-incremental-compiled-files.txt

// you now have all chaindata extracted and are ready to start your node
// be sure to point your heimdall/bor config files to the datadir path of your extracted data 
// this ensures the systemd services can properly register the snapshot data on client start
// symlinks can also be used if needing to keep default client config settings
```
