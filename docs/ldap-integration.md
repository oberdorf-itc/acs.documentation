# LDAP Integration Guidelines

## Overview

The OITC Access Control System uses LDAP as a general configuration backend for access definitions, RFID transponder metadata, notification settings, technical accounts, and access control rules. LDAP provides a central, queryable repository for:

- access points and their configuration
- RFID transponder registration and activation state
- access roles and rules
- user and technical account information
- notification behavior for readers

The repository in `resources/openldap` contains the OpenLDAP schema and directory skeleton required to run the ACS backend.

## Required LDAP schema files

The ACS requires these schema files:

- `oitc-desfire.schema`
  - Defines MIFARE DESFire RFID transponder attributes and object classes such as `oitcTransponderID`, `oitcRFIDApplicationID`, and `oitcRFIDFileID`.
  - Provides DESFire-specific configuration metadata used by the ACS card management and RFID provisioning workflows.

- `oitc-acs.schema`
  - Defines ACS-specific auxiliary object classes and attributes for users, entry points, roles, and access rules.
  - Includes extension attributes such as `oitcACSTransponderIsActive`, `oitcACSEntryPointRules`, `oitcACSRoleName`, and `oitcACSLimitationTimeFrame`.

These custom schemas depend on standard OpenLDAP schemas that must also be available in your OpenLDAP installation:

- `core.schema`
- `cosine.schema`
- `nis.schema`
- `inetorgperson.schema`

The sample `slapd.conf` in `resources/openldap` includes all required schema files:

```text
include /etc/ldap/schema/core.schema
include /etc/ldap/schema/cosine.schema
include /etc/ldap/schema/inetorgperson.schema
include /etc/ldap/schema/nis.schema
include /etc/ldap/schema/oitc-desfire.schema
include /etc/ldap/schema/oitc-acs.schema
```

## OID namespace and schema purpose

The custom ACS schemas use the enterprise OID namespace allocated to Michael Oberdorf IT-Consulting:

- base OID: `1.3.6.1.4.1.46756`

Within that namespace:

- `1.3.6.1.4.1.46756.3.1.1` — attribute types for DESFire RFID objects
- `1.3.6.1.4.1.46756.3.1.2` — object classes for DESFire RFID objects
- `1.3.6.1.4.1.46756.3.2.1` — attribute types for ACS-specific extensions
- `1.3.6.1.4.1.46756.3.2.2` — object classes for ACS-specific extensions

The schema files therefore separate two main areas:

- `oitc-desfire.schema` for RFID transponder and DESFire application/file metadata
- `oitc-acs.schema` for Access Control System configuration objects, access rules, and auxiliary extensions on standard LDAP entries

## Directory tree skeleton (`dit.ldif`)

The file `resources/openldap/dit.ldif` defines the minimum directory information tree (DIT) skeleton required by the ACS. The root suffix is:

- `dc=oberdorf-itc,dc=de`

**Hint**: The root suffix and all the organizational units can be modified. It's just a simple basic structure.

Under that suffix, the DIT includes:

- `ou=OITC Access Control System,dc=oberdorf-itc,dc=de`
  - `ou=Entry Points` — configured RFID reader points
  - `ou=Transponders` — registered transponder objects
  - `ou=Access Roles` — role definitions
  - `ou=Access Rules` — access limitation rules
  - `ou=Notify Configuration` — reader notification settings

- `ou=Users,dc=oberdorf-itc,dc=de`
  - user entries and identities

- `ou=Technical Accounts,dc=oberdorf-itc,dc=de`
  - service accounts such as `Directory Manager`, `ACS Manager`, `Card Management System`, and `ACS Relay Control Service`

- `ou=Groups,dc=oberdorf-itc,dc=de`
  - administrative and ACS operator groups such as `Directory Administrators` and `OITC ACS Administrators`

### Key DIT entries

Important example entries from `dit.ldif`:

- `cn=noLimit,ou=Access Rules,ou=OITC Access Control System,dc=oberdorf-itc,dc=de`
  - A default access rule object of class `oitcACSAccessLimitation`

- `oitcACSNotifyId=error,ou=Notify Configuration,ou=OITC Access Control System,dc=oberdorf-itc,dc=de`
  - Reader notification configuration for error states

- `uid=Card Management System,ou=Technical Accounts,dc=oberdorf-itc,dc=de`
  - Technical account used by ACS card and transponder management

This skeleton ensures the ACS has the expected branches and object classes available at startup.

## How LDAP is used in the Access Control System

LDAP acts as the central configuration store for the ACS. The system uses it to store and resolve:

- RFID transponder metadata and active/inactive state
- ownership links between transponders and users
- access point configuration and rule associations
- access roles and limitation rules
- notification settings for reader status signals
- technical service accounts and operator groups

The ACS reads LDAP entries at runtime to decide whether a transponder has permission to open a door, which rule set applies, and how notifications should behave. LDAP is not only a user directory in this system; it is the primary configuration backend for access logic.

## Deployment notes

- Import `dit.ldif` into your OpenLDAP directory after the LDAP server is initialized.
- Ensure the custom schema files are loaded before adding ACS entries.
- Verify the base suffix is set to `dc=oberdorf-itc,dc=de` to match the sample DIT.
- Protect sensitive attributes such as `oitcACSTransponderRandomSecret` and DESFire keys with appropriate ACLs.

## Recommended configuration files

Use the sample `resources/openldap/slapd.conf` as a reference for:

- schema inclusion order
- loglevel and module configuration
- ACLs protecting ACS secrets and directory write access
- database suffix and root DN
- LDAP index configuration for ACS attributes

By following the schema and DIT structure in `resources/openldap`, the ACS can reliably use LDAP as its general configuration backend.
