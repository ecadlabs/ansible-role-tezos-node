Role Name
=========

Runs a Tezos RPC node using docker

Requirements
------------

Docker

Role Variables
--------------


Available variables are listed below, along with default values (see `defaults/main.yml`):

The Tezos network you wish to provision. No Default. Typically `carthagenet` or
`mainnet`

    network:

The location where on the host where the Tezos nodes data directory will
reside 

    node_data_dir: "/srv/tezos/{{ network }}_node"

The location on the host where the Tezos client configuration will reside. This
directory contains client configuration and keys used by the `tezos-client`
command.

    client_data_dir: "/srv/tezos/{{ network }}_client"

The tezos docker image to use.

    tezos_docker_image: tezos/tezos:v7.0-rc1

The history mode you wish to operate your node in. Options are full, archive or
snapshot (currently only tested using `full`)

    history_mode: full

Bootstrap strategy controls how your node will bootstrap. `genesis` will
bootstrap without a snapshot. Specify `snapshot` to have the role download and
import a snapshot.

    bootstrap_strategy: genesis

The path or URL to the snapshot file that will be used for initial import of
your node. The snapshot will be stored to the host and mounted via a volume
into a short lived docker image responsible for the import process.

        snapshot_tmp_file: /tmp/snapshot

The block hash of the import file. 

    snapshot_block_hash: BLTU1znZ4H1Mz3mMWNhqrsMSehosYL31nqrrrihxD7FRjkEGLPT


Dependencies
------------

None (but make sure you have docker installed, `geerlingguy.docker` works well)

Example Playbook
----------------

    - hosts: servers
      roles:
        - role: ecadlabs.tezos_node
          bootstrap_strategy: snapshot
          snapshot_url: "https://storage.googleapis.com/tezos-snapshots/snapshot_cart.full"
          network: carthagenet
          snapshot_block_hash: BLTU1znZ4H1Mz3mMWNhqrsMSehosYL31nqrrrihxD7FRjkEGLPT
          snapshot_tmp_file: /tmp/snapshot

License
-------

MIT

Author Information
------------------

Created by the humans from ECAD Labs Inc. https://ecadlabs.com
