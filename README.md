Role Name
=========

This Ansible Role aims to make deploying a Tezos node fast and easy for Ansible users.

The role is heavily parameterized, allowing users to deploy nodes for different Tezos networks (mainnet/ithacanet/jakartanet/etc..) and various economic protocols to support block transitions.

Two bootstrap strategies are supported, namely syncing from genesis or importing a snapshot for fast bootstrapping.

The role has been tested against [Version 13 of the Tezos Node][tezos_v13].

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

The Tezos network you wish to provision. This variable does not have a default, so you must set it. Typically, values are `jakartanet` or `mainnet`. The `tezos_network` value is used for several purposes; naming of docker containers, naming of a docker network, selection of which Tezos network to use, and validating that snapshot imports are from the expected network

    tezos_network:

The location where on the host the Tezos nodes data directory will reside. This role uses Docker bind mounts over docker volumes.

    node_data_dir: "/srv/tezos/{{ tezos_network }}_node"

The location on the host where the Tezos client configuration will reside. This directory contains client configuration and keys used by the `tezos-client` command.

    client_data_dir: "/srv/tezos/{{ tezos_network }}_client"

The tezos docker image to use.

    octez_version: v13.0

The [history mode][history_mode] you wish to operate your node in. Options are archive, full, or rolling

    history_mode: full

Providing a snapshot url controls how your node will bootstrap. Specify a `snapshot_url` to have the role download and import a snapshot. As there are different snapshots for each history mode, this snapshot must have the same history mode as the node. If the value provided begins with `http://` or `https://`, the role will download a snapshot from that URL. If the provided value is a Unix file path such as `/var/tmp/a_tezos_snapshot` the role will copy the snapshot from the Ansible host machine to the target.

    snapshot_url: https://mainnet.xtz-shots.io/rolling # See https://xtz-shots.io/

The path or URL to the snapshot file that will be used for the initial import of your node. The snapshot will be downloaded to the target host's filesystem and mounted via a volume into a short-lived docker image responsible for the import process.

        snapshot_tmp_file: /tmp/snapshot

Dependencies
------------

None (but make sure you have docker installed, `geerlingguy.docker` works well)

Example Playbook
----------------

For mainnet:

    - hosts: servers
      roles:
        - role: ecadlabs.tezos_node
          snapshot_url: https://mainnet.xtz-shots.io/rolling # See https://xtz-shots.io/
          history_mode: rolling
          tezos_network: mainnet
          snapshot_tmp_file: /tmp/snapshot

License
-------

MIT

Author Information
------------------

Created by the humans from ECAD Labs Inc. https://ecadlabs.com

[tezos_v13]: https://tezos.gitlab.io/releases/version-13.html
[history_mode]: https://tezos.gitlab.io/user/history_modes.html
