# ansible-openssh

Deploy OpenSSH daemon configuration.

## Requirements

* Ansible 2.8+;

## Example configuration

```yaml
---
openssh:
# Enable sshd service or not.
- enable: 'true'
# Restart sshd service after deploy or not.
  restart: 'true'
# Restart method for sshd service - 'reload' or 'restart'.
  restart_method: 'reload'
# openssh daemon settings.
  settings:
# Specifies what environment variables sent by the client will be copied into
# the session's environ. The TERM environment variable is always accepted
# whenever the client requests a pseudo-terminal as it is required by the
# protocol. Variables are specified by name, which may contain the wildcard
# characters '*' and '?'. Multiple environment variables may be separated by
# whitespace or spread across multiple accept_env directives. Be warned that
# some environment variables could be used to bypass restricted user
# environments. For this reason, care should be taken in the use of this
# directive. The default is not to accept any environment variables.
  - accept_env:
    - 'FOO'
    - 'BAR'
# Specifies which address family should be used by sshd. Valid arguments are
# 'any' (the default), 'inet' (use IPv4 only), or 'inet6' (use IPv6 only).
    address_family: 'any'
# Specifies whether ssh-agent forwarding is permitted. The default is 'yes'.
# Note that disabling agent forwarding does not improve security unless users
# are also denied shell access, as they can always install their own forwarders.
    allow_agent_forwarding: 'yes'
# This keyword can be followed by a list of group name patterns, separated by
# spaces. If specified, login is allowed only for users whose primary group or
# supplementary group list matches one of the patterns. Only group names are
# valid, a numerical group ID is not recognized. By default, login is allowed
# for all groups. The allow/deny directives are processed in the following
# order: 'deny_users', 'allow_users', 'deny_groups', and finally 'allow_groups'.
    allow_groups:
    - '*'
# Specifies whether StreamLocal (Unix-domain socket) forwarding is permitted.
# The available options are 'yes' (the default) or 'all' to allow StreamLocal
# forwarding, 'no' to prevent all StreamLocal forwarding, 'local' to allow local
# (from the perspective of ssh) forwarding only or 'remote' to allow remote
# forwarding only. Note that disabling StreamLocal forwarding does not improve
# security unless users are also denied shell access, as they can always install
# their own forwarders.
    allow_stream_local_forwarding: 'yes'
# Specifies whether TCP forwarding is permitted. The available options are
# 'yes' (the default) or all to allow TCP forwarding, 'no' to prevent all TCP
# forwarding, local to allow local (from the perspective of ssh forwarding only
# or remote to allow remote forwarding only. Note that disabling TCP forwarding
# does not improve security unless users are also denied shell access, as they
# can always install their own forwarders.
    allow_tcp_forwarding: 'yes'
# This keyword can be followed by a list of user name patterns, separated by
# spaces. If specified, login is allowed only for user names that match one of
# the patterns. Only user names are valid a numerical user ID is not recognized.
# By default, login is allowed for all users. If the pattern takes the form
# USER@HOST then USER and HOST are separately checked, restricting logins to
# particular users from particular hosts. HOST criteria may additionally
# contain addresses to match in CIDR address/masklen format. The allow/deny
# directives are processed in the following order: deny_users, allow_users,
# deny_groups, and finally allow_groups.
    allow_users:
    - 'bob'
    - 'alice'
# Specifies the authentication methods that must be successfully completed for
# a user to be granted access. This option must be followed by one or more
# lists of comma-separated authentication method names, or by the single string
# 'any' to indicate the default behaviour of accepting any single authentication
# method. If the default is overridden, then successful authentication requires
# completion of every method in at least one of these lists. For example,
# "publickey,password publickey,keyboard-interactive" would require the user to
# complete public key authentication, followed by either password or keyboard
# interactive authentication. Only methods that are next in one or more lists
# are offered at each stage, so for this example it would not be possible to
# attempt password or keyboard-interactive authentication before public key.
# For keyboard interactive authentication it is also possible to restrict
# authentication to a specific device by appending a colon followed by the
# device identifier bsdauth or pam, depending on the server configuration. For
# example, "keyboard-interactive:bsdauth" would restrict keyboard interactive
# authentication to the bsdauth device. If the publickey method is listed more
# than once, sshd verifies that keys that have been used successfully are not
# reused for subsequent authentications. For example, "publickey,publickey"
# requires successful authentication using two different public keys. Note that
# each authentication method listed should also be explicitly enabled in the
# configuration.
# The available authentication methods are: 'password', 'publickey',
# 'gssapi-with-mic', 'hostbased', 'keyboard-interactive', 'none' - used for
# access to password-less accounts when 'permit_empty_passwords' is enabled;
    authentication_methods: 'any'
# Specifies a program to be used to look up the user's public keys. The program
# must be owned by root, not writable by group or others and specified by an
# absolute path. If no arguments are specified then the username of the target
# user is used. The program should produce on standard output zero or more
# lines of authorized_keys output (see AUTHORIZED_KEYS in sshd. If a key
# supplied by 'authorized_keys_command' does not successfully authenticate and
# authorize the user then public key authentication continues using the usual
# authorized_keys_file files. By default, no 'authorized_keys_command' is run.
    authorized_keys_command: ''
# Specifies the user under whose account the 'authorized_keys_command' is run. It
# is recommended to use a dedicated user that has no other role on the host
# than running authorized keys commands. If 'authorized_keys_command' is
# specified but 'authorized_keys_command_user' is not, then sshd will refuse to
# start.
    authorized_keys_command_user: 'nobody'
# Specifies the file that contains the public keys used for user authentication.
# The format is described in the AUTHORIZED_KEYS FILE FORMAT section of sshd.
# Arguments to authorized_keys_file accept the tokens described in the TOKENS
# section. After expansion, 'authorized_keys_file' is taken to be an absolute
# path or one relative to the user's home directory. Multiple files may be
# listed. Alternately this option may be set to none to skip checking for user
# keys in files.
    authorized_keys_file:
    - '.ssh/authorized_keys'
    - '.ssh/authorized_keys2'
# Specifies a program to be used to generate the list of allowed certificate
# principals as per authorized_principals_file. The program must be owned by
# root, not writable by group or others and specified by an absolute path.
# If no arguments are specified then the username of the target user is used.
# The program should produce on standard output zero or more lines of
# authorized_principals_file output. If either authorized_principals_command or
# authorized_principals_file is specified, then certificates offered by the
# client for authentication must contain a principal that is listed. By default,
# no authorized_principals_command is run.
    authorized_principals_command: ''
# Specifies a file that lists principal names that are accepted for certificate
# authentication. When using certificates signed by a key listed in
# 'trusted_user_ca_keys', this file lists names, one of which must appear in the
# certificate for it to be accepted for authentication. Names are listed one
# per line preceded by key options (as described in AUTHORIZED_KEYS FILE FORMAT
# in sshd. Empty lines and comments starting with '#' are ignored. After
# expansion, 'authorized_principals_file' is taken to be an absolute path or one
# relative to the user's home directory. The default is none, i.e. not to use a
# principals file in this case, the username of the user must appear in a
# certificate's principals list for it to be accepted. Note that
# 'authorized_principals_file' is only used when authentication proceeds using a
# CA listed in 'trusted_user_ca_keys' and is not consulted for certification
# authorities trusted via '~/.ssh/authorized_keys', though the principals= key
# option offers a similar facility.
    authorized_principals_command_user: 'nobody'
# Specifies the user under whose account the 'authorized_principals_command' is
# run. It is recommended to use a dedicated user that has no other role on the
# host than running authorized principals commands. If
# 'authorized_principals_command' is specified but
# 'authorized_principals_command_user' is not, then sshd will refuse to start.
    authorized_principals_file: ''
# The contents of the specified file are sent to the remote user before
# authentication is allowed. If the argument is none then no banner is
# displayed. By default, no banner is displayed.
    banner: ''
# Specifies which algorithms are allowed for signing of certificates by
# certificate authorities (CAs). Certificates signed using other algorithms
# will not be accepted for public key or host-based authentication.
    ca_signature_algorithms:
    - 'ecdsa-sha2-nistp256'
    - 'ecdsa-sha2-nistp384'
    - 'ecdsa-sha2-nistp521'
    - 'ssh-ed25519'
    - 'rsa-sha2-512'
    - 'rsa-sha2-256'
    - 'ssh-rsa'
# Specifies whether challenge-response authentication is allowed (e.g. via PAM
# or through authentication styles). The default is 'yes'.
    challenge_response_authentication: 'yes'
# Specifies the pathname of a directory to chroot to after authentication. At
# session startup sshd checks that all components of the pathname are
# root-owned directories which are not writable by any other user or group.
# After the chroot, sshd changes the working directory to the user's home
# directory. Arguments to chroot_directory accept the tokens described in the
# TOKENS section. The chroot_directory must contain the necessary files and
# directories to support the user's session. For an interactive session this
# requires at least a shell, typically sh, and basic '/dev' nodes such as
# 'null', 'zero', 'stdin', 'stdout', 'stderr', and 'tty' devices. For file
# transfer sessions using SFTP no additional configuration of the environment
# is necessary if the in-process sftp-server is used, though sessions which use
# logging may require '/dev/log' inside the chroot directory on some operating
# systems. For safety, it is very important that the directory hierarchy be
# prevented from modification by other processes on the system (especially those
# outside the jail). Misconfiguration can lead to unsafe environments which
# sshd cannot detect. The default is none, indicating not to chroot.
    chroot_directory: ''
# Specifies the ciphers allowed. Multiple ciphers must be comma-separated. If
# the specified value begins with a '+' character, then the specified ciphers
# will be appended to the default set instead of replacing them. If the
# specified value begins with a '-' character, then the specified ciphers
# (including wildcards) will be removed from the default set instead of
# replacing them.
    ciphers:
    - 'aes256-ctr'
    - 'aes128-ctr'
    - 'aes256-cbc'
    - 'aes128-cbc'
    - '3des-cbc'
# Sets the number of client alive messages which may be sent without sshd
# receiving any messages back from the client. If this threshold is reached
# while client alive messages are being sent, sshd will disconnect the client,
# terminating the session. It is important to note that the use of client alive
# messages is very different from 'tcp_keep_alive'. The client alive messages
# are sent through the encrypted channel and therefore will not be spoofable.
# The TCP keepalive option enabled by 'tcp_keep_alive' is spoofable. The client
# alive mechanism is valuable when the client or server depend on knowing when
# a connection has become inactive. The default value is '3'. If
# client_alive_interval is set to 15, and client_alive_count_max is left at the
# default, unresponsive SSH clients will be disconnected after approximately
# 45 seconds.
    client_alive_count_max: '3'
# Sets a timeout interval in seconds after which if no data has been received
# from the client, sshd will send a message through the encrypted channel to
# request a response from the client. The default is '0', indicating that these
# messages will not be sent to the client.
    client_alive_interval: '0'
# Specifies whether compression is enabled after the user has authenticated
# successfully. The argument must be 'yes', 'delayed' (a legacy synonym
# for 'yes') or 'no'. The default is 'yes'.
    compression: 'yes'
# This keyword can be followed by a list of group name patterns, separated by
# spaces. Login is disallowed for users whose primary group or supplementary
# group list matches one of the patterns. Only group names are valid, a
# numerical group ID is not recognized. By default, login is allowed for all
# groups. The allow/deny directives are processed in the following order:
# 'deny_users', 'allow_users', 'deny_groups', and finally 'allow_groups'.
    deny_groups:
    - 'developers'
    - 'accountants'
# This keyword can be followed by a list of user name patterns, separated by
# spaces. Login is disallowed for user names that match one of the patterns.
# Only user names are valid, a numerical user ID is not recognized. By default,
# login is allowed for all users. If the pattern takes the form USER@HOST then
# USER and HOST are separately checked, restricting logins to particular users
# from particular hosts. HOST criteria may additionally contain addresses to
# match in CIDR address/masklen format. The allow/deny directives are processed
# in the following order: 'deny_users', 'allow_users', 'deny_groups', and
# finally 'allow_groups'.
    deny_users:
    - 'bob'
    - 'alice'
# Disables all forwarding features, including X11, ssh-agent, TCP and
# StreamLocal. This option overrides all other forwarding related options and
# may simplify restricted configurations.
    disable_forwarding: 'false'
# Writes a temporary file containing a list of authentication methods and
# public credentials (e.g. keys) used to authenticate the user. The location of
# the file is exposed to the user session through the SSH_USER_AUTH environment
# variable. The default is 'no'.
    expose_auth_info: 'no'
# Specifies the hash algorithm used when logging key fingerprints. Valid
# options are: 'md5' and 'sha256'. The default is 'sha256'.
    fingerprint_hash: 'sha256'
# Forces the execution of the command specified by force_command, ignoring any
# command supplied by the client and ~/.ssh/rc if present. The command is
# invoked by using the user's login shell with the '-c' option. This applies to
# shell, command, or subsystem execution. It is most useful inside a Match
# block. The command originally supplied by the client is available in the
# SSH_ORIGINAL_COMMAND environment variable. Specifying a command of
# internal-sftp will force the use of an in-process SFTP server that requires
# no support files when used with chroot_directory. The default is none.
    force_command: ''
# Specifies whether remote hosts are allowed to connect to ports forwarded for
# the client. By default, sshd binds remote port forwardings to the loopback
# address. This prevents other remote hosts from connecting to forwarded ports.
# gateway_ports can be used to specify that sshd should allow remote port
# forwardings to bind to non-loopback addresses, thus allowing other hosts to
# connect. The argument may be 'no' to force remote port forwardings to be
# available to the local host only, 'yes' to force remote port forwardings to
# bind to the wildcard address, or 'clientspecified' to allow the client to
# select the address to which the forwarding is bound. The default is 'no'.
    gateway_ports: ''
# Specifies whether user authentication based on GSSAPI is allowed. The default
# is 'no'.
    gssapi_authentication: 'no'
# Specifies whether to automatically destroy the user's credentials cache on
# logout. The default is 'yes'.
    gssapi_cleanup_credentials: 'yes'
# Determines whether to be strict about the identity of the GSSAPI acceptor a
# client authenticates against. If set to 'yes' then the client must
# authenticate against the host service on the current hostname. If set to 'no'
# then the client may authenticate against any service key stored in the
# machine's default store. This facility is provided to assist with operation
# on multi homed machines. The default is 'yes'.
    gssapi_strict_acceptor_check: 'yes'
# Specifies the key types that will be accepted for hostbased authentication
# as a list of comma-separated patterns. Alternately if the specified value
# begins with a '+' character, then the specified key types will be appended to
# the default set instead of replacing them. If the specified value begins with
# a '-' character, then the specified key types (including wildcards) will be
# removed from the default set instead of replacing them. The default for this
# option is:
    hostbased_accepted_key_types:
    - 'ecdsa-sha2-nistp256-cert-v01@openssh.com'
    - 'ecdsa-sha2-nistp384-cert-v01@openssh.com'
    - 'ecdsa-sha2-nistp521-cert-v01@openssh.com'
    - 'ssh-ed25519-cert-v01@openssh.com'
    - 'rsa-sha2-512-cert-v01@openssh.com'
    - 'rsa-sha2-256-cert-v01@openssh.com'
    - 'ssh-rsa-cert-v01@openssh.com'
    - 'ecdsa-sha2-nistp256'
    - 'ecdsa-sha2-nistp384'
    - 'ecdsa-sha2-nistp521'
    - 'ssh-ed25519'
    - 'rsa-sha2-512'
    - 'rsa-sha2-256'
    - 'ssh-rsa'
# Specifies whether rhosts or /etc/hosts.equiv authentication together with
# successful public key client host authentication is allowed (host-based
# authentication). The default is 'no'.
    hostbased_authentication: 'no'
# Specifies whether or not the server will attempt to perform a reverse name
# lookup when matching the name in the '~/.shosts', '~/.rhosts', and
# '/etc/hosts.equiv' files during 'hostbased_authentication'. A setting of 'yes'
# means that sshd uses the name supplied by the client rather than attempting
# to resolve the name from the TCP connection itself. The default is 'no'.
    hostbased_uses_name_from_packet_only: 'no'
# Specifies a file containing a public host certificate. The certificate's
# public key must match a private host key already specified by 'host_key'. The
# default behaviour of sshd is not to load any certificates.
    host_certificate: ''
# Specifies a file containing a private host key used by SSH. The defaults are
# '/etc/ssh/ssh_host_ecdsa_key', '/etc/ssh/ssh_host_ed25519_key' and
# '/etc/ssh/ssh_host_rsa_key'. Note that sshd will refuse to use a file if it
# is group/world-accessible and that the 'host_key_algorithms' option restricts
# which of the keys are actually used by sshd. It is possible to have multiple
# host key files. It is also possible to specify public host key files instead.
# In this case operations on the private key will be delegated to an
# sshHostKeyAlgorithms-agent.
    host_key: ''
# Identifies the UNIX-domain socket used to communicate with an agent that has
# access to the private host keys. If the string 'SSH_AUTH_SOCK' is specified,
# the location of the socket will be read from the 'SSH_AUTH_SOCK' environment
# variable.
    host_key_agent: ''
# Specifies the host key algorithms that the server offers. The default for
# this option is:
    host_key_algorithms:
    - 'ecdsa-sha2-nistp256-cert-v01@openssh.com'
    - 'ecdsa-sha2-nistp384-cert-v01@openssh.com'
    - 'ecdsa-sha2-nistp521-cert-v01@openssh.com'
    - 'ssh-ed25519-cert-v01@openssh.com'
    - 'rsa-sha2-512-cert-v01@openssh.com'
    - 'rsa-sha2-256-cert-v01@openssh.com'
    - 'ssh-rsa-cert-v01@openssh.com'
    - 'ecdsa-sha2-nistp256'
    - 'ecdsa-sha2-nistp384'
    - 'ecdsa-sha2-nistp521'
    - 'ssh-ed25519'
    - 'rsa-sha2-512'
    - 'rsa-sha2-256'
    - 'ssh-rsa'
# Specifies that '.rhosts' and '.shosts' files will not be used in
# hostbased_authentication. '/etc/hosts.equiv' and '/etc/ssh/shosts.equiv' are
# still used. The default is 'yes'.
    ignore_rhosts: 'yes'
# Specifies whether sshd should ignore the user's '~/.ssh/known_hosts' during
# hostbased_authentication and use only the system-wide known hosts file
# '/etc/ssh/known_hosts'. The default is 'no'.
    ignore_user_known_hosts: 'no'
# Specifies the IPv4 type-of-service or DSCP class for the connection. Accepted
# values are 'af11', 'af12', 'af13', 'af21', 'af22', 'af23', 'af31', 'af32',
# 'af33', 'af41', 'af42', 'af43', 'cs0', 'cs1', 'cs2', 'cs3', 'cs4', 'cs5',
# 'cs6', 'cs7', 'ef', 'lowdelay', 'throughput', 'reliability', a numeric value,
# or none to use the operating system default. This option may take one or two
# arguments, separated by whitespace.
# If one argument is specified, it is used as the packet class unconditionally.
# If two values are specified, the first is automatically selected for
# interactive sessions and the second for non-interactive sessions. The default
# is 'af21' (Low-Latency Data) for interactive sessions and 'cs1'
# (Lower Effort) for non-interactive sessions.
    ip_qos: 'af21 cs1'
# Specifies whether to allow keyboard-interactive authentication. The argument
# to this keyword must be 'yes' or 'no'. The default is to use whatever value
# 'challenge_response_authentication' is set to (by default 'yes').
    kbd_interactive_authentication: 'yes'
# Specifies whether the password provided by the user for
# 'password_authentication' will be validated through the Kerberos KDC. To use
# this option, the server needs a Kerberos servtab which allows the
# verification of the KDC's identity. The default is 'no'.
    kerberos_authentication: 'no'
# If AFS is active and the user has a Kerberos 5 TGT, attempt to acquire an AFS
# token before accessing the user's home directory. The default is 'no'.
    kerberos_get_afs_token: ''
# If password authentication through Kerberos fails then the password will be
# validated via any additional local mechanism such as '/etc/passwd'. The
# default is 'yes'.
    kerberos_or_local_passwd: 'yes'
# Specifies whether to automatically destroy the user's ticket cache file on
# logout. The default is 'yes'.
    kerberos_ticket_cleanup: 'yes'
# Specifies the available KEX (Key Exchange) algorithms. Multiple algorithms
# must be comma-separated. Alternately if the specified value begins with a '+'
# character, then the specified methods will be appended to the default set
# instead of replacing them. If the specified value begins with a '-' character,
# then the specified methods (including wildcards) will be removed from the
# default set instead of replacing them. The default is:
    kex_algorithms:
    - 'curve25519-sha256'
    - 'curve25519-sha256@libssh.org'
    - 'ecdh-sha2-nistp256'
    - 'ecdh-sha2-nistp384'
    - 'ecdh-sha2-nistp521'
    - 'diffie-hellman-group-exchange-sha256'
    - 'diffie-hellman-group16-sha512'
    - 'diffie-hellman-group18-sha512'
    - 'diffie-hellman-group14-sha256'
    - 'diffie-hellman-group14-sha1'
# Specifies the local addresses sshd should listen on. If port is not specified,
# sshd will listen on the address and all Port options specified. The default is
# to listen on all local addresses on the current default routing domain.
    listen_address:
    - '192.168.1.1:22'
    - '5.128.220.1:2222'
# The server disconnects after this time if the user has not successfully
# logged in. If the value is '0', there is no time limit. The default is '120'
# seconds.
    login_grace_time: '120'
# Gives the verbosity level that is used when logging messages from sshd.
# The possible values are: 'QUIET', 'FATAL', 'ERROR', 'INFO', 'VERBOSE',
# 'DEBUG', 'DEBUG1', 'DEBUG2', and 'DEBUG3'. The default is 'INFO'. 'DEBUG' and
# 'DEBUG1' are equivalent. 'DEBUG2' and 'DEBUG3' each specify higher levels of
# debugging output. Logging with a 'DEBUG' level violates the privacy of users
# and is not recommended.
    log_level: 'INFO'
# Specifies the available MAC (message authentication code) algorithms. The MAC
# algorithm is used for data integrity protection. Multiple algorithms must be
# comma-separated. If the specified value begins with a '+' character, then the
# specified algorithms will be appended to the default set instead of replacing
# them. If the specified value begins with a '-' character, then the specified
# algorithms (including wildcards) will be removed from the default set instead
# of replacing them. The default is:
    macs:
    - 'umac-64-etm@openssh.com'
    - 'umac-128-etm@openssh.com'
    - 'hmac-sha2-256-etm@openssh.com'
    - 'hmac-sha2-512-etm@openssh.com'
    - 'hmac-sha1-etm@openssh.com'
    - 'umac-64@openssh.com'
    - 'umac-128@openssh.com'
    - 'hmac-sha2-256'
    - 'hmac-sha2-512'
    - 'hmac-sha1'
# Specifies the maximum number of authentication attempts permitted per
# connection. Once the number of failures reaches half this value, additional
# failures are logged. The default is '6'.
    max_auth_tries: '6'
# Specifies the maximum number of open shell, login or subsystem (e.g. sftp)
# sessions permitted per network connection. Multiple sessions may be
# established by clients that support connection multiplexing. Setting
# MaxSessions to '1' will effectively disable session multiplexing, whereas
# setting it to '0' will prevent all shell, login and subsystem sessions while
# still permitting forwarding. The default is '10'.
    max_sessions: '10'
# Specifies the maximum number of concurrent unauthenticated connections to the
# SSH daemon. Additional connections will be dropped until authentication
# succeeds or the 'login_grace_time' expires for a connection. The default is
# '10:30:100'. Alternatively, random early drop can be enabled by specifying
# the three colon separated values 'start:rate:full' (e.g. '10:30:60'). sshd
# will refuse connection attempts with a probability of rate/100 (30%) if
# there are currently start (10) unauthenticated connections. The probability
# increases linearly and all connection attempts are refused if the number of
# unauthenticated connections reaches full (60).
    max_startups:
    - start: '10'
      rate: '30'
      full: '100'
# Specifies whether password authentication is allowed. The default is 'yes'.
    password_authentication: 'yes'
# When password authentication is allowed, it specifies whether the server
# allows login to accounts with empty password strings. The default is 'no'.
    permit_empty_passwords: 'no'
# Specifies the addresses/ports on which a remote TCP port forwarding may
# listen. Multiple permissions may be specified, an argument of any can be used
# to remove all restrictions and permit any listen requests. An argument of
# none can be used to prohibit all listen requests The host name may contain
# wildcards, the wildcard '*' can also be used in place of a port number to
# allow all ports. By default all port forwarding listen requests are permitted.
# Note that the gateway_ports option may further restrict which addresses may be
# listened on. Note also that ssh will request a listen host of 'localhost' if
# no listen host was specifically requested, and this this name is treated
# differently to explicit localhost addresses of '127.0.0.1' and '::1'.
    permit_listen:
    - port: '22'
    - host: '5.128.0.1'
      port: '2222'
# Specifies the destinations to which TCP port forwarding is permitted.
# Multiple forwards may be specified, an argument of 'any' can be used to remove
# all restrictions and permit any forwarding requests. An argument of 'none' can
# be used to prohibit all forwarding requests. The wildcard '*' can be used for
# host or port to allow all hosts or ports, respectively. By default all port
# forwarding requests are permitted.
    permit_open:
    - host: '*'
      port: '*'
# Specifies whether root can log in using ssh. The argument must be 'yes',
# 'prohibit-password', 'forced-commands-only', or 'no'. The default is
# 'prohibit-password'. If this option is set to 'prohibit-password', password
# and keyboard-interactive authentication are disabled for root. If this option
# is set to 'forced-commands-only', root login with public key authentication
# will be allowed, but only if the command option has been specified (which may
# be useful for taking remote backups even if root login is normally not
# allowed). All other authentication methods are disabled for root. If this
# option is set to 'no', root is not allowed to log in.
    permit_root_login: 'prohibit-password'
# Specifies whether pty allocation is permitted. The default is 'yes'.
    permit_tty: 'yes'
# Specifies whether tun device forwarding is allowed. The argument must be
# 'yes', 'point-to-point' (layer 3), 'ethernet' (layer 2), or 'no'. Specifying
# 'yes' permits both 'point-to-point' and 'ethernet'. The default is 'no'.
# Independent of this setting, the permissions of the selected tun device must
# allow access to the user.
    permit_tunnel: 'no'
# Specifies whether '~/.ssh/environment' and 'environment=' options in
# '~/.ssh/authorized_keys' are processed by sshd. Valid options are 'yes', 'no'
# or a pattern-list specifying which environment variable names to accept
# (for example 'LANG,LC_*'). The default is 'no'. Enabling environment
# processing may enable users to bypass access restrictions in some
# configurations using mechanisms such as LD_PRELOAD.
    permit_user_environment: 'no'
# Specifies whether any '~/.ssh/rc' file is executed. The default is 'yes'.
    permit_user_rc: 'yes'
# Specifies the file that contains the process ID of the SSH daemon, or none to
# not write one. The default is '/run/sshd.pid'.
    pid_file: '/run/sshd.pid'
# Specifies the port number that sshd listens on. The default is '22'.
    port: '22'
# Specifies whether sshd should print the date and time of the last user login
# when a user logs in interactively. The default is 'yes'.
    print_last_log: 'yes'
# Specifies whether sshd should print '/etc/motd' when a user logs in
# interactively. On some systems it is also printed by the shell,
# '/etc/profile', or equivalent). The default is 'yes'.
    print_motd: 'yes'
# Specifies the key types that will be accepted for public key authentication.
# Alternately if the specified value begins with a '+' character, then the
# specified key types will be appended to the default set instead of replacing
# them. If the specified value begins with a '-' character, then the specified
# key types (including wildcards) will be removed from the default set instead
# of replacing them. The default for this option is:
    pub_key_accepted_key_types:
    - 'ecdsa-sha2-nistp256-cert-v01@openssh.com'
    - 'ecdsa-sha2-nistp384-cert-v01@openssh.com'
    - 'ecdsa-sha2-nistp521-cert-v01@openssh.com'
    - 'ssh-ed25519-cert-v01@openssh.com'
    - 'rsa-sha2-512-cert-v01@openssh.com'
    - 'rsa-sha2-256-cert-v01@openssh.com'
    - 'ssh-rsa-cert-v01@openssh.com'
    - 'ecdsa-sha2-nistp256'
    - 'ecdsa-sha2-nistp384'
    - 'ecdsa-sha2-nistp521'
    - 'ssh-ed25519'
    - 'rsa-sha2-512'
    - 'rsa-sha2-256'
    - 'ssh-rsa'
# Specifies whether public key authentication is allowed. The default is 'yes'.
    pubkey_authentication: 'yes'
# Specifies the maximum amount of data that may be transmitted before the
# session key is renegotiated, optionally followed a maximum amount of time that
# may pass before the session key is renegotiated. The first argument is
# specified in bytes and may have a suffix of 'K', 'M', or 'G' to indicate
# Kilobytes, Megabytes, or Gigabytes, respectively. The default is between '1G'
# and '4G', depending on the cipher. The optional second value is specified in
# seconds and may use any of the units documented in the TIME FORMATS section.
# The default value for RekeyLimit is default none, which means that rekeying
# is performed after the cipher's default amount of data has been sent or
# received and no time based rekeying is done.
    rekey_limit: ''
# Specifies revoked public keys file, or none to not use one. Keys listed in
# this file will be refused for public key authentication. Note that if this
# file is not readable, then public key authentication will be refused for all
# users. Keys may be specified as a text file, listing one public key per line,
# or as an OpenSSH Key Revocation List (KRL) as generated by ssh-keygen. For
# more information on KRLs, see the KEY REVOCATION LISTS section in ssh-keygen.
    revoked_keys: ''
# Specifies an explicit routing domain that is applied after authentication has
# completed. The user session, as well and any forwarded or listening IP
# sockets, will be bound to this rdomain. If the routing domain is set to %D,
# then the domain in which the incoming connection was received will be applied.
    r_domain: ''
# Specifies one or more environment variables to set in child sessions started
# by sshd as "NAME=VALUE". Environment variables set by 'set_env' override the
# default environment and any variables specified by the user via 'accept_env'
# or 'permit_user_environment'.
    set_env:
    - key: 'FOO'
      value: 'bar'
    - key: 'SPACE'
      value: 'separated value'
# Sets the octal file creation mode mask (umask) used when creating a
# Unix-domain socket file for local or remote port forwarding. This option is
# only used for port forwarding to a Unix-domain socket file. The default value
# is '0177', which creates a Unix-domain socket file that is readable and
# writable only by the owner. Note that not all operating systems honor the
# file mode on Unix-domain socket files.
    stream_local_bind_mask: '0177'
# Specifies whether to remove an existing Unix-domain socket file for local or
# remote port forwarding before creating a new one. If the socket file already
# exists and 'stream_local_bind_unlink' is not enabled, sshd will be unable to
# forward the port to the Unix-domain socket file. This option is only used for
# port forwarding to a Unix-domain socket file. The argument must be 'yes' or
# 'no'. The default is 'no'.
    stream_local_bind_unlink: 'no'
# Specifies whether sshd should check file modes and ownership of the user's
# files and home directory before accepting login. This is normally desirable
# because novices sometimes accidentally leave their directory or files
# world-writable. The default is 'yes'. Note that this does not apply to
# 'chroot_directory', whose permissions and ownership are checked
# unconditionally.
    strict_modes: ''
# Configures an external subsystem (e.g. file transfer daemon). Arguments
# should be a subsystem name and a command (with optional arguments) to execute
# upon subsystem request. The command sftp-server implements the SFTP file
# transfer subsystem. Alternately the name internal-sftp implements an
# in-process SFTP server. This may simplify configurations using
# 'chroot_directory' to force a different filesystem root on clients. By default
# no subsystems are defined.
    subsystem: ''
# Gives the facility code that is used when logging messages from sshd. The
# possible values are: 'DAEMON', 'USER', 'AUTH', 'LOCAL0', 'LOCAL1', 'LOCAL2',
# 'LOCAL3', 'LOCAL4', 'LOCAL5', 'LOCAL6', 'LOCAL7'. The default is 'AUTH'.
    syslog_facility: 'AUTH'
# Specifies whether the system should send TCP keepalive messages to the other
# side. If they are sent, death of the connection or crash of one of the
# machines will be properly noticed. However, this means that connections will
# die if the route is down temporarily, and some people find it annoying. On
# the other hand, if TCP keepalives are not sent, sessions may hang
# indefinitely on the server, leaving 'ghost' users and consuming server
# resources. The default is 'yes' (to send TCP keepalive messages), and the
# server will notice if the network goes down or the client host crashes. This
# avoids infinitely hanging sessions. To disable TCP keepalive messages, the
# value should be set to 'no'.
    tcp_keep_alive: 'yes'
# Specifies a file containing public keys of certificate authorities that are
# trusted to sign user certificates for authentication, or none to not use one.
# Keys are listed one per line, if a certificate is presented for
# authentication and has its signing CA key listed in this file, then it may be
# used for authentication for any user listed in the certificate's principals
# list. Note that certificates that lack a list of principals will not be
# permitted for authentication using 'trusted_user_ca_keys'.
    trusted_user_ca_keys: ''
# Specifies whether sshd should look up the remote host name, and to check that
# the resolved host name for the remote IP address maps back to the very same
# IP address. If this option is set to 'no' (the default) then only addresses
# and not host names may be used in '~/.ssh/authorized_keys' from and
# sshd_config 'match host' directives.
    use_dns: 'no'
# Enables the Pluggable Authentication Module interface. If set to 'yes' this
# will enable PAM authentication using 'challenge_response_authentication' and
# 'password_authentication' in addition to PAM account and session module
# processing for all authentication types. Because PAM challenge-response
# authentication usually serves an equivalent role to password authentication,
# you should disable either 'password_authentication' or
# 'challenge_response_authentication'. If 'use_pam' is enabled, you will not be
# able to run sshd as a non-root user. The default is 'no'.
    use_pam: 'no'
# Optionally specifies additional text to append to the SSH protocol banner
# sent by the server upon connection. The default is none.
    version_addendum: ''
# Specifies the first display number available for sshd's X11 forwarding. This
# prevents sshd from interfering with real X11 servers. The default is '10'.
    x11_display_offset: '10'
# Specifies whether X11 forwarding is permitted. The argument must be 'yes' or
# 'no'. The default is 'no'. When X11 forwarding is enabled, there may be
# additional exposure to the server and to client displays if the sshd proxy
# display is configured to listen on the wildcard address, though this is not
# the default. Additionally, the authentication spoofing and authentication
# data verification and substitution occur on the client side. The security
# risk of using X11 forwarding is that the client's X11 display server may be
# exposed to attack when the SSH client requests forwarding. A system
# administrator may have a stance in which they want to protect clients that
# may expose themselves to attack by unwittingly requesting X11 forwarding,
# which can warrant a no setting. Note that disabling X11 forwarding does not
# prevent users from forwarding X11 traffic, as users can always install their
# own forwarders.
    x11_forwarding: 'no'
# Specifies whether sshd should bind the X11 forwarding server to the loopback
# address or to the wildcard address. By default, sshd binds the forwarding
# server to the loopback address and sets the hostname part of the DISPLAY
# environment variable to localhost. This prevents remote hosts from connecting
# to the proxy display. However, some older X11 clients may not function with
# this configuration. X11UseLocalhost may be set to 'no' to specify that the
# forwarding server should be bound to the wildcard address. The argument must
# be 'yes' or 'no'. The default is 'yes'.
    x11_use_localhost: 'yes'
# Specifies the full pathname of the xauth program, or none to not use one.
# The default is '/usr/bin/xauth'.
    x_auth_location: '/usr/bin/xauth'
# Introduces a conditional block. If all of the criteria on the match are
# satisfied, the keywords on the following lines override those set in the
# global section of the config file.
    match:
    - condition:
      - key: 'User'
        value: 'bender'
      - key: 'Address'
        value:
        - '10.0.0.0/8'
        - '100.64.0.0/10'
        - '192.168.0.0/16'
        - '198.19.0.0/16'
      policy:
      - key: 'PasswordAuthentication'
        value: 'yes'
      - key: 'AuthenticationMethods'
        value:
        - 'publickey'
        - 'password'
```
