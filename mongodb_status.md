> MONGODB_STATUS    (/home/runner/work/community.mongodb/community.mongodb/plugins/modules/mongodb_status.py)

        Validates the status of the cluster. The module expects all
        replicaset nodes to be PRIMARY, SECONDARY or ARBITER. Will
        wait until a timeout for the replicaset state to converge if
        required.

  * This module is maintained by The Ansible Community
OPTIONS (= is mandatory):

- interval
        The number of seconds to wait between poll executions.
        [Default: 30]
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
        [Default: (null)]
        type: str

- login_port
        The MongoDB port to login to.
        [Default: 27017]
        type: int

- login_user
        The username to authenticate with.
        [Default: (null)]
        type: str

- poll
        The maximum number of times query for the replicaset status.
        [Default: 1]
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

- name: Check replicaset is healthy, fail if not after first attempt
  mongodb_status:
    replicaset: rs0
  when: ansible_hostname == "mongodb1"

- name: Wait for the replicaset rs0 to converge, check 5 times, 10 second interval between checks
  mongodb_status:
    replicaset: rs0
    poll: 5
    interval: 10
  when: ansible_hostname == "mongodb1"


RETURN VALUES:

failed:
  description: If the mnodule had failed or not.
  returned: always
  type: bool
iteration:
  description: Number of times the module has queried the replicaset status.
  returned: always
  type: int
msg:
  description: Status message.
  returned: always
  type: str
replicaset:
  description: The last queried status of all the members of the replicaset if obtainable.
  returned: always
  type: dict

