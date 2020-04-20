> MONGODB_INFO    (/home/runner/work/community.mongodb/community.mongodb/plugins/modules/mongodb_info.py)

        Gather information about MongoDB instance.

  * This module is maintained by The Ansible Community
OPTIONS (= is mandatory):

- filter
        Limit the collected information by comma separated string or
        YAML list.
        Allowable values are `general', `databases', `total_size',
        `parameters', `users', `roles'.
        By default, collects all subsets.
        You can use '!' before value (for example, `!users') to
        exclude it from the information.
        If you pass including and excluding values to the filter, for
        example, `filter=!general,users', the excluding values,
        `!general' in this case, will be ignored.
        [Default: (null)]
        elements: str
        type: list

- login_database
        The database where login credentials are stored.
        [Default: admin]
        type: str

- login_host
        The host running MongoDB instance to login to.
        [Default: localhost]
        type: str

- login_password
        The password used to authenticate with.
        Required when `login_user' is specified.
        [Default: (null)]
        type: str

- login_port
        The MongoDB server port to login to.
        [Default: 27017]
        type: int

- login_user
        The MongoDB user to login with.
        Required when `login_password' is specified.
        [Default: (null)]
        type: str

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


NOTES:
      * Requires the pymongo Python package on the remote host,
        version 2.4.2+.


REQUIREMENTS:  pymongo

AUTHOR: Andrew Klychkov (@Andersson007)
        METADATA:
          status:
          - preview
          supported_by: community
        

EXAMPLES:

- name: Gather all supported information
  mongodb_info:
    login_user: admin
    login_password: secret
  register: result

- name: Show gathered info
  debug:
    msg: '{{ result }}'

- name: Gather only information about databases and their total size
  mongodb_info:
    login_user: admin
    login_password: secret
    filter: databases, total_size

- name: Gather all information except parameters
  mongodb_info:
    login_user: admin
    login_password: secret
    filter: '!parameters'


RETURN VALUES:

general:
  description: General instance information.
  returned: always
  type: dict
  sample: {"allocator": "tcmalloc", "bits": 64, "storageEngines": ["biggie"], "version": "4.2.3", "maxBsonObjectSize": 16777216}
databases:
  description: Database information.
  returned: always
  type: dict
  sample: {"admin": {"empty": false, "sizeOnDisk": 245760}, "config": {"empty": false, "sizeOnDisk": 110592}}
total_size:
  description: Total size of all databases in bytes.
  returned: always
  type: int
  sample: 397312
users:
  description: User information.
  returned: always
  type: dict
  sample: {"new_user": {"_id": "config.new_user", "db": "config", "mechanisms": ["SCRAM-SHA-1", "SCRAM-SHA-256"], "roles": []}}
roles:
  description: Role information.
  returned: always
  type: dict
  sample: {"restore": {"db": "admin", "inheritedRoles": [], "isBuiltin": true, "roles": []}}
parameters:
  description: Server parameters information.
  returned: always
  type: dict
  sample: {"maxOplogTruncationPointsAfterStartup": 100, "maxOplogTruncationPointsDuringStartup": 100, "maxSessions": 1000000}

