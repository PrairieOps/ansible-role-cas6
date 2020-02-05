PrairieOps.cas6
=========

[CAS 6](https://apereo.github.io/cas/6.1.x/index.html) Ansible role by PrairieOps for OULibraries.
Forked from [OULibraries CAS 4.x ansible role](https://github.com/OULibraries/ansible-role-cas)

Currently focused on [LDAP integration](https://apereo.github.io/cas/6.1.x/installation/LDAP-Authentication.html), and [SAML2 integration](https://apereo.github.io/cas/6.1.x/installation/Configuring-SAML2-Authentication.html).

Requirements
------------

Expects to be deployed to [Red Hat Universal Base Image 7](https://access.redhat.com/containers/#/registry.access.redhat.com/ubi7/ubi), but will probably work fine with CentOS/RHEL 7.

Role Variables
--------------
You can optionally authenticate against a simple filestore with username::password combinations. Stores credentials in the clear. Don't use in production.

```
cas_fileauth_users:
  - username: username
    password: userpassword
```

You can optionally configure CAS to build with a custom overlay based
on [overlay template](https://github.com/apereo/cas-overlay-template)
from Apereo by specifying a git repository for the template and the
version to use.

```
cas_overlay_repo: 
cas_overlay_version: 
```

You can optionally enable a [SAML2 Identity Provider](https://apereo.github.io/cas/6.1.x/installation/Configuring-SAML2-Authentication.html) service in cas.
To do so, set:
```
cas_saml2_idp: true
cas_samlIdp_hostName: cas.example.com
cas_samlIdp_scope: example.com
```



You can optionally authenticate against multiple ldap systems.  This role currently supports authenticated search only.

```
cas_ldap:
    # URI for this LDAP endpoint
  - url: 'ldap://ldap.example.com'
    # Type of system:
    # AD|AUTHENTICATED|DIRECT|ANONYMOUS|SASL
    type: 'AD'
    # Start TLS for SSL connections
    useSsl: 'true'
    useStartTLS: 'false'
    # LDAP connection timeout in milliseconds
    connectTimeout: '5000'
    # Setting to true allows cas to start without this ldap provider
    failFast: 'true'
    # Base DN of users to be authenticated
    baseDn: 'OU=Users,DC=domain,DC=example,DC=com'
    # DN format for users to be authenticated
    dnFormat: '%s@example.com'
    # The filter is used to search for the user account
    # https://access.redhat.com/documentation/en-US/Red_Hat_Directory_Server/8.2/html/Administration_Guide/Finding_Directory_Entries-LDAP_Search_Filters.html
    userFilter: 'sAMAccountName={user}'
    # Recursively search the Base DN?
    subtreeSearch: 'true'
    # The attribute that represents the username
    principalAttributeId: 'sAMAccountName'
    # The attribute that represents the password
    principalAttributePassword: 'unicodePwd'
    # A list of principal attributes
    principalAttributeList: 'sn,cn,givenName,sAMAccountName'
    # Modifies principal attributes returned by CAS
    # NONE|UPPERCASE|LOWERCASE
    principalTransformation_caseConversion: 'NONE'
    # Allows multiple values per attribute
    allowMultiplePrincipalAttributeValues: 'true'
    # The junk drawer of user attributes
    additionalAttributes: 'memberOf'
    # Bind account DN
    bindDn: 'CN=ServiceAccount,OU=SomeNonStandardOU,DC=domain,DC=example,DC=com'
    # Bind account password
    bindCredential: 'AStrongPassphraseofSomeSort'
    # LDAP connection pool configuration
    pool_minSize: '0'
    pool_maxSize: '10'
    pool_validateOnCheckout: 'true'
    pool_validatePeriodically: 'true'
    pool_validatePeriod: '600'
```

You can optionally configure CAS to act as a client against a SAML ID provider. Learn about CAS and SAML to discover how to configure these values.

```
cas_pac4j_saml:
  storepass:
  keypass:
  dname:
  idp_md_url:
  entityId:
  logoutLocation:
  consumerLocation:
  orgName:
  orgDisplayname:
  orgURL:
```

See defaults/main.yml for the rest

Dependencies
------------

Example Playbook
----------------

```
- hosts: servers
  vars_files:
    - my-vars.yml
  roles:
    - PrairieOps.cas6
```

License
-------

[MIT](LICENSE)

Author Information
------------------

Jason Sherman
