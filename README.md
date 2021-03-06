# Autofs module for Puppet

[![Puppet Forge](http://img.shields.io/puppetforge/v/pdxcat/autofs.svg)](https://forge.puppetlabs.com/pdxcat/autofs) [![Build Status](https://travis-ci.org/pdxcat/puppet-module-autofs.png?branch=master)](https://travis-ci.org/pdxcat/puppet-module-autofs)

## Description
This is a maintainance release of `pdxcat/autofs` to support Sles.  You should probably use `puppet/autofs`

_Puppet module for managing Autofs mountpoints and files._

### Some Contrived Example usage

``` puppet
autofs::mount { '/home':
  map     => 'ldap:ou=home,ou=autofs,dc=cat,dc=pdx,dc=edu',
  options => '-rw,hard,intr,nosuid,nobrowse',
}

autofs::mount { '/cat':
  map     => 'ldap:ou=cat,ou=autofs,dc=cat,dc=pdx,dc=edu',
  options => '-ro,hard,intr,nosuid,browse',
}

autofs::include { 'auto.web': }

autofs::mount { '/www':
  map     => 'ldap:ou=www,ou=autofs,dc=cat,dc=pdx,dc=edu',
  options => '-rw,hard,intr,nosuid,browse',
  mapfile => '/etc/auto.web',
}

autofs::include { 'auto.local': }
```

### Resulting files

#### /etc/auto.master

```
/cat ldap:ou=cat,ou=autofs,dc=cat,dc=pdx,dc=edu -ro,hard,intr,nosuid,browse
/home ldap:ou=home,ou=autofs,dc=cat,dc=pdx,dc=edu -rw,hard,intr,nosuid,nobrowse
+auto.local
+auto.web
```

#### /etc/auto.web

```
/www ldap:ou=www,ou=autofs,dc=cat,dc=pdx,dc=edu -rw,hard,intr,nosuid,browse
```

#### /etc/auto.local

This file is not managed by puppet, and so there is no way to know what its
contents will be. Puppet doesn't care.
