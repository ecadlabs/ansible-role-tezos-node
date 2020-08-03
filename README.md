Role Name
=========

This Ansible Role aims to make deploying a Tezos node fast and easy for Ansible users.

The role is heavily parameterized, allowing users to deploy nodes for different Tezos networks (mainnet/carthagenet/labnet/etc..) and various economic protocols to support block transitions.

Two bootstrap strategies are supported, namely syncing from genesis or importing a snapshot for fast bootstrapping.

The role uses [Version 7 of the Tezos Node][tezos_v7], also known as the multinetwork node.

_This role does not manage any Tezos keys_

Requirements
------------

Docker (Tested on Debian Buster)

Installation
------------

    ansible-galaxy install ecadlabs.tezos_node

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

The Tezos network you wish to provision. This variable does not have a default, so you must set it. Typically, values are `carthagenet` or `mainnet`. The `tezos_network` value is used for several purposes; naming of docker containers, naming of a docker network, selection of which Tezos network to use, and validating that snapshot imports are from the expected network

    tezos_network:

The location where on the host the Tezos nodes data directory will reside. This role uses Docker bind mounts over docker volumes.

    node_data_dir: "/srv/tezos/{{ tezos_network }}_node"

The location on the host where the Tezos client configuration will reside. This directory contains client configuration and keys used by the `tezos-client` command.

    client_data_dir: "/srv/tezos/{{ tezos_network }}_client"

The tezos docker image to use.

    tezos_docker_image: tezos/tezos:v7.0-rc1

The history mode you wish to operate your node in. Options are full, archive or snapshot (currently only tested using `full`)

    history_mode: full

Bootstrap strategy controls how your node will bootstrap. `genesis` will bootstrap without a snapshot. Specify `snapshot` to have the role download and import a snapshot.

    bootstrap_strategy: genesis

The path or URL to the snapshot file that will be used for the initial import of your node. If the value provided begins with `http://` or `https://`, the role will download a snapshot from that URL. If the provided value is a Unix file path such as `/var/tmp/a_tezos_snapshot` the role will copy the snapshot from the Ansible host machine to the target.

    snapshot_url:

The path or URL to the snapshot file that will be used for the initial import of your node. The snapshot will be stored to the host and mounted via a volume into a short-lived docker image responsible for the import process.

        snapshot_tmp_file: /tmp/snapshot

The block hash of the _last_ block in the snapshot file. The import process uses this value to verify that the latest hash of the provided import file is the one you are expecting.

    snapshot_block_hash: BLHFBWdSiGL8AZVCM8PP3qXeD6HBuoZimfM5jWfRfL6n8FF6mK2

Dependencies
------------

None (but make sure you have docker installed, `geerlingguy.docker` works well)

Example Playbook
----------------

    - hosts: servers
      roles:
        - role: ecadlabs.tezos_node
          bootstrap_strategy: snapshot
          snapshot_url: snapshot_carthagenet_BLy7vs2msoekpJQs81GpfWkHsseTWDfp4HbVwHLfpAMzuL5YVtB.full
          tezos_network: carthagenet
          snapshot_block_hash: BLy7vs2msoekpJQs81GpfWkHsseTWDfp4HbVwHLfpAMzuL5YVtB
          snapshot_tmp_file: /tmp/snapshot

License
-------

MIT

Author Information
------------------

Created by the humans from ECAD Labs Inc. https://ecadlabs.com


[tezos_v7]: https://tezos.gitlab.io/releases/version-7.html
