# Reference

<!-- DO NOT EDIT: This document was generated by Puppet Strings -->

## Table of Contents

### Classes

* [`simp_apache`](#simp_apache): Configures an Apache server
* [`simp_apache::conf`](#simp_apacheconf): This class sets up apache.conf.
* [`simp_apache::install`](#simp_apacheinstall): Apache package management
* [`simp_apache::service`](#simp_apacheservice): Control the Apache service
* [`simp_apache::ssl`](#simp_apachessl): Configures an Apache server with SSL support
* [`simp_apache::validate`](#simp_apachevalidate): Should be used as input to `validate_deep_hash` when managing `ldap`

### Defined types

* [`simp_apache::site`](#simp_apachesite): This adds a 'site' to your configuration.

### Resource types

* [`htaccess`](#htaccess): Manages the contents of htaccess files using the htpasswd command. Right now the $namevar must be a path/user combination as documented under

### Functions

* [`simp_apache::auth`](#simp_apacheauth): Takes a hash of arguments related to Apache 'Auth' settings and returns a reasonably formatted set of options.  Currently, only htaccess and 
* [`simp_apache::limits`](#simp_apachelimits): Takes a hash of arguments related to Apache 'Limits' settings and returns a reasonably formatted set of options.  Currently, host, user ('val
* [`simp_apache::munge_httpd_networks`](#simp_apachemunge_httpd_networks): Provides a method by which an array of networks can be properly formatted for an Apache Allow/Deny segment.  This handles the case of 0.0.0.0

### Data types

* [`Simp_apache::LogSeverity`](#simp_apachelogseverity): Valid log serveries for Apache

## Classes

### `simp_apache`

Ensures that the appropriate files are in the appropriate places and can
optionally rsync the `/var/www/html` content.

Ideally, we will move over to the Puppet Labs apache module in the future but
it's going to be quite a bit of work to port all of our code.

#### Parameters

The following parameters are available in the `simp_apache` class.

##### `data_dir`

Data type: `Stdlib::AbsolutePath`

The location where apache web data should be stored. Set to /srv/www for
legacy reasons.

Default value: `'/var/www'`

##### `rsync_web_root`

Data type: `Boolean`

Whether or not to rsync over the web root.

Default value: ``true``

##### `ssl`

Data type: `Boolean`

Whether or not to enable SSL. You will need to set the Hiera
variables for apache::ssl appropriately for your needs.

Default value: ``true``

##### `rsync_source`

Data type: `String`

The source on the rsync server.

Default value: `"apache_${::environment}_${facts['os']['name']}/www"`

##### `rsync_server`

Data type: `Simplib::Host`

The name/address of the rsync server.

Default value: `simplib::lookup('simp_options::rsync::server',  { 'default_value' => '127.0.0.1' })`

##### `rsync_timeout`

Data type: `Integer`

The rsync connection timeout.

Default value: `simplib::lookup('simp_options::rsync::timeout', { 'default_value' => 2 })`

### `simp_apache::conf`

This class sets up apache.conf.

* **See also**
  * The
    * following parameters are referenced in the stock apache
documentation

#### Parameters

The following parameters are available in the `simp_apache::conf` class.

##### `httpd_timeout`

Data type: `Integer`

The Timeout variable. Renamed to not conflict with the Puppet
reserved word 'timeout'.

Default value: `120`

##### `httpd_loglevel`

Data type: `Simp_apache::LogSeverity`

The LogLevel variable. Renamed to not conflict with the Puppet
reserved word 'loglevel'.

Default value: `'warn'`

##### `listen`

Data type: `Array[Variant[Simplib::Host::Port, Simplib::Port]]`

An array of ports upon which Apache should listen.

NOTE: If you are using an IPv6 with a port, you need to
  bracket the address

Default value: `[80]`

##### `firewall`

Data type: `Boolean`

Whether or not to use the SIMP IPTables module.

Default value: `simplib::lookup('simp_options::firewall', { 'default_value' => false })`

##### `syslog`

Data type: `Boolean`

Whether or not to use the SIMP Rsyslog module.

Default value: `simplib::lookup('simp_options::syslog', { 'default_value' => false })`

##### `syslog_target`

Data type: `Stdlib::AbsolutePath`

If $syslog is true, store the apache logs at this
location.

Default value: `'/var/log/httpd'`

##### `purge`

Data type: `Boolean`

Whether or not to purge the configuration directories.

Default value: ``true``

##### `keepalive`

Data type: `Boolean`



Default value: ``false``

##### `maxkeepalive`

Data type: `Integer`



Default value: `100`

##### `keepalivetimeout`

Data type: `Integer`



Default value: `15`

##### `prefork_startservers`

Data type: `Integer`



Default value: `8`

##### `prefork_minspareservers`

Data type: `Integer`



Default value: `5`

##### `prefork_maxspareservers`

Data type: `Integer`



Default value: `20`

##### `prefork_serverlimit`

Data type: `Integer`



Default value: `3000`

##### `prefork_maxclients`

Data type: `Integer`



Default value: `3000`

##### `prefork_maxrequestsperchild`

Data type: `Integer`



Default value: `4000`

##### `worker_startservers`

Data type: `Integer`



Default value: `2`

##### `worker_maxclients`

Data type: `Integer`



Default value: `3000`

##### `worker_minsparethreads`

Data type: `Integer`



Default value: `25`

##### `worker_maxsparethreads`

Data type: `Integer`



Default value: `75`

##### `worker_threadsperchild`

Data type: `Integer`



Default value: `25`

##### `worker_maxrequestsperchild`

Data type: `Integer`



Default value: `0`

##### `includes`

Data type: `Optional[Array[String]]`



Default value: ``undef``

##### `serveradmin`

Data type: `String`



Default value: `'root@localhost'`

##### `servername`

Data type: `Optional[String]`



Default value: ``undef``

##### `allowroot`

Data type: `Simplib::Netlist`



Default value: `['127.0.0.1','::1']`

##### `defaulttype`

Data type: `String`



Default value: `'text/plain'`

##### `enablemmap`

Data type: `Boolean`



Default value: ``true``

##### `enablesendfile`

Data type: `Boolean`



Default value: ``true``

##### `user`

Data type: `String`



Default value: `'apache'`

##### `group`

Data type: `String`



Default value: `'apache'`

##### `logformat`

Data type: `String`



Default value: `'%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"'`

##### `logfacility`

Data type: `Simplib::Syslog::LowerFacility`



Default value: `'local6'`

### `simp_apache::install`

Apache package management

#### Parameters

The following parameters are available in the `simp_apache::install` class.

##### `httpd_ensure`

Data type: `String`

The ensure status the httpd package

Default value: `simplib::lookup('simp_options::package_ensure', { 'default_value' => 'installed' })`

##### `mod_ldap_ensure`

Data type: `String`

The ensure status the mod_ldap package

Default value: `simplib::lookup('simp_options::package_ensure', { 'default_value' => 'installed' })`

##### `mod_ssl_ensure`

Data type: `String`

The ensure status the mod_ssl package

Default value: `simplib::lookup('simp_options::package_ensure', { 'default_value' => 'installed' })`

### `simp_apache::service`

Control the Apache service

#### Parameters

The following parameters are available in the `simp_apache::service` class.

##### `manage`

Data type: `Boolean`

Whether or not to manage the service

If set to `false`, you may need to add the service name to
`svckill::ignore` if you are in enforcing mode.

Default value: ``true``

##### `service_name`

Data type: `String[1]`

The name of the service to manage

Default value: `'httpd'`

##### `ensure`

Data type: `String[1]`

The state that the service should be in

Default value: `'running'`

##### `enable`

Data type: `Boolean`

Whether or not to enable the daemon

Default value: ``true``

##### `hasstatus`

Data type: `Boolean`

Whether or not the service has a 'status' command

Default value: ``true``

##### `hasrestart`

Data type: `Boolean`

If set to `true` then the contents of `$restart` will be ignored

Default value: ``false``

##### `restart`

Data type: `String[1]`

A specific command to use to restart the daemon

* Ignored if `$hasrestart` is set to `true`
* The `sleep 3` is in place to prevent a race condition from happening and
  the `reload || restart` is in place to try to force a clean restart if a
  reload fails to do the job.

Default value: `'/bin/sleep 3; /sbin/service httpd reload || /sbin/service httpd restart'`

### `simp_apache::ssl`

Ensures that the appropriate files are in the appropriate places and have the
correct permissions.

@NOTE: Any parameter that comes directly from Apache is not documented
here and should be found in the Apache mod_ssl reference
documentation.

* **See also**
  * https://httpd.apache.org/docs/current/mod/mod_ssl.html#sslverifyclient

#### Parameters

The following parameters are available in the `simp_apache::ssl` class.

##### `listen`

Data type: `Array[Variant[Simplib::Host::Port, Simplib::Port]]`

An array of ports upon which the stock SSL configuration should
listen.

@NOTE: If you are using an IPv6 with a port, you need to bracket the
address

Default value: `[443]`

##### `trusted_nets`

Data type: `Simplib::Netlist`

An array of networks that you trust to connect to your server.

Default value: `simplib::lookup('simp_options::trusted_nets', { 'default_value' => ['127.0.0.1', '::1'] })`

##### `logformat`

Data type: `String`

The default LogFormat to be used for SSL logging. Set to '' to
disable logging.

Default value: `'%t %h %{SSL_CLIENT_S_DN_CN}x %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b %s'`

##### `enable_default_vhost`

Data type: `Boolean`

Whether to activate the default VirtualHost on the $listen port.

Default value: ``true``

##### `firewall`

Data type: `Boolean`

Whether to use the SIMP iptables module.

Default value: `simplib::lookup('simp_options::firewall', { 'default_value' => false, })`

##### `pki`

Data type: `Variant[Boolean,Enum['simp']]`

* If 'simp', include SIMP's pki module and use pki::copy to manage
  application certs in /etc/pki/simp_apps/simp_apache/x509
* If true, do *not* include SIMP's pki module, but still use pki::copy
  to manage certs in /etc/pki/simp_apps/simp_apache/x509
* If false, do not include SIMP's pki module and do not use pki::copy
  to manage certs.  You will need to appropriately assign a subset of:
  * app_pki_dir
  * app_pki_key
  * app_pki_cert
  * app_pki_ca
  * app_pki_ca_dir

Default value: `simplib::lookup('simp_options::pki', { 'default_value' => false })`

##### `app_pki_external_source`

Data type: `String`

* If pki = 'simp' or true, this is the directory from which certs will be
  copied, via pki::copy.  Defaults to /etc/pki/simp/x509.

* If pki = false, this variable has no effect.

Default value: `simplib::lookup('simp_options::pki::source', { 'default_value' => '/etc/pki/simp/x509' })`

##### `app_pki_dir`

Data type: `Stdlib::AbsolutePath`

This variable controls the basepath of $app_pki_key, $app_pki_cert,
$app_pki_ca, $app_pki_ca_dir, and $app_pki_crl.
It defaults to /etc/pki/simp_apps/simp_apache/pki.

Default value: `'/etc/pki/simp_apps/simp_apache/x509'`

##### `app_pki_key`

Data type: `Stdlib::AbsolutePath`

Path and name of the private SSL key file

Default value: `"${app_pki_dir}/private/${facts['fqdn']}.pem"`

##### `app_pki_cert`

Data type: `Stdlib::AbsolutePath`

Path and name of the public SSL certificate

Default value: `"${app_pki_dir}/public/${facts['fqdn']}.pub"`

##### `app_pki_ca_dir`

Data type: `Stdlib::AbsolutePath`

Path to the CA.

Default value: `"${app_pki_dir}/cacerts"`

##### `haveged`

Data type: `Boolean`

Whether to use the SIMP haveged module to assist with entropy generation.

Default value: `simplib::lookup('simp_options::haveged', { 'default_value' => false })`

##### `openssl_cipher_suite`

Data type: `Array[String]`

The Cipher Suite the client is permitted to negotiate in the SSL handshake
phase.

Default value: `simplib::lookup('simp_options::openssl::cipher_suite', { 'default_value' => ['DEFAULT', '!MEDIUM'] })`

##### `ssl_protocols`

Data type: `Array[String]`

This directive can be used to control which versions of the SSL/TLS
protocol will be accepted in new connections.

Default value: `['TLSv1.2']`

##### `ssl_honor_cipher_order`

Data type: `Boolean`

Option to prefer the server's cipher preference order.

Default value: ``true``

##### `sslverifyclient`

Data type: `String`

This directive sets the Certificate verification level for the Client
Authentication.

Default value: `'require'`

##### `sslverifydepth`

Data type: `Integer`

This directive sets how deeply mod_ssl should verify before deciding that
the clients don't have a valid certificate.

Default value: `10`

### `simp_apache::validate`

or `limits` ACLs

## Defined types

### `simp_apache::site`

It simply pulls a $name'd template from the templates/sites directory under
the apache module, or somewhere else if you specify.  The name should be
'something'.conf and should be an Apache includable configuration file.

#### Examples

##### 

```puppet
site { 'public': }
```

#### Parameters

The following parameters are available in the `simp_apache::site` defined type.

##### `content`

Data type: `String`

Set this to something other than 'base' if you with to write in your own
custom content on the fly.

Default value: `'base'`

## Resource types

### `htaccess`

Manages the contents of htaccess files using the htpasswd command.
Right now the $namevar must be a path/user combination as
documented under the $name parameter. Hopefully, this can be fixed
in the future.

Note: If you want different permissions than root:root 640, you
will need to create a 'file' object to manage the target file.

#### Properties

The following properties are available in the `htaccess` type.

##### `ensure`

Valid values: `present`, `absent`

The basic property that the resource should be in.

Default value: `present`

##### `password`

The user's new password either as an SHA hash or as plain text.
Anything not prefixed with {SHA} will be treated as plain text.

#### Parameters

The following parameters are available in the `htaccess` type.

##### `name`

namevar

A variable of the format 'path:username'. This will hopefully be
split in the future but, for now, you cannot use usernames that
contain a colon ':'.

##### `provider`

The specific backend to use for this `htaccess` resource. You will seldom need to specify this --- Puppet will usually
discover the appropriate provider for your platform.

## Functions

### `simp_apache::auth`

Type: Ruby 4.x API

Takes a hash of arguments related to Apache 'Auth' settings and
returns a reasonably formatted set of options.

Currently, only htaccess and LDAP support are implemented.

#### Examples

##### Htaccess and LDAP authentication:

```puppet
simp_apache::auth({
  # Htaccess support
  'file' => {
    'enable'    => 'true',
    'user_file' => '/etc/httpd/conf.d/test/.htdigest'
  }
  'ldap'    => {
    'enable'      => 'true',
    # The LDAP server URI in Apache form.
    'url'         => ['ldap://server1','ldap://server2'],
    # Must be one of 'NONE', 'SSL', 'TLS', or 'STARTTLS'
    'security'    => 'STARTTLS',
    'binddn'      => 'cn=happy,ou=People,dc=your,dc=domain',
    'bindpw'      => 'birthday',
    'search'      => 'ou=People,dc=your,dc=domain',
    # Whether or not your LDAP groups are POSIX groups.
    'posix_group' => 'true'
   }
 }
)

Output:
  AuthName "Please Authenticate"
  AuthType Basic
  AuthBasicProvider ldap file
  AuthLDAPUrl "ldap://server1 server2/ou=People,dc=your,dc=domain" STARTTLS
  AuthLDAPBindDN "cn=happy,ou=People,dc=your,dc=domain',
  AuthLDAPBindPassword 'birthday'
  AuthLDAPGroupAttributeIsDN off
  AuthLDAPGroupAttribute memberUid
  AuthUserFile /etc/httpd/conf.d/elasticsearch/.htdigest
```

#### `simp_apache::auth(Hash $auth_hash)`

Takes a hash of arguments related to Apache 'Auth' settings and
returns a reasonably formatted set of options.

Currently, only htaccess and LDAP support are implemented.

Returns: `String` Formatted Apache authentication settings

##### Examples

###### Htaccess and LDAP authentication:

```puppet
simp_apache::auth({
  # Htaccess support
  'file' => {
    'enable'    => 'true',
    'user_file' => '/etc/httpd/conf.d/test/.htdigest'
  }
  'ldap'    => {
    'enable'      => 'true',
    # The LDAP server URI in Apache form.
    'url'         => ['ldap://server1','ldap://server2'],
    # Must be one of 'NONE', 'SSL', 'TLS', or 'STARTTLS'
    'security'    => 'STARTTLS',
    'binddn'      => 'cn=happy,ou=People,dc=your,dc=domain',
    'bindpw'      => 'birthday',
    'search'      => 'ou=People,dc=your,dc=domain',
    # Whether or not your LDAP groups are POSIX groups.
    'posix_group' => 'true'
   }
 }
)

Output:
  AuthName "Please Authenticate"
  AuthType Basic
  AuthBasicProvider ldap file
  AuthLDAPUrl "ldap://server1 server2/ou=People,dc=your,dc=domain" STARTTLS
  AuthLDAPBindDN "cn=happy,ou=People,dc=your,dc=domain',
  AuthLDAPBindPassword 'birthday'
  AuthLDAPGroupAttributeIsDN off
  AuthLDAPGroupAttribute memberUid
  AuthUserFile /etc/httpd/conf.d/elasticsearch/.htdigest
```

##### `auth_hash`

Data type: `Hash`

Hash containing desired Apache authentication
methods and relevant parameters as key value pairs. The
key is the authentication method, while the corresponding
value is a Hash of relevant parameters.

### `simp_apache::limits`

Type: Ruby 4.x API

Takes a hash of arguments related to Apache 'Limits' settings and
returns a reasonably formatted set of options.

Currently, host, user ('valid-user' only), ldap-user, and 
ldap-group limits are supported.  The hash keys for these are
host limit: 'hosts'
user limit: 'users'; only applies for 'valid-user', all others assumed
  LDAP users
ldap-user limit: 'users'
ldap-group limit: 'ldap_groups'

Groups of LDAP user primary groups are not supported since you would need
to know the GID.

#### Examples

##### Host, user and ldap_group limits:

```puppet

apache_limits(
  {
    # Set the defaults
    # If this is omitted, it just defaults to 'GET'.
    'defaults' => [ 'GET', 'POST', 'PUT' ],
    # Allow the hosts/subnets below to GET, POST, and PUT to ES.
    'hosts'  => {
      '1.2.3.4'     => 'defaults',
      '3.4.5.6'     => 'defaults',
      '10.1.2.0/24' => 'defaults'
    },
    # You can make a special user 'valid-user' that will translate to
    # allowing all valid users.
    'users'  => {
      # Allow user bob GET, POST, and PUT to ES.
      'bob'     => 'defaults',
      # Allow user alice GET, POST, PUT, and DELETE to ES.
      'alice'   => ['GET','POST','PUT','DELETE']
    },
    'ldap_groups' => {
       # Let the nice users read from ES.
       "cn=nice_users,ou=Group,${::basedn}" => 'defaults'
     }
  }
)

Output:
  <Limit DELETE>
    Order allow,deny
    Require user alice
    Satisfy any
  </Limit>

  <Limit GET>
    Order allow,deny
    Allow from 1.2.3.4
    Allow from 3.4.5.6
    Allow from 10.1.2.0/24
    Require ldap-user bob
    Require ldap-user alice
    Require ldap-group cn=nice_users,ou=Group,dc=your,dc=domain
    Satisfy any
  </Limit>

  <Limit POST>
    Order allow,deny
    Allow from 1.2.3.4
    Allow from 3.4.5.6
    Allow from 10.1.2.0/24
    Require ldap-user bob
    Require ldap-user alice
    Require ldap-group cn=nice_users,ou=Group,dc=your,dc=domain
    Satisfy any
  </Limit>

  <Limit PUT>
    Order allow,deny
    Allow from 1.2.3.4
    Allow from 3.4.5.6
    Allow from 10.1.2.0/24
    Require ldap-user bob
    Require ldap-user alice
    Require ldap-group cn=nice_users,ou=Group,dc=your,dc=domain
    Satisfy any
  </Limit>
```

#### `simp_apache::limits(Hash $limits_hash)`

Takes a hash of arguments related to Apache 'Limits' settings and
returns a reasonably formatted set of options.

Currently, host, user ('valid-user' only), ldap-user, and 
ldap-group limits are supported.  The hash keys for these are
host limit: 'hosts'
user limit: 'users'; only applies for 'valid-user', all others assumed
  LDAP users
ldap-user limit: 'users'
ldap-group limit: 'ldap_groups'

Groups of LDAP user primary groups are not supported since you would need
to know the GID.

Returns: `String` Formatted Apache limits settings

##### Examples

###### Host, user and ldap_group limits:

```puppet

apache_limits(
  {
    # Set the defaults
    # If this is omitted, it just defaults to 'GET'.
    'defaults' => [ 'GET', 'POST', 'PUT' ],
    # Allow the hosts/subnets below to GET, POST, and PUT to ES.
    'hosts'  => {
      '1.2.3.4'     => 'defaults',
      '3.4.5.6'     => 'defaults',
      '10.1.2.0/24' => 'defaults'
    },
    # You can make a special user 'valid-user' that will translate to
    # allowing all valid users.
    'users'  => {
      # Allow user bob GET, POST, and PUT to ES.
      'bob'     => 'defaults',
      # Allow user alice GET, POST, PUT, and DELETE to ES.
      'alice'   => ['GET','POST','PUT','DELETE']
    },
    'ldap_groups' => {
       # Let the nice users read from ES.
       "cn=nice_users,ou=Group,${::basedn}" => 'defaults'
     }
  }
)

Output:
  <Limit DELETE>
    Order allow,deny
    Require user alice
    Satisfy any
  </Limit>

  <Limit GET>
    Order allow,deny
    Allow from 1.2.3.4
    Allow from 3.4.5.6
    Allow from 10.1.2.0/24
    Require ldap-user bob
    Require ldap-user alice
    Require ldap-group cn=nice_users,ou=Group,dc=your,dc=domain
    Satisfy any
  </Limit>

  <Limit POST>
    Order allow,deny
    Allow from 1.2.3.4
    Allow from 3.4.5.6
    Allow from 10.1.2.0/24
    Require ldap-user bob
    Require ldap-user alice
    Require ldap-group cn=nice_users,ou=Group,dc=your,dc=domain
    Satisfy any
  </Limit>

  <Limit PUT>
    Order allow,deny
    Allow from 1.2.3.4
    Allow from 3.4.5.6
    Allow from 10.1.2.0/24
    Require ldap-user bob
    Require ldap-user alice
    Require ldap-group cn=nice_users,ou=Group,dc=your,dc=domain
    Satisfy any
  </Limit>
```

##### `limits_hash`

Data type: `Hash`

Hash containing desired Apache limits

### `simp_apache::munge_httpd_networks`

Type: Ruby 4.x API

Provides a method by which an array of networks can be properly formatted
for an Apache Allow/Deny segment.

This handles the case of 0.0.0.0/0, which Apache doesn't care for and
this function will convert to 'ALL'.

The case where a <dotted quad address>/<dotted quatted netmask> is
passed is also handled since Apache doesn't care for these at all.

#### `simp_apache::munge_httpd_networks(Array $networks)`

Provides a method by which an array of networks can be properly formatted
for an Apache Allow/Deny segment.

This handles the case of 0.0.0.0/0, which Apache doesn't care for and
this function will convert to 'ALL'.

The case where a <dotted quad address>/<dotted quatted netmask> is
passed is also handled since Apache doesn't care for these at all.

Returns: `Array` Array of network s formated appropriately for Apache

##### `networks`

Data type: `Array`

Array of networks to be converted to Apache format

## Data types

### `Simp_apache::LogSeverity`

Valid log serveries for Apache

Alias of `Enum['emerg', 'alert', 'crit', 'err', 'warn', 'notice', 'info', 'debug']`

