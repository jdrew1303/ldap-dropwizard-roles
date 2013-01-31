Example LDAP Authentication in [Dropwizard][dw]
=========================================

This is a sample project bringing together a number of pieces from various
sources that tie together:


- LDAP-based HTTP Basic authentication
- Roles-based Injectibles
- SSL-enabled

Configuration
-------------
See the [`example-config.yaml`](https://github.com/klauern/ldap-dropwizard-roles/blob/master/example-config.yaml) for details pertinent to setting up the project for
your own LDAP server.  Dropwizard is an incredibly flexible set of helpers,
utilities, and wrappers around some of the best Java libraries out there, so
this app is more of that kind of goodness.


Project Layout
--------------
If you're interested in implementing your own LDAP authentication scheme,
you will want to crib some of the implementation of the [`LdapAuthenticator`](https://github.com/klauern/ldap-dropwizard-roles/blob/master/src/main/java/com/klauer/dropwizard/core/auth/ldap/LdapAuthenticator.java) and
[`LdapConnectionFactory`](https://github.com/klauern/ldap-dropwizard-roles/blob/master/src/main/java/com/klauer/dropwizard/core/auth/ldap/LdapConnectionFactory.java), which I borrowed from Xavier Shay in the [Dropwizard
Google Group](https://groups.google.com/d/msg/dropwizard-user/JTHdtIrwXGc/QXIdUgLoD90J).

For the roles-based authorization, I created an annotation, [`@AuthRolesAllowed`](https://github.com/klauern/ldap-dropwizard-roles/blob/master/src/main/java/com/klauer/dropwizard/core/auth/AuthRolesAllowed.java),
which is implemented with a provider and injectible: [`LdapAuthRolesAllowedInjectible`](https://github.com/klauern/ldap-dropwizard-roles/blob/master/src/main/java/com/klauer/dropwizard/core/auth/ldap/LdapAuthRolesAllowedInjectable.java)
and [`LdapAuthRolesAllowedProvider`](https://github.com/klauern/ldap-dropwizard-roles/blob/master/src/main/java/com/klauer/dropwizard/core/auth/ldap/LdapAuthRolesAllowedProvider.java).  Coda Hale has a [great article explaining
how injection providers work](http://codahale.com/what-makes-jersey-interesting-injection-providers/).


Caveat Emptor
---------------
It should come as no surprise that LDAP implementations vary, and a layout that works for me is sure to not work exactly out of the box for you.  For instance, while Mr. Shay's `LdapAuthenticator` provided 90% of the work, the difference in how users are looked up required code changes.  Furthermore, in order to do role-based lookup, I eschewed a few big components of the [Dropwizard Authentication](http://dropwizard.codahale.com/manual/auth/) module and rewrote my own to include roles.


Example Project Layout
-------------------------
Simply because `tree` works in DOS and I thought it was nifty to include:

     ./
    │   config.yaml
    │   example-config.yaml
    │   example.jks
    │   pom.xml
    │   README.md
    └───src
        └───main
            ├───java
            │   └───com
            │       └───klauer
            │           └───dropwizard
            │               │   ApplicationConfiguration.java
            │               │   ExampleService.java
            │               ├───core
            │               │   └───auth
            │               │       │   AuthRolesAllowed.java
            │               │       │   BasicAuth.java
            │               │       │   BasicCredentialsWithRoles.java
            │               │       │   User.java
            │               │       └───ldap
            │               │               LdapAuthenticator.java
            │               │               LdapAuthRolesAllowedInjectable.java
            │               │               LdapAuthRolesAllowedProvider.java
            │               │               LdapConfiguration.java
            │               │               LdapConnectionFactory.java
            │               └───resources
            │                       LdapRolesResource.java
            └───resources
                    banner.txt
    


[dw]: http://dropwizard.codahale.com