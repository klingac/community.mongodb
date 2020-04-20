> MONGODB_PARAMETER    (/home/runner/work/community.mongodb/community.mongodb/plugins/modules/mongodb_parameter.py)

        Change an administrative parameter on a MongoDB server.

  * This module is maintained by The Ansible Community
OPTIONS (= is mandatory):

- login_database
        The database where login credentials are stored.
        [Default: (null)]
        type: str

- login_host
        The host running the database.
        [Default: localhost]
        type: str

- login_password
        The login user's password used to authenticate with.
        [Default: (null)]
        type: str

- login_port
        The MongoDB port to connect to.
        [Default: 27017]
        type: int

- login_user
        The MongoDB username used to authenticate with.
        [Default: (null)]
        type: str

= param
        MongoDB administrative parameter to modify.

        type: str

- param_type
        Define the type of parameter value.
        (Choices: int, str)[Default: str]
        type: str

- replica_set
        Replica set to connect to (automatically connects to primary
        for writes).
        [Default: (null)]
        type: str

- ssl
        Whether to use an SSL connection when connecting to the
        database.
        [Default: False]
        type: bool

= value
        MongoDB administrative parameter value to set.

        type: str


NOTES:
      * Requires the pymongo Python package on the remote host,
        version 2.4.2+.
      * This can be installed using pip or the OS package
        manager.
      * See also
        http://api.mongodb.org/python/current/installation.html


REQUIREMENTS:  pymongo

AUTHOR: Loic Blot (@nerzhul)
        METADATA:
          status:
          - preview
          supported_by: community
        

EXAMPLES:

- name: Set MongoDB syncdelay to 60 (this is an int)
  mongodb_parameter:
    param: syncdelay
    value: 60
    param_type: int


RETURN VALUES:

before:
    description: value before modification
    returned: success
    type: str
after:
    description: value after modification
    returned: success
    type: str

