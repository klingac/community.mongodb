> MONGODB_SHARD    (/home/runner/work/community.mongodb/community.mongodb/plugins/modules/mongodb_shard.py)

        Add or remove shards from a MongoDB Cluster.

  * This module is maintained by The Ansible Community
OPTIONS (= is mandatory):

- autosplit
        Disable or enable the autosplit flag in the config.settings
        collection.
        [Default: (null)]
        type: bool

- balancer_state
        Manage the Balancer for the Cluster
        (Choices: started, stopped, None)[Default: (null)]
        type: str

- login_database
        The database where login credentials are stored.
        [Default: admin]
        type: str

- login_host
        The host to login to.
        This must be a mongos.
        [Default: localhost]
        type: str

- login_password
        The login user's password used to authenticate with.
        [Default: (null)]
        type: str

- login_port
        The MongoDB port to login to.
        [Default: 27017]
        type: int

- login_user
        The MongoDB user to login with.
        [Default: (null)]
        type: str

= shard
        The shard connection string.
        Should be supplied in the form <replicaset>/host:port as
        detailed in https://docs.mongodb.com/manual/tutorial/add-
        shards-to-shard-cluster/.
        For example rs0/example1.mongodb.com:27017.

        type: str

- sharded_databases
        Enable sharding on the listed database.
        Can be supplied as a string or a list of strings.
        Sharding cannot be disabled on a database.
        [Default: (null)]
        type: raw

- ssl
        Whether to use an SSL connection when connecting to the
        database.
        [Default: False]
        type: bool

- ssl_cert_reqs
        Specifies whether a certificate is required from the other
        side of the connection, and whether it will be validated if
        provided.
        (Choices: CERT_NONE, CERT_OPTIONAL, CERT_REQUIRED)[Default:
        CERT_REQUIRED]
        type: str

- state
        Whether the shard should be present or absent from the
        Cluster.
        (Choices: absent, present)[Default: present]
        type: str


NOTES:
      * Requires the pymongo Python package on the remote host,
        version 2.4.2+.


REQUIREMENTS:  pymongo

AUTHOR: Rhys Campbell (@rhysmeister)
        METADATA:
          status:
          - preview
          supported_by: community
        

EXAMPLES:

- name: Add a replicaset shard named rs1 with a member running on port 27018 on mongodb0.example.net
  mongodb_shard:
    login_user: admin
    login_password: admin
    shard: "rs1/mongodb0.example.net:27018"
    state: present

- name: Add a standalone mongod shard running on port 27018 of mongodb0.example.net
  mongodb_shard:
    login_user: admin
    login_password: admin
    shard: "mongodb0.example.net:27018"
    state: present

- name: To remove a shard called 'rs1'
  mongodb_shard:
    login_user: admin
    login_password: admin
    shard: rs1
    state: absent

# Single node shard running on localhost
- name: Ensure shard rs0 exists
  mongodb_shard:
    login_user: admin
    login_password: secret
    shard: "rs0/localhost:3001"
    state: present

# Single node shard running on localhost
- name: Ensure shard rs1 exists
  mongodb_shard:
    login_user: admin
    login_password: secret
    shard: "rs1/localhost:3002"
    state: present

# Enable sharding on a few databases when creating the shard
- name: To remove a shard called 'rs1'
  mongodb_shard:
    login_user: admin
    login_password: admin
    shard: rs1
    sharded_databases:
      - db1
      - db2
    state: present


RETURN VALUES:

mongodb_shard:
    description: The name of the shard to create.
    returned: success
    type: str
sharded_enabled:
    description: Databases that have had sharding enabled during module execution.
    returned: success when sharding is enabled
    type: list

