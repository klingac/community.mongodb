> MONGODB_REPLICASET    (/home/runner/work/community.mongodb/community.mongodb/plugins/modules/mongodb_replicaset.py)

        Initialises a MongoDB replicaset in a new deployment.
        Validates the replicaset name for existing deployments.

  * This module is maintained by The Ansible Community
OPTIONS (= is mandatory):

- arbiter_at_index
        Identifies the position of the member in the array that is an
        arbiter.
        [Default: (null)]
        type: int

- chaining_allowed
        When `settings.chaining_allowed=true', the replicaset allows
        secondary members to replicate from other secondary members.
        When `settings.chaining_allowed=false', secondaries can
        replicate only from the primary.
        [Default: True]
        type: bool

- election_timeout_millis
        The time limit in milliseconds for detecting when a
        replicaset's primary is unreachable.
        [Default: 10000]
        type: int

- heartbeat_timeout_secs
        Number of seconds that the replicaset members wait for a
        successful heartbeat from each other.
        If a member does not respond in time, other members mark the
        delinquent member as inaccessible.
        The setting only applies when using `protocol_version=0'. When
        using `protocol_version=1' the relevant setting is
        `settings.election_timeout_millis'.
        [Default: 10]
        type: int

- login_database
        The database where login credentials are stored.
        [Default: admin]
        type: str

- login_host
        The MongoDB hostname.
        [Default: localhost]
        type: str

- login_password
        The password to authenticate with.
        If auth is not enabled do not supply this value.
        [Default: (null)]
        type: str

- login_port
        The MongoDB port to login to.
        [Default: 27017]
        type: int

- login_user
        The username to authenticate with.
        If auth is not enabled do not supply this value.
        [Default: (null)]
        type: str

- members
        Yaml list consisting of the replicaset members.
        Csv string will also be accepted i.e.
        mongodb1:27017,mongodb2:27017,mongodb3:27017.
        A dict can also be used to specify advanced replicaset member
        options.
        If a port number is not provided then 27017 is assumed.
        [Default: (null)]
        elements: raw
        type: list

- protocol_version
        Version of the replicaset election protocol.
        (Choices: 0, 1)[Default: 1]
        type: int

- replica_set
        Replicaset name.
        [Default: rs0]
        type: str

- ssl
        Whether to use an SSL connection when connecting to the
        database
        [Default: False]
        type: bool

- ssl_cert_reqs
        Specifies whether a certificate is required from the other
        side of the connection, and whether it will be validated if
        provided.
        (Choices: CERT_NONE, CERT_OPTIONAL, CERT_REQUIRED)[Default:
        CERT_REQUIRED]
        type: str

- validate
        Performs some basic validation on the provided replicaset
        config.
        [Default: True]
        type: bool


NOTES:
      * Requires the pymongo Python package on the remote host,
        version 2.4.2+. This can be installed using pip or the
        OS package manager. @see
        http://api.mongodb.org/python/current/installation.html


REQUIREMENTS:  pymongo

AUTHOR: Rhys Campbell (@rhysmeister)
        METADATA:
          status:
          - preview
          supported_by: community
        

EXAMPLES:

# Create a replicaset called 'rs0' with the 3 provided members
- name: Ensure replicaset rs0 exists
  mongodb_replicaset:
    login_host: localhost
    login_user: admin
    login_password: admin
    replica_set: rs0
    members:
    - mongodb1:27017
    - mongodb2:27017
    - mongodb3:27017
  when: groups.mongod.index(inventory_hostname) == 0

# Create two single-node replicasets on the localhost for testing
- name: Ensure replicaset rs0 exists
  mongodb_replicaset:
    login_host: localhost
    login_port: 3001
    login_user: admin
    login_password: secret
    login_database: admin
    replica_set: rs0
    members: localhost:3001
    validate: no

- name: Ensure replicaset rs1 exists
  mongodb_replicaset:
    login_host: localhost
    login_port: 3002
    login_user: admin
    login_password: secret
    login_database: admin
    replica_set: rs1
    members: localhost:3002
    validate: no

- name: Create a replicaset and use a custom priority for each member
  mongodb_replicaset:
    login_host: localhost
    login_user: admin
    login_password: admin
    replica_set: rs0
    members:
    - host: "localhost:3001"
      priority: 1
    - host: "localhost:3002"
      priority: 0.5
    - host: "localhost:3003"
      priority: 0.5
  when: groups.mongod.index(inventory_hostname) == 0

- name: Create replicaset rs1 with options and member tags
  mongodb_replicaset:
    login_host: localhost
    login_port: 3001
    login_database: admin
    replica_set: rs1
    members:
    - host: "localhost:3001"
      priority: 1
      tags:
        dc: "east"
        usage: "production"
    - host: "localhost:3002"
      priority: 1
      tags:
        dc: "east"
        usage: "production"
    - host: "localhost:3003"
      priority: 0
      hidden: true
      slaveDelay: 3600
      tags:
        dc: "west"
        usage: "reporting"


RETURN VALUES:

mongodb_replicaset:
  description: The name of the replicaset that has been created.
  returned: success
  type: str

