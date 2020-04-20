> MONGODB_USER    (/home/runner/work/community.mongodb/community.mongodb/plugins/modules/mongodb_user.py)

        Adds or removes a user from a MongoDB database.

  * This module is maintained by The Ansible Community
OPTIONS (= is mandatory):

= database
        The name of the database to add/remove the user from.
        (Aliases: db)
        type: str

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
        type: str

- login_user
        The MongoDB username used to authenticate with.
        [Default: (null)]
        type: str

= name
        The name of the user to add or remove.
        (Aliases: user)
        type: str

- password
        The password to use for the user.
        (Aliases: pass)[Default: (null)]
        type: str

- replica_set
        Replica set to connect to (automatically connects to primary
        for writes).
        [Default: (null)]
        type: str

- roles
        The database user roles valid values could either be one or
        more of the following strings: 'read', 'readWrite', 'dbAdmin',
        'userAdmin', 'clusterAdmin', 'readAnyDatabase',
        'readWriteAnyDatabase', 'userAdminAnyDatabase',
        'dbAdminAnyDatabase'
        Or the following dictionary '{ db: DATABASE_NAME, role:
        ROLE_NAME }'.
        This param requires pymongo 2.5+. If it is a string, mongodb
        2.4+ is also required. If it is a dictionary, mongo 2.6+ is
        required.
        [Default: (null)]
        elements: raw
        type: list

- ssl
        Whether to use an SSL connection when connecting to the
        database.
        [Default: (null)]
        type: bool

- ssl_cert_reqs
        Specifies whether a certificate is required from the other
        side of the connection, and whether it will be validated if
        provided.
        (Choices: CERT_NONE, CERT_OPTIONAL, CERT_REQUIRED)[Default:
        CERT_REQUIRED]
        type: str

- state
        The database user state.
        (Choices: absent, present)[Default: present]
        type: str

- update_password
        `always' will update passwords if they differ.
        `on_create' will only set the password for newly created
        users.
        (Choices: always, on_create)[Default: always]
        type: str


NOTES:
      * Requires the pymongo Python package on the remote host,
        version 2.4.2+. This can be installed using pip or the
        OS package manager. @see
        http://api.mongodb.org/python/current/installation.html


REQUIREMENTS:  pymongo

AUTHOR: Elliott Foster (@elliotttf), Julien Thebault (@Lujeni)
        METADATA:
          status:
          - preview
          supported_by: community
        

EXAMPLES:

- name: Create 'burgers' database user with name 'bob' and password '12345'.
  mongodb_user:
    database: burgers
    name: bob
    password: 12345
    state: present

- name: Create a database user via SSL (MongoDB must be compiled with the SSL option and configured properly)
  mongodb_user:
    database: burgers
    name: bob
    password: 12345
    state: present
    ssl: True

- name: Delete 'burgers' database user with name 'bob'.
  mongodb_user:
    database: burgers
    name: bob
    state: absent

- name: Define more users with various specific roles (if not defined, no roles is assigned, and the user will be added via pre mongo 2.2 style)
  mongodb_user:
    database: burgers
    name: ben
    password: 12345
    roles: read
    state: present

- name: Define roles
  mongodb_user:
    database: burgers
    name: jim
    password: 12345
    roles: readWrite,dbAdmin,userAdmin
    state: present

- name: Define roles
  mongodb_user:
    database: burgers
    name: joe
    password: 12345
    roles: readWriteAnyDatabase
    state: present

- name: Add a user to database in a replica set, the primary server is automatically discovered and written to
  mongodb_user:
    database: burgers
    name: bob
    replica_set: belcher
    password: 12345
    roles: readWriteAnyDatabase
    state: present

# add a user 'oplog_reader' with read only access to the 'local' database on the replica_set 'belcher'. This is useful for oplog access (MONGO_OPLOG_URL).
# please notice the credentials must be added to the 'admin' database because the 'local' database is not synchronized and can't receive user credentials
# To login with such user, the connection string should be MONGO_OPLOG_URL="mongodb://oplog_reader:oplog_reader_password@server1,server2/local?authSource=admin"
# This syntax requires mongodb 2.6+ and pymongo 2.5+
- name: Roles as a dictionary
  mongodb_user:
    login_user: root
    login_password: root_password
    database: admin
    user: oplog_reader
    password: oplog_reader_password
    state: present
    replica_set: belcher
    roles:
      - db: local
        role: read


RETURN VALUES:

user:
    description: The name of the user to add or remove.
    returned: success
    type: str

