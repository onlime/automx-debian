=====================
 Basic Configuration
=====================

This document contains information for initial and/or basic configuration of automx.

automx reads runtime behaviour and all settings controlling a domains account provisioning from a single configuration file. By default automx expects to find this file at /etc/automx.conf.

Format
======

The general format of the automx.conf file is as follows:

- The basic element contained in an INI file is the property. Every property has a name and a value, delimited by an equals sign (“=”). The name appears to the left of the equals sign.
- Properties are grouped into sections. The section name appears on a line by itself, in square brackets (“[” and “]”). All properties after the section declaration are associated with that section. There is no explicit “end of section” delimiter; sections end at the next section declaration, or the end of the file. Sections may not be nested.
- Section and property names are case sensitive.
- A line with a number sign (“#”) begins a comment. Anything following a number sign will be ignored by automx.


Sections
========

Sections create a namespace in which properties specific to a domain are defined. The section name identifies the domain. The three section names [automx], [DEFAULT] and [global] are reserved for special purposes within automx.

[automx]
''''''''

Controlling automx Runtime Behaviour

This section is mandatory - it lists all options controlling automx runtime behaviour. The properties provider and domains are also mandatory. Usage of memcache and all of its associated properties is highly recommended.

[automx]

The following example shows a typical [automx] section setup::

        [automx]
        provider = example.com  1
        domains = *  2
        logfile = /var/log/automx/automx.log  3
        debug = yes  4
        memcache = 127.0.0.1:11211  5
        memcache_ttl = 86400
        client_error_limit = 5  6
        rate_limit_exception_networks = 127.0.0.0/8, ::1/128  7

1
        The provider property configures automx to identify the webservice as example.com.

2
        The wildcard option * used in domains instructs automx to answer any configuration request regardless of the domain sent by the mail client.

3
        All log information should go to /var/log/automx/automx.log.

4
        Debugging is enabled and infos will be sent to logfile.

5
        Statistical data controlling errors caused by clients accessing database backends will be sent to the specified memcache service.

6
        In this example a client may not cause more than 5 errors before automx will refuse to answer further queries.

7
        Clients listed in rate_limit_exception_networks are excluded from rate limiting.


[DEFAULT]
'''''''''

Properties present in all other sections

This section is optional. Settings in this section define properties which will be present in all other sections. It is useful to avoid redundancy.

[DEFAULT]

The following example shows a typical [DEFAULT] section setup::

        [DEFAULT]
        action = settings  1
        account_type = email  2
        account_name = Example Inc.  3
        account_name_short = Example  4

1
        The default action for automx is to provide account settings.

        .. NOTE::

           The Microsoft schema forsees other actions that account provisioning.

2
        The account_type should be an email account.

3
        The account should show up as Example Inc. in the clients account list.

4
        The accounts short name should be Example.


[global]
''''''''

A global backend

Setting this section is mandatory, but it may remain empty. It provides a backend, which will be used whenever automx should serve a domain, but no section with domain-specific settings has been specified.

Other sections may either explicitly or implicitly refer to the [global] section as a whole. An explicit reference specifies global as backend property. Implicit references simply announce the domain in automx' domains list and omit an explicit section definition for that domain.

        .. NOTE::

          This is useful when many domains should use the same backend or when automx domain property configures it to run as wildcard service.

[global]

The following example configures automx to query a LDAP directory service. Refer to automx_ldap(5) for a detailed discussion of parameters and their meaning::

        [global]
        backend = ldap

        account_name = ${cn} (Example Inc.)
        display_name = ${givenName} ${sn}

        smtp = yes
        smtp_server = mail.example.com
        smtp_port = 587
        smtp_encryption = starttls
        smtp_auth = plaintext
        smtp_auth_identity = ${mail}
        smtp_expiration_date = 2012-12-31
        smtp_refresh_ttl = 0
        smtp_default = yes

        imap = yes
        imap_server = mail.example.com
        imap_port = 993
        imap_encryption = ssl
        imap_auth = plaintext
        imap_auth_identity = ${mail}
        imap_expiration_date = 2012-12-31
        imap_refresh_ttl = 0

        pop = no
        pop_server = mail.example.com
        pop_port = 995
        pop_encryption = ssl
        pop_auth = plaintext
        pop_auth_identity = ${mail}
        pop_expiration_date = 2012-12-31
        pop_refresh_ttl = 0

        host = ldap://ldap.example.com
        base = ou=people,dc=example,dc=com
        result_attrs = mail, cn, givenName, sn
        scope = sub
        filter = (&(objectClass=*)(uniqueIdentifier=%s))

        bindmethod = sasl
        saslmech = EXTERNAL
        usetls = yes
        reqcert = demand
        cert = /etc/ssl/certs/mail.example.com.crt.pem
        key = /etc/ssl/private/mail.example.com.key.pem
        cacert = /etc/ssl/certs/ca-certificates.crt


Authors
'''''''

Christian Rößner <cr@ys4.de>
        Wrote the program.

Patrick Ben Koetter <p@sys4.de>
        Wrote the documentation.
