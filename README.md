![DK Hostmaster Logo][DKHMLOGO]

# DK Hostmaster EPP Service Specification

![Markdownlint Action][GHAMKDBADGE]
![Spellcheck Action][GHASPLLBADGE]

2021-04-08
Revision: 4.0

## Table of Contents

<!-- MarkdownTOC bracket=round levels="1,2,3,4,5" indent="  " autolink="true" autoanchor="true" -->

- [Introduction](#introduction)
  - [About this Document](#about-this-document)
  - [License](#license)
  - [Document History](#document-history)
- [The .dk Registry in Brief](#the-dk-registry-in-brief)
- [EPP in Brief](#epp-in-brief)
- [EPP Service](#epp-service)
  - [SSL/TLS Support](#ssltls-support)
  - [Available Environments](#available-environments)
    - [Production](#production)
    - [Sandbox](#sandbox)
- [Implementation Requirements](#implementation-requirements)
  - [Client Transaction ID \(`clTRID`\)](#client-transaction-id-cltrid)
  - [IP Whitelisting](#ip-whitelisting)
- [Implementation Extensions](#implementation-extensions)
  - [`dkhm:authinfoexdate`](#authinfoexdate)
  - [`dkhm:autoRenew`](#dkhmautorenew)
  - [`dkhm:contact_validated`](#dkhmcontact_validated)
  - [`dkhm:CVR`](#dkhmcvr)
  - [`dkhm:delDate`](#dkhmdeldate)
  - [`dkhm:domain_confirmed`](#dkhmdomain_confirmed)
  - [`dkhm:domainAdvisory`](#dkhmdomainadvisory)
  - [`dkhm:EAN`](#dkhmean)
  - [`dkhm:management`](#dkhmmanagement)
  - [`dkhm:mobilephone`](#dkhmmobilephone)
  - [`dkhm:orderconfirmationToken`](#dkhmorderconfirmationtoken)
  - [`dkhm:pnumber`](#dkhmpnumber)
  - [`dkhm:registrant_validated`](#dkhmregistrant_validated)
  - [`dkhm:requestedNsAdmin`](#dkhmrequestednsadmin)
  - [`dkhm:risk_assessment`](#dkhmrisk_assessment)
  - [`dkhm:secondaryEmail`](#dkhmsecondaryemail)
  - [`dkhm:trackingNo`](#dkhmtrackingno)
  - [`dkhm:url`](#dkhmurl)
  - [`dkhm:userType`](#dkhmusertype)
- [Implementation Limitations](#implementation-limitations)
  - [Unimplemented Commands](#unimplemented-commands)
  - [Authentication](#authentication)
  - [AuthInfo](#authinfo)
  - [DNSSEC](#dnssec)
  - [Contact Creation](#contact-creation)
  - [Host Update](#host-update)
  - [Domain Update](#domain-update)
  - [Host Info](#host-info)
  - [Waiting List]("waiting-list)
  - [Contact Info](#contact-info)
  - [Domain Info](#domain-info)
  - [Information Disclosure](#information-disclosure)
  - [Encoding and IDN domains](#encoding-and-idn-domains)
  - [Disclosure of Client ID](#disclosure-of-client-id)
  - [Unsupported Domain Status Codes](#unsupported-domain-status-codes)
  - [Unsupported Contact Status Codes](#unsupported-contact-status-codes)
  - [Unsupported Host Status Codes](#unsupported-host-status-codes)
- [Supported Object Transform and Query Commands](#supported-object-transform-and-query-commands)
  - [hello and greeting](#hello-and-greeting)
  - [login](#login)
    - [login request](#login-request)
    - [login response](#login-response)
  - [logout](#logout)
    - [logout request](#logout-request)
    - [logout response](#logout-response)
  - [poll and Message Queue](#poll-and-message-queue)
    - [poll req request](#poll-req-request)
    - [poll req response](#poll-req-response)
    - [poll ack request](#poll-ack-request)
    - [poll ack response](#poll-ack-response)
    - [poll ack response for non-existent message \(or previously acknowledged message\)](#poll-ack-response-for-non-existent-message-or-previously-acknowledged-message)
  - [balance and Prepaid Account](#balance-and-prepaid-account)
    - [balance request](#balance-request)
    - [balance response](#balance-response)
  - [Domain](#domain)
    - [create domain](#create-domain)
      - [create domain request](#create-domain-request)
      - [create domain response](#create-domain-response)
      - [Poll and Messages](#poll-and-messages)
        - [create domain poll message for successful creation](#create-domain-poll-message-for-successful-creation)
        - [create domain poll message for unsuccessful creation, existing domain](#create-domain-poll-message-for-unsuccessful-creation-existing-domain)
      - [Role Mapping](#role-mapping)
    - [check domain](#check-domain)
      - [check domain request](#check-domain-request)
      - [check domain response](#check-domain-response)
    - [info domain](#info-domain)
      - [info domain request](#info-domain-request)
      - [info domain response](#info-domain-response)
    - [renew domain](#renew-domain)
      - [renew domain request](#renew-domain-request)
      - [renew domain response](#renew-domain-response)
    - [update domain](#update-domain)
      - [update domain request](#update-domain-request)
      - [update domain response](#update-domain-response)
      - [change registrant](#change-registrant)
      - [add name server](#add-name-server)
      - [remove name server](#remove-name-server)
      - [add contact](#add-contact)
      - [remove contact](#remove-contact)
      - [remove DSRECORDS](#remove-dsrecords)
      - [add DSRECORDS](#add-dsrecords)
      - [setting AuthInfo](#setting-authinfo)
      - [unsetting AuthInfo](#unsetting-authinfo)
      - [AuthInfo Token Format](#authinfo-token-format)
    - [delete domain](#delete-domain)
      - [delete domain request](#delete-domain-request)
      - [delete domain response](#delete-domain-response)
    - [restore domain](#restore-domain)
      - [restore domain request](#restore-domain-request)
      - [restore domain response](#restore-domain-response)
    - [transfer domain](#transfer-domain)
      - [transfer domain request](#transfer-domain-request)
      - [transfer domain response](#transfer-domain-response)
      - [Transition Period](#transition-period)
    - [withdraw](#withdraw)
      - [withdraw request](#withdraw-request)
      - [withdraw response](#withdraw-response)
  - [Contact](#contact)
    - [create contact](#create-contact)
      - [CVR / Vat Number Indication](#cvr--vat-number-indication)
      - [Forced and Smart Contact Creation](#forced-and-smart-contact-creation)
      - [Address Handling](#address-handling)
      - [create contact request](#create-contact-request)
      - [create contact response](#create-contact-response)
    - [check contact](#check-contact)
      - [check contact request](#check-contact-request)
      - [check contact response](#check-contact-response)
    - [info contact](#info-contact)
      - [info contact request](#info-contact-request)
      - [info contact response](#info-contact-response)
    - [update contact](#update-contact)
      - [update contact request](#update-contact-request)
      - [update contact response](#update-contact-response)
    - [delete contact](#delete-contact)
      - [delete contact request](#delete-contact-request)
      - [delete contact response](#delete-contact-response)
    - [transfer contact](#transfer-contact)
  - [Host](#host)
    - [create host](#create-host)
      - [create host request](#create-host-request)
      - [create host response](#create-host-response)
      - [create host request with request to new administrator](#create-host-request-with-request-to-new-administrator)
      - [create host response from request to new administrator](#create-host-response-from-request-to-new-administrator)
      - [Delayed create host response, from request to new administrator](#delayed-create-host-response-from-request-to-new-administrator)
      - [create host request, with request to registrant of host domain name](#create-host-request-with-request-to-registrant-of-host-domain-name)
      - [create host response, from request to registrant of domain name](#create-host-response-from-request-to-registrant-of-domain-name)
      - [Delayed create host response, from request to registrant of domain name](#delayed-create-host-response-from-request-to-registrant-of-domain-name)
    - [check host](#check-host)
      - [check host request](#check-host-request)
      - [check host response](#check-host-response)
    - [info host](#info-host)
      - [info host request](#info-host-request)
      - [info host response](#info-host-response)
    - [update host](#update-host)
      - [process](#process)
      - [Change hostname sub-process](#change-hostname-sub-process)
      - [Add IP Address sub-process](#add-ip-address-sub-process)
      - [Remove IP Address sub-process](#remove-ip-address-sub-process)
      - [Change admin sub-process](#change-admin-sub-process)
      - [update host request with request to new administrator](#update-host-request-with-request-to-new-administrator)
      - [update host response with request to new administrator](#update-host-response-with-request-to-new-administrator)
      - [Delayed update host response from request to new administrator](#delayed-update-host-response-from-request-to-new-administrator)
    - [delete host](#delete-host)
      - [delete host request](#delete-host-request)
      - [delete host response](#delete-host-response)
- [Data Collection Policy](#data-collection-policy)
  - [Access](#access)
  - [Purpose Statement](#purpose-statement)
  - [Recipient Statement](#recipient-statement)
  - [Retention Statement](#retention-statement)
- [References](#references)
- [Resources](#resources)
  - [XSD/XML Schemas](#xsd-xml-schemas)
  - [Mailing list](#mailing-list)
  - [Issue Reporting](#issue-reporting)
  - [Demo/Test Client](#demotest-client)
  - [Additional Information](#additional-information)
- [Appendices](#appendices)
  - [Greeting](#greeting)
  - [Status Codes](#status-codes)
    - [Domain Status Codes](#domain-status-codes)
    - [Contact Status Codes](#domain-status-codes)
    - [Host Status Codes](#domain-status-codes)
  - [Privilege Matrix for Registrant Managed Objects](#privilege-matrix-registrant-managed-objects)
  - [Privilege Matrix for Registrar Managed Objects](#privilege-matrix-registrar-managed-objects)
  - [Compatibility Matrix](#compatibility-matrix)

<!-- /MarkdownTOC -->

<a id="introduction"></a>
## Introduction

This document describes and specifies the implementation offered by DK Hostmaster for interaction with the central registry for the ccTLD dk using the Extensible Provisioning Protocol (EPP). It is primarily aimed at a technical audience, and the reader is required to have prior knowledge of DNS registration and administration and EPP.

<a id="about-this-document"></a>
### About this Document

This specification describes version 4.X.X of the DK Hostmaster EPP Implementation. Future releases will be reflected in updates to this specification, please see the [Document History](#document-history) section below.

The document describes the current DK Hostmaster EPP implementation, for more general documentation on the EPP protocol, EPP client development or configuration, please refer to the RFCs and additional resources in the [References](#references) and [Resources](#resources) chapters below.

Do note that the specification aims to describes the latest release of the service. Service version is listed in the [Document History](#document-history),
so given changes implemented in the service are reflected in the specification. Do note that a service might be released to the sandbox environment
prior to being released to production after a grace period.

This document is not the authoritative source for business and policy rules and possible discrepancies between this an any authoritative sources are regarded as errors in this document. This document is aimed at being the external technical specification and describes the implementation facing the users and is an interpretation of authoritative sources and can therefor be erroneous.

The actively used XSD file is indicated in the [EPP service specification][DKHMEPPWIKI], the [EPP XSD file repository][DKHMXSD] might contain changes not actively used by the service.

The current service version can be obtained from the [Greeting](#greeting) message, from the service.

Any future extensions and possible additions and changes to the implementation are not within the scope of this document and will not be discussed or mentioned throughout this document.

The specification mentions the **Registrar Portal** service (RP), which complements the EPP service for consistency between services.

This document is owned and maintained by DK Hostmaster A/S and must not be distributed without this information.

All examples provided in the document are fabricated/modified from real data to demonstrate commands etc. any resemblance to actual data are coincidental.

<a id="license"></a>
### License

This document is copyright by DK Hostmaster A/S and is licensed under the MIT License, please see the separate LICENSE file for details.

<a id="document-history"></a>
### Document History

- 4.0 2021-04-08
  - Introduction of support for registrar/registrant administration
  - Outlined business rules for [`dkhm:orderconfirmationToken`](#dkhmorderconfirmationtoken)
  - The procedures for renewal and application/creation are not being changed, in regard to use and protocol, however
    - The business policies in relation to these operations, do however change, since the billing operation changes, please see the [create domain](#create-domain) and [renew domain](#renew-domain) commands
    - The introduction of registrar support influences the business rules for [create domain](#create-domain)
  - Added information on setting/unsetting autorizations using AuthInfo tokens, see [setting AuthInfo](#setting-AuthInfo) and [unsetting AuthInfo](#unsetting-AuthInfo)
  - Added information on `dkhm:management` extension for [create domain](#create-domain) and [create contact](#create-contact), which overrides account default
  - Added description of new and improved change name server process, both using authorisation and under registrar administration
  - Added documentation on the extension to [info domain](#info-domain) with information on the `AuthInfo` expiration date using the `dkhm::authInfoExDate` extension
  - Added description of the changed [create contact](#create-contact) process, handling registrar administration
  - Introduction of support for [delete domain](#delete-domain) command
  - Introduction of support for [restore domain](#restore-domain) via [update domain](#update-domain) command
  - Introduction of support for [balance](#balance-and-prepaid-account) command
  - Introduction of support for [transfer domain](#transfer-domain) command
  - Added clarifications on status codes for domains, contacts and hosts
  - Removed XSD Version History, referencing original source in [EPP XSD repository][DKHMXSD], so there is a single source for this information
  - This version of the specification is based on the DK Hostmaster EPP XSD version 4.3
  - Addition of disclaimer, setting the scope and frame for this specification

- 3.9 2020-10-19
  - Added some details on sessions in the section on [login](#login)

- 3.8 2020-04-21
  - Aligned XSD versions in all examples to version 3.0

- 3.7 2020-04-04
  - Revised information on IP whitelisting

- 3.6 2020-01-23
  - Cleaned and aligned XSD versions in all examples, currently using version 2.6
  - Added clarifications and examples on advisories extension, used in [info domain](#info-domain)

- 3.5 2020-01-09
  - Updated with information on XSD version 3.0
  - Introduced in release 3.4.0 of the EPP service

- 3.4 2019-10-18
  - Clarified [renew domain](#renew-domain)
  - Correction/clarification on ownership of DSRECORDS for [create domain](#create-domain) and [update domain](#update-domain), in the general section on [DNSSEC](#dnssec)
  - Clarification on status of DNSSEC for [create domain](#create-domain) and [update domain](#update-domain), in the general section on [DNSSEC](#dnssec)

- 3.3 2019-09-09
  - Update to [info domain](#info-domain), example response updated, it now contains DNSSEC data
  - Added clarifications on DNSSEC data availability, even though it is limited currently
  - Added information on the EPP Service Specification Wiki for operation reference information
  - Corrected spelling errors and other minor issues

- 3.2 2019-07-29
  - Clarification to [create contact](#create-contact), removed obsolete description of attention field rule for registrants

- 3.1 2019-05-07
  - Minor update documenting changes introduced in release 3.2.X of the EPP service
  - Update to [info domain](#info-domain) extended with registrant validation status
  - Update to [info contact](#info-contact) extended with e-mail address for the contact object, where applicable
  - Update to [info domain](#info-domain) extended with DNSSEC information
  - Update to [create contact](#create-contact) more strict handling of VAT numbers, see table specifying the use of the field in: [CVR / Vat Number Indication](#cvr--vat-number-indication)
  - Update to [update domain](#update-domain) documentation on deletion and addition of DNSSEC records

- 3.0 2019-04-30
  - Major update based on the changes with major release 3.X.X of the EPP service
  - Documenting removal of public access to information on non-registrant users for contact objects (users)
  - Documenting removal of public access to information on name server contacts for host objects (name servers)

- 2.16 2019-01-11
  - Updated [info domain](#info-domain) to capability to provide information on the billing contact, when applicable

- 2.15 2018-12-03
  - Added information on the new consolidated sandbox environment
  - Corrected some spelling and grammatical errors

- 2.14 2018-11-21
  - Updated information on sandbox environment, latest changes to domain creation emulation had not been added

- 2.13 2018-11-08
  - Updated description of risk assessment under [create domain](#create-domain)

- 2.12 2018-10-29
  - Updated process diagram for [update domain](#update-domain)
  - Added missing process diagram for command evaluation for [update domain](#update-domain)

- 2.11 2018-10-23
  - Added more information on the rules and errors codes related to [renew domain](#renew-domain)

- 2.10 2018-10-08
  - Added more information on [create host](#create-host) command and the use of extension versus authentication for specification of name server administrator.

- 2.9 2018-10-03
  - Added diagram for contact creation revision 1.0, please see the [create contact](#create-contact) command section

- 2.8 2018-09-18
  - Removed pointers to the decommissioned pre-activation service and pre-activation service specification

- 2.7 2018-09-11
  - Minor correction, to the `reason` (status) for domains offered from waiting list, since the `reason` did not comply with the XSD definition. The `reason` is corrected in EPP service version: EPP 2.4.2

- 2.6 2018-08-22
  - Describes EPP service 2.4.X
  - Added information on `reason` (status) `enqueued` for check domain command
  - Added information on silenced out of band communication for change of billing contact for a domain

- 2.5 2018-06-22
  - Updated XSD history and information on XSD version 2.4
  - Added information on service and specification versions and retrieving of version information from the service
  - Added examples of poll messages related to domain creation

- 2.4 2018-05-25
  - Added information on format of the `orderconfirmation` Token, this is implemented with EPP release 2.3.0 currently only available in sandbox and introduces the new extension: `dkhm:url`
  - Addition of risk assessment for [create domain](#create-domain) command poll response. The XSD files revision 2.2 describes the changes to the XSD and supports the new extension: `dkhm:risk_assessment`

- 2.3 2018-05-01
  - Updated XSD history
  - Added diagram for [create domain](#create-domain)

- 2.2 2017-12-19
  - Removed information on status blocked, which has been deprecated

- 2.1 2017-06-08
  - Removed information on waiting list handling, since this is being revisited

- 2.0 2016-10-24
  - Describes EPP service 2.X.X
  - Added renew domain description
  - Added [update domain](#update-domain) description
  - Added create/update/delete host descriptions
  - Added update contact description
  - Added XSD 2.0 description

- 1.10 2016-06-08
  - Added information on IP whitelisting

- 1.9 2016-01-30
  - Information on new waiting list handling
  - Information on new DNSSEC key handling

- 1.8 2015-09-03
  - Minor corrections
  - More information on extensions for possible registration of the DK Hostmaster extensions with IANA in relation to [RFC:7451]
  - Added [RFC:7451] compliant descriptions in subdirectory: `rfc7451/`

- 1.7 2015-05-12
  - This revision of the specification is describing EPP service release 1.3.X
  - This release also updates the [XSD][DKHMXSD] specification to revision 1.4, introducing the extension `pnumber` for transport of production unit numbers for validation of Danish companies as part of the [create contact](#create-contact) command

- 1.6 2015-01-06
  - This revision of the specification is describing EPP service release 1.2.X
  - This release also updates the [XSD][DKHMXSD] specification to revision 1.3
  - The document has with this revision been ported from a proprietary format to markdown and is being hosted on GitHub for easier maintenance and distribution, this has resulted in a lot of minor corrections and clarifications.
  - Extended the section about this document, due to the migration to GitHub, so copyright is now explicitly mentioned
  - [info contact](#info-contact) command extended with validation information
  - [create domain](#create-domain) command extended with validation information for registrant
  - [create domain](#create-domain) command extended with information on confirmation status for domain

- 1.5 2014-06-18
  - This revision of the specification is describing EPP service release 1.1.X
  - The test environment is no longer active
  - Examples updated to latest [XSD][DKHMXSD] revision (1.2)
  - Pre-activation token (`orderconfirmationToken`) can be transported via extension for [create domain](#create-domain) command

- 1.4 2013-11-19
  - Corrected links in resources
  - Emphasized the use of the `auto` keyword for contact creation, this has also been listed in the implementation limitations section
  - Added information on the restrictive use of `clTRID` in new section entitled: Implementation Requirements

- 1.3 2013-10-29
  - This revision of the specification is describing EPP service release 1.0.9
  - Added information on use of `clTRID` in context of [create domain](#create-domain) command
  - Added more information on the domain check command, which has been extended with EPP service release 1.0.9.
  - This release also updates the [XSD][DKHMXSD] specification to revision 1.1

- 1.2 2013-08-07
  - This revision of the specification is describing EPP service release 1.0.8
  - Added note on domain check

- 1.1 2013-05-31
  - Added paragraph on passwords in section on the login command
  - Added mention of standard port 700
  - Corrected some of the XML examples, which had not been updated to reflect the correct use of [XSDs][DKHMXSD]
  - Added important note on contact creation

- 1.0 2013-02-25
  - Initial revision
  - Describes EPP service 1.X.X
  - Introduces [XSD][DKHMXSD] specification revision 1.0

<a id="the-dk-registry-in-brief"></a>
## The .dk Registry in Brief

DK Hostmaster is the registry for the ccTLD for Denmark (dk), with DK Hostmaster maintaining the central DNS registry.

The legislation and registry model utilized in Denmark imposes some limitations compared to the general scope of the EPP protocol. These limitations are described in detail below in the chapter entitled Implementation Limitations, and these are explained further in the command descriptions where the single commands deviate from the EPP standard specification. In addition to limitations and deviations in the mentioned sections, a few others have been implemented to support DNS registration under Danish legislation, these are described in detail under the individual commands where relevant.

DK Hostmaster offer two administration models, please see [the complete description of the concept][CONCEPT] of the administration models offered by DK Hostmaster A/S for details.

In brief the two models are:

- Registrar managed, where the registrar takes the administrative role of the domain name and administers the assets with DK Hostmaster "registrar management"
- Registrant managed, where the registrant administers the assets directly with DK Hostmaster, also referred to as: "registrant management"

The EPP service is the same, but the capabilities and business roles vary depending on choice of administrative model. This is described in detail under the different commands and outlined in:

- [Privilege Matrix for Registrant Managed Objects](#privilege-matrix-registrant-managed-objects)
- [Privilege Matrix for Registrar Managed Objects](#privilege-matrix-registrar-managed-objects)

Our EPP extensions are registered with the [IANA EPP Extension Repository][IANA] as by [RFC:7451].

<a id="epp-in-brief"></a>
## EPP in Brief

EPP is an XML-based protocol aimed at provisioning data between registries. The protocol is intended for machine-to-machine communication in a client-server setup. Please see the [References](#references) chapter for more information on specifications and references for EPP.

Please note that the service does not support XML entity expansion on the server side, due to security implications related to this feature.

<a id="epp-service"></a>
## EPP Service

The DK Hostmaster’s EPP Service is based on an SOA architecture. EPP implementation is regarded as a service offered to external parties requiring provisioning actions towards DK Hostmaster.

The EPP service requires the use of and possible development of EPP client software. This is beyond the scope of this specification as the API and other assets for assisting in this are the primary object of this document.

In addition to the assets, DK Hostmaster aims to assist users and developers of EPP client software with integration towards DK Hostmaster and therefore provide facilities to ease this integration. This is primarily centered around a sandbox environment and related documentation.

The service is implemented under the following principles:

1 Adhere to the standard to the extent possible or use non-intrusive extensions to support the requirements or finally use mandatory extensions to adhere to service requirements
1 Use _in-band_ communication, meaning requests made via EPP will be responded to via EPP unless the end-user have specified differently
1 Use standard error codes to the extent possible, communicating state more clearly and unambiguously

<a id="ssltls-support"></a>
### SSL/TLS Support

The EPP service supports the following protocols for transport security:

- TLSv1.2

<a id="available-environments"></a>
### Available Environments

DK Hostmaster offers the following two environments:

- production
- sandbox

Updates to both environments are announced via the tech-announce mailing list.

Please see the [information page][DKHMMAIL] for details on subscribing etc.

<a id="production"></a>
#### Production

- Please see [EPP service specification Wiki][DKHMEPPWIKI] for up to date information for the production environment accessible at: `epp.dk-hostmaster.dk` on port: `700`

- This environment is the production environment
- info and check requests made to this environment will reflect live production data
- create requests made to this environment will be carried out provided that they comply with business rules and general terms
- Approved domains will be processed for possible activation and propagation into the zone
- Contacts (users) will be created and will be available in other systems like the self-service system etc.
- Hosts (name servers) will be processed for possible activation
- The Change Password operation is available in this environment
- Please note that this operation will change the password and this change will be reflected in other systems
- This is environment is using [IP Whitelisting](#ip-whitelisting)
- This environment is only available to registrars

<a id="sandbox"></a>
#### Sandbox

- Please see [EPP service specification Wiki][DKHMEPPWIKI] for up to date information for the production environment accessible at : `epp-sandbox.dk-hostmaster.dk` on port: `700`

- This environment is intended for client development towards the DK Hostmaster EPP service
- info and check requests made to this environment will reflect sandbox data. For host objects, some static content synched in by DK Hostmaster, in addition to sandbox data
- create requests made to this environment will be serialized in the sandbox environment, provided that syntax and data are valid
- Domains will be enqueued and are processed for possible activation, responses are reflected in messages available for polling, propagation into a zone file is not supported
- Contacts (users) can be created and will only be available in the sandbox system

- The Change Password operation will only change the password in the sandbox environment
- This environment is available to both registrars and name server administrators

Please note that when you first start to use the EPP sandbox environment, the access credentials are matching your production credentials. If these do not work as expected (e.g. error `2200`). please contact: DK Hostmaster to get the credentials synchronized.

For more information on the consolidated sandbox environment please see [the specification][DKHMSANDBOX].

<a id="implementation-requirements"></a>
## Implementation Requirements

This section outlines the overall requirements in regard to implementing an EPP client to work with the DK Hostmaster EPP service.

<a id="client-transaction-id-cltrid"></a>
### Client Transaction ID (`clTRID`)

In order to ensure transactional integrity and due to the asynchronous nature of some of the EPP commands, we rely on the client transaction id to be unique. This is unique as per client id. The assists in ensuring that a delayed response can be easily identified by simple means.

The `clTRID` is recommended to be unique for all transactions and is required to be unique for the [create domain](#create-domain) command.

<a id="ip-whitelisting"></a>
### IP Whitelisting

Access to the EPP service production environment requires IP whitelisting of IP addresses.

Maintenance of IP addresses is done in the **Registrar Portal** (RP) and requires an active registrar account for access.

<a id="implementation-extensions"></a>
## Implementation Extensions

The EPP service implemented by DK Hostmaster holds several extensions, these are documented where appropriate for the specific commands etc. This section serves to give an overview of the extensions as a whole.

Please refer to the [EPP XSD file repository][DKHMXSD] for implementation details.

Here follows a list of the extensions in alphabetical order. All are described separately and in detail below.

- `dkhm:authInfoExDate`
- `dkhm:autoRenew`
- `dkhm:delDate`
- `dkhm:contact_validated`
- `dkhm:CVR`
- `dkhm:domain_confirmed`
- `dkhm:domainAdvisory`
- `dkhm:EAN`
- `dkhm:management`
- `dkhm:mobilephone`
- `dkhm:orderconfirmationToken`
- `dkhm:pnumber`
- `dkhm:registrant_validated`
- `dkhm:requestedNsAdmin`
- `dkhm:risk_assessment`
- `dkhm:secondaryEmail`
- `dkhm:trackingNo`
- `dkhm:url`
- `dkhm:userType`

<a id="dkhmauthinfoexdate"></a>
### `dkhm:authInfoExDate`

This extension is used to expose the expiration date for a `AuthInfo` token if set for a domain name.

Please see:

- the [info domain](#info-domain) command
- [section on AuthInfo token format](#authinfo-token-format)

<a id="dkhmautorenew"></a>
### `dkhm:autoRenew`

This extension is used to expose the auto-renewal flag for a domain name.

Please see:

- the [info domain](#info-domain) command
- the [create domain](#create-domain) command
- the [update domain](#update-domain) command

The choice of renewal policy is based on the default for the specific registrar account. The default can be overridden per request or application using the extension: `dkhm:autoRenew`, which support two values:

- `true`, indicating auto-renewal
- `false`, indicating auto-expire

The default for a registrar account is auto-renewal. A new default can be set in the registrar portal.

<a id="dkhmcontact_validated"></a>
### `dkhm:contact_validated`

Contact objects related to the role of registrant has to be validated, this field is used to indicate the status of a validation of a contact object via the [info contact](#info-contact) command.

<a id="dkhmcvr"></a>
### `dkhm:CVR`

The CVR extension is for transporting VAT registration numbers. The number is used for validation and VAT accounting. More information is available under the [create contact](#create-contact) command.

<a id="dkhmdeldate"></a>
### `dkhm:delDate`

```xml
<extension>
    <dkhm:delDate xmlns:dkhm="urn:dkhm:xml:ns:dkhm-4.3">2021-01-31T00:00:00.0Z</dkhm:delDate>
</extension>
```

Please see:

- the [info domain](#info-domain) command
- the [delete domain](#delete-domain) command

<a id="dkhmdomain_confirmed"></a>
### `dkhm:domain_confirmed`

Domain names registered with DK Hostmaster, has to be confirmed by the registrant, this is can either be done using pre-application agreement to terms, see the [`dkhm:orderconfirmationToken`](#dkhmorderconfirmationtoken) extension or other systems with DK Hostmaster. The domain confirmation process is handled via via the [create domain](#create-domain) command using this extension.

<a id="dkhmdomainadvisory"></a>
### `dkhm:domainAdvisory`

Any special circumstances in relation to a domain name, can be communicated using this special field. Please see [info domain](#info-domain).

Currently two advisories are communicated:

- `pendingDeletionDate`, indicating that a given domain name is scheduled for deletion
- `offeredOnWaitingList`, indicating that a given domain name has been offered to a designated registrant

<a id="dkhmean"></a>
### `dkhm:EAN`

The EAN extension, holds the [EAN number][EAN] associated with public organizations in Denmark. The field is mandatory for this type of contact objects and is required for electronic invoicing, more information is available under the [create contact](#create-contact) command.

<a id="dkhmmanagement"></a>
### `dkhm:management`

The choice of administration model is based on the default for the specific registrar account. The default can be overridden per request or application using the extension: `dkhm:management`, which support two values:

- `registrar`, indicating registrar management
- `registrant`, indicating registrant management

The extension can be used with the commands:

- [create domain](#create-domain)
- [create contact](#create-contact)

If not specified the default of the registrar account will be used. The default can be set in the registrar portal.

To change the management model for an existing domain, please see the [transfer domain](#transfer-domain) command.

<a id="dkhmmobilephone"></a>
### `dkhm:mobilephone`

Contact objects can have a mobile phone number in addition to `voice` and `fax`.

<a id="dkhmorderconfirmationtoken"></a>
### `dkhm:orderconfirmationToken`

This is a special field for supporting the business flow where the agreement for a domain name is accepted by the registrant with the registrar. More information is available under the [create domain](#create-domain) command.

See also: [`dkhm:domain_confirmed`](#dkhmdomain_confirmed)

<a id="dkhmpnumber"></a>
### `dkhm:pnumber`

The p-number extension is for holding production-unit numbers, used for validation for Danish companies, with more physical addresses related to a single VAT number. More information is available under the [create contact](#create-contact) command.

See also: [`dkhm:CVR`](#dkhmcvr)

<a id="dkhmregistrant_validated"></a>
### `dkhm:registrant_validated`

Contact objects related to the role of registrant has to be validated, this field is used to indicate the status of a validation object via the [create domain](#create-domain) command.

See also [`contact_validated`](#dkhmcontact_validated).

<a id="dkhmrequestednsadmin"></a>
### `dkhm:requestedNsAdmin`

The extension is used for [update host](#update-host) and [create host](#create-host), where it is possible to request another name server administrator than the authenticated user.

<a id="dkhmrisk_assessment"></a>
### `dkhm:risk_assessment`

This extension is used in the poll response in relation to domain creation. The extension provides information on the risk assessment made by DK Hostmaster.

Please see the [create domain](#create-domain) command.

<a id="dkhmsecondaryemail"></a>
### `dkhm:secondaryEmail`

Contact objects can have a secondary email address in addition to `email`.

<a id="dkhmtrackingno"></a>
### `dkhm:trackingNo`

A unique tracking number for a domain registration for uniformity with RP. EPP it not the only channel of domain registration and in order to handle registrations via multiple channels, a unique tracking-id is assigned to every request. More information is available under the [create domain](#create-domain) command.

<a id="dkhmurl"></a>
### `dkhm:url`

This extension can be used to redirect an end-user to the next step. For now it is used in relation to domain creation, where the user can be directed to the next step if this is handled by DK Hostmaster. More information is available under the [create domain](#create-domain) command.

<a id="dkhmusertype"></a>
### `dkhm:userType`

The `userType` extension is used to categorize a contact type, since the requirements for data differs between the different user types, we need to be able to differentiate between:

- company
- individual
- public organization
- association

More information is available under the [create contact](#create-contact) command.

Related extensions are [`dkhm:EAN`](#dkhmean), [`dkhm:CVR`](#dkhmcvr) and [`dkhm:pnumber`](#dkhmpnumber).

<a id="implementation-limitations"></a>
## Implementation Limitations

As mentioned the EPP service comes with some limitations. Please see the [Compatibility Matrix](compatibility-matrix) in the appendices for a high-level overview.

<a id="unimplemented-commands"></a>
### Unimplemented Commands

The following commands have not been implemented in the service described in this specification:

- `delete` contact
- `transfer` contact

In general the service is not localized and all EPP related errors and messages are provided in English.

<a id="authentication"></a>
### Authentication

The DK Hostmaster EPP service, only support username/password authentication.

See also the [login](#login) command for details.

<a id="authinfo"></a>
### AuthInfo

`AuthInfo` in limited to providing authorizations for 3rd. parties.

It is currently supported for the following commands:

- [update domain](#update-domain), for use for changing name servers
- [transfer domain](#transfer-domain), for use for changing registrar
- [create domain](#create-domain), for use for registering a domain name offered from a waiting list

Please see the command descriptions for more details.

For registration of domain names offered from a waiting list, the `AuthInfo` field is utilized for the [create domain](#create-domain), the format of the token is however in this context is simpler.

Specifying `AuthInfo` for a [contact create](#contact-create) has no effect and it is not recommended to disclose this information in this command.

From [RFC:5733]:

> A <contact:authInfo> element that contains authorization
> information to be associated with the contact object.  This
> mapping includes a password-based authentication mechanism, but
> the schema allows new mechanisms to be defined in new schemas.

The element is not optional, but mandatory, it should not be populated with sensitive information.

<a id="dnssec"></a>
### DNSSEC

I accordance with [RFC:5910]. We support DS only and not DNSKEY. In addition the maximum signature lifetime (`secDNS:maxSigLife`) is disregarded. See [section 3.3][RFC:5910-3.3]) in the referenced RFC.

DK Hostmaster specifies rules ownership of DNSSEC keys. If you provide DNSSEC keys a part of registration using [create domain](#create-domain) or later updating using [update domain](#update-domain), the keys are associated with the NSA as owner.

Not all algorithms are supported, please refer to the [DK Hostmaster Name Service specification][DKHMDNSSPEC] for a complete list of supported algorithms.

When adding DSRECORDS for a domain name using [create domain](#create-domain) or [update domain](#update-domain), the status is by default active and the DSRECORDS will be published. The DNSSEC status feature for the domain name can prohibit the publication, this feature is available only to the registrant for the specific domain and act as a kill switch for use by the registrant in case of issues with DNSSEC.

Availability of DNSSEC information and status is currently limited to public available data.

In addition with the improvements to the process for change of name servers using [update domain](#update-domain), all current DSRECORDS are deleted as part of this operation.

<a id="contact-creation"></a>
### Contact Creation

This command does not support the feature of providing a predefined contact-id. The contact-id has to be specified as `auto` and the contact-id is assigned by DK Hostmaster. See also information on the [create contact](#create-contact) command.

Due to a limitation in the AAA system implemented by DK Hostmaster, it is currently not possible to see contact objects using [info contact](#info-contact), if these are not registrants. This is regarded as a temporarily limitation, which will be addressed at some point in the future. The recommendation is to use [check contact](#check-contact).

REF: [issue #34](https://github.com/DK-Hostmaster/epp-service-specification/issues/34)

<a id="host-update"></a>
### Host Update

This command does not support the setting and removal of status using the XML element: `host:status`. The status is assigned by DK Hostmaster. See also information on the [update host](#update-host) command.

<a id="domain-update"></a>
### Domain Update

This command does not support the change of the registrant and the setting and removal of status using the XML element: `domain:status`. The status is assigned by DK Hostmaster. See also information on the [update domain](#update-domain) command.

<a id="host-info"></a>
### Host Info

The command [info host](#info-host) will only supply the name server administrator/zone contact information if the requesting and authenticated user has a relationship to the user, either via a domain role or registrar group, which provides authorization to access the information.

<a id="waiting-list"></a>
## Waiting List

DK Hostmaster offers a waiting list service for domain names, when a domain name becomes available to the first position on a waiting list, it should be registered using the standard registration process either via RP or EPP. This influences the [create domain](#create-domain) command, which should just be populated with the token issued by DK Hostmaster authorizing registration

Alternatively the user-id of the waiting list user which has been pre-approved for registration of the domain name with DK Hostmaster.

No other information is available on waiting lists via EPP.

Please refer to the DK Hostmaster [website][DKHMWAITLIST] for more information.

<a id="contact-info"></a>
### Contact Info

The command [info contact](#info-contact) will only supply the registrant information. For other contact objects, only if the requesting user and authenticated has a relationship to the designated contact object, either via a host or domain role or registrar group.

<a id="domain-info"></a>
### Domain Info

The command [info domain](#info-domain) will only supply the registrant information for relevant contact objects. For other contact objects assigned to the domain name, the requesting user has to have a relationship to the domain or contact object, either via a host or domain role or registrar group.

Availability of DNSSEC information and status is currently limited to public available data.

<a id="information-disclosure"></a>
### Information Disclosure

Please note that some information is not disclosed when using Object Query Commands. See the specific commands for more information.

Additionally DK Hostmaster does not implement the optional `contact:disclose` element.

> An OPTIONAL <contact:disclose> element that allows a client to
> identify elements that require exceptional server-operator
> handling to allow or restrict disclosure to third parties.  See
> Section 2.9 for a description of the child elements contained
> within the <contact:disclose> element.

From [RFC:5733].

<a id="encoding-and-idn-domains"></a>
### Encoding and IDN domains

DK Hostmaster supports IDN domain names and the EPP commands support Punycode notation for this in requests. DK Hostmaster does not support Punycode notation in responses at this time.

For details on supported characters, please see: [the DK Hostmaster Name Service specification][DKHMDNSSPEC].

<a id="disclosure-of-client-id"></a>
### Disclosure of Client ID

As specified in [RFC:5731], [RFC:5732] and [RFC:5733] the info commands all display a reference sponsoring entity.

The same scheme will be implemented in RP and the end-user self-service portal (SB) the data presentation is consistent across all portals.

The public facing interface is expected to present the registrar relation as well. Meaning that the information on registrar relation will be made available in:

- in WHOIS, see [DK Hostmaster WHOIS Service Specification][DKHMWHOISSPEC]
- on www.dk-hostmaster.dk, see - [DK Hostmaster RESTful WHOIS Service Specification][DKHMWHOISRESTSPEC]

<a id="unsupported-domain-status-codes"></a>
### Unsupported Domain Status Codes

Several of the domain status codes described in [RFC:5731] and the [ICANN status code description][ICANN] are not supported.

All of the `client*` status codes are note supported:

- `clientDeleteProhibited`
- `clientHold`
- `clientRenewProhibited`
- `clientTransferProhibited`
- `clientUpdateProhibited`

The administrative model does not support user enforced restraints.

- `addPeriod`
- `autoRenewPeriod`
- `renewPeriod`
- `transferPeriod`

Are all ICANN statuses and are not regarded as standard and they do not map to business rules used in the DK Hostmaster registry system, based on the descriptions in the [ICANN status code description][ICANN].

- `pendingRenew`
- `pendingRestore`
- `pendingTransfer`

The operations for renew, restore and transfer are instantaneous and the listed pending-states do therefor not map to business processes used in the DK Hostmaster registry system.

- `inactive`

This state is unsupported, since domain names in the DK Hostmaster registry **must** have associated name servers.

The [domain status codes listing](#domain-status-codes) holds a complete listing of all the status codes.

<a id="unsupported-contact-status-codes"></a>
### Unsupported Contact Status Codes

Several of the host status codes described in [RFC:5733] are not supported.

All of the `client*` status codes are note supported:

- `clientDeleteProhibited`
- `clientUpdateProhibited`
- `clientTransferProhibited`

The administrative model does not support user enforced restraints.

- `pendingCreate`
- `pendingDelete`
- `pendingTransfer`

The operations for renew, restore and transfer are instantaneous and the listed pending-states do therefor not map to business processes used in the DK Hostmaster registry system.

<a id="unsupported-host-status-codes"></a>
### Unsupported Host Status Codes

Several of the host status codes described in [RFC:5732] are not supported.

All of the `client*` status codes are note supported:

- `clientDeleteProhibited`
- `clientUpdateProhibited`

The administrative model does not support user enforced restraints.

- `pendingTransfer`

The operations for transfer are instantaneous and the listed `pendingTransfer` do therefor not map to business processes used in the DK Hostmaster registry system.

<a id="supported-object-transform-and-query-commands"></a>
## Supported Object Transform and Query Commands

The following section describes the currently supported EPP commands. As mentioned previously, some of the commands have been extended beyond the basic capabilities of EPP. These extensions are described separately under each command and are included in the [DKHMXSD][DKHMXSD] listed in the [Resources](#resources) chapter.

The supported commands are:

- `hello`
- `login`, including change password
- `logout`
- `create` (contact/domain/host)
- `check` (contact/domain/host)
- `info` (contact/domain/host)
- `update` (contact/domain/host)
- `renew` (domain)
- `transfer` (domain)
- `delete` (host/domain)
- `poll`, including acknowledgement of messages
- `balance` command extension
- `restore` extension to update domain command

Commands that have not been extended are not described in much detail, please refer to the general EPP documentation from IETF (see: the RFCs listed in [References](#references)).

<a id="hello-and-greeting"></a>
### hello and greeting

This part of the EPP protocol is described in [RFC:5730]. This command adheres to the standard. For a more detailed explanation of the data collection policy announced via the greeting, please see the [Data Collection Policy](#data-collection-policy) chapter.

As announced in the [greeting](#greeting), the following objects are available:

- `Host`, see also the chapter on [Host assets](#host)
- `Domain`, see also the chapter on [Domain assets](#domain)
- `Contact`, see also the chapter on [Contact assets](#contact)

With regard to extensions, the following are available:

- [secDNS-1.1][DKHMXSD]
- [dkhm-4.3][DKHMXSD]

Please see the greeting response included in the [appendices](#greeting) for illustration of the actual announcement.

<a id="login"></a>
### login

This part of the EPP protocol is described in [RFC:5730]. This command adheres to the standard.

The login uses the general Authentication Authorization and Access (AAA) framework in DK Hostmaster. This mean that in addition to the validation of username and password specified as part of the login request, an attempt is made to authorize the authenticated user for access to the actual EPP service and subsequent operations.

Authorization is currently only available to specified user roles, therefore the username provided must point to an entity with the role of registrar or name server administrator with the DK Hostmaster registry. See also Available Environments above.

DK Hostmaster supports the change of passwords via EPP. Please refer to the chapter Available Environments for any special circumstances.

Password should adhere to the following requirements:

EPP supports a password with at least 6 and max 16, where DK Hostmaster supports 8 - 64 characters. The password must include at least three of these four character types:

- Lower-case letters
- Upper-case letters
- Numbers
- Special Characters

The following characters are legal special characters in passwords:

```text
% ` ' ( ) * + - , . / : ; < > = ! _ & ~ { } | ^ ? $ # @ " [ ]
```

Currently, the only language supported is English. So the language parameter is ignored and all responses are provided in English.

Successful authentication established a session with a life span of 700 seconds (5 minutes), it can be kept alive by sending additional `hello` commands or similar.

The overall life span is 28800 seconds (8 hours) after this the session is terminated and should be reestablished with a new authentication (`login`).

<a id="login-request"></a>
#### login request

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<command>
		<login>
			<clID>REG-999999</clID>
			<pw>*********</pw>
			<options>
				<version>1.0</version>
				<lang>en</lang>
			</options>
			<svcs>
                <objURI>urn:ietf:params:xml:ns:domain-1.0</objURI>
                <objURI>urn:ietf:params:xml:ns:host-1.0</objURI>
                <objURI>urn:ietf:params:xml:ns:contact-1.0</objURI>
			</svcs>
		</login>
		<clTRID>d52eaf8995d2b679fe9dc53ee5bc3ad9</clTRID>
	</command>
</epp>
```

<a id="login-response"></a>
#### login response

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1000">
			<msg>User REG-999999 logged in.</msg>
		</result>
		<trID>
			<clTRID>d52eaf8995d2b679fe9dc53ee5bc3ad9</clTRID>
			<svTRID>63BE4FAE-F6F9-11E3-867F-A6B052036DCB</svTRID>
		</trID>
	</response>
</epp>
```

<a id="logout"></a>
### logout

This part of the EPP protocol is described in [RFC:5730]. This command adheres to the standard.

There are no special additions or alterations to the specification or use of this command.

<a id="logout-request"></a>
#### logout request

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<command>
		<logout />
		<clTRID>9450488c8280671c051f273285d7bec7</clTRID>
	</command>
</epp>
```

<a id="logout-response"></a>
#### logout response

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1500">
			<msg>User logged out. Closing Connection.</msg>
		</result>
		<trID>
			<clTRID>9450488c8280671c051f273285d7bec7</clTRID>
			<svTRID>370F8F46-F6F3-11E3-867F-A6B052036DCB</svTRID>
		</trID>
	</response>
</epp>
```

<a id="poll-and-message-queue"></a>
### poll and Message Queue

This part of the EPP protocol is described in [RFC:5730]. This command adheres to the standard.

There are no special additions or alterations to the specification or use of this command.

For clarification `2303` is returned in case a provided message-id (`msgID`) point to a non-existing message.

| Return Code | Description                                    |
| ----------- | ---------------------------------------------- |
| 2303        | In case a requested message no longer exist    |
| 1301        | Command completed successfully; ack to dequeue |
| 1000        | A messages was successfully dequeued using ack |

<a id="poll-req-request"></a>
#### poll req request

```XML
<?xml version="1.0" encoding="UTF-8"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<poll op="req"/>
		<clTRID>09ed6c730e5c4c671c69ea8a4325ac06</clTRID>
	</command>
</epp>
```

<a id="poll-req-response"></a>
#### poll req response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="10" id="1">
			<msg>Create domain pending for eksempel.dk</msg>    </msgQ>
		<resData>
			<domain:creData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name>eksempel.dk</domain:name>
				<domain:crDate>2013-02-13T13:43:24.0Z</domain:crDate>
			</domain:creData>
		</resData>
		<trID>
			<clTRID>bb96ddfcbe2becbe1e7d974a5b22e29a</clTRID>
			<svTRID>EFE89190-CC4B-11E6-B51D-4F7D3A107CA1</svTRID>
		</trID>
	</response>
</epp>
```

<a id="poll-ack-request"></a>
#### poll ack request

```XML
<?xml version="1.0" encoding="UTF-8"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<poll msgID="1" op="ack"/>
		<clTRID>a05bd42e77b26fe18801cbf5216ee199</clTRID>
	</command>
</epp>
```

<a id="poll-ack-response"></a>
#### poll ack response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1000">
			<msg>Command completed successfully</msg>
		</result>
		<msgQ count="9" id="2">
		</msgQ>
		<trID>
			<clTRID>770e65ed92827c810421faf709b5523c</clTRID>
			<svTRID>4ECFA0E0-CC4C-11E6-A3CB-78843A107CA1</svTRID>
		</trID></response>
</epp>
```

<a id="poll-ack-response-for-non-existent-message-or-previously-acknowledged-message"></a>
#### poll ack response for non-existent message (or previously acknowledged message)

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="2303">
			<msg>Object does not exist</msg>
		</result>
		<msgQ count="8" id="5">
		</msgQ>
		<trID>
			<clTRID>9bee91be9f7d15808ce3425af406ddc4</clTRID>
			<svTRID>A615AEDA-CC4C-11E6-9191-4F7D3A107CA1</svTRID>
		</trID>
	</response>
</epp>
```

<a id="balance-and-prepaid-account"></a>
### balance and Prepaid Account

With the introduction of the registrar management, a prepaid model for registrars is also put in place.

This mean that EPP commands relating to billable operations are evaluated in context of the registrar account and accounting is completed as part of the execution of the operation.

The operations currently classified as billable are:

- Domain name application/creation, this is described in detail in the [create domain](#create-domain) section
- Domain name renewal. this is described in detail in the [renew domain](#renew-domain) section
- Restoration from deletion, this is described in detail in the [restore domain](#restore-domain) section
- Restoration from suspension, also described in detail in the [restore domain](#restore-domain) section

This mean that a new error scenario is introduced with version 4.0.0 of the service for creation/application, where an application/create request will be declined, in case of insufficient funds. The renewal operation is not subjected to this policy, please refer to the registrar contract for specific details as this is a technical document and not the authoritative source for business and policy rules.

All prices and amounts relating to currencies are provided in DKK, converted to the EPP currency type, using decimal point (`.`) and not decimal comma (`,`), which is the definition for the Danish locale.

<a id="balance-request"></a>
#### balance request

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <command>
        <info>
            <balance:info xmlns:balance="http://www.verisign.com/epp/balance-1.0"/>
        </info>
        <clTRID>ABC-12345</clTRID>
    </command>
</epp>
```

Example lifted from "[Balance Mapping for the Extensible Provisioning Protocol (EPP)][BALANCE]" (see [References](#references)).

<a id="balance-response"></a>
#### balance response

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <response>
        <result code="1000">
            <msg>Command completed successfully</msg>
        </result>
        <resData>
            <balance:infData xmlns:balance="http://www.verisign.com/epp/balance-1.0">
                <balance:creditLimit>1000.00</balance:creditLimit>
                <balance:balance>200.00</balance:balance>
                <balance:availableCredit>800.00</balance:availableCredit>
                <balance:creditThreshold>
                    <balance:fixed>500.00</balance:fixed>
                </balance:creditThreshold>
            </balance:infData>
        </resData>
        <trID>
            <clTRID>ABC-12345</clTRID>
            <svTRID>54322-XYZ</svTRID>
        </trID>
    </response>
</epp>
```

Example lifted from "[Balance Mapping for the Extensible Provisioning Protocol (EPP)][BALANCE]" (see [References](#references)).

| Return Code | Description                                                   |
| ----------- | ------------------------------------------------------------- |
| 2201        | No authorization to view the balance of the requested account |
| 2303        | The account for which balance was requested does not exist    |
| 1000        | Request was responded to successfully                         |

<a id="domain"></a>
### Domain

The default behavior of the EPP [create domain](#create-domain) command as described in [RFC:5731], will attach the client-ID (`CLID`) of the authenticated party to the object created.

Having the client-ID specified at this time indicates that the domain name is under registrar management from creation. To change the registrar and discontinue the registrar management will require a transfer.

For more information on the [disclose of the Client ID](#disclosure-of-client-id)

If the registrar does not want to create domains under registrar management the default behaviour can be configured in RP, where the available are:

- registrar management
- registrant management

The setting will be global and will influence the behaviour in both:

- EPP
- RP

These settings are controlling the account as a whole for all relevant commands.

- `create domain`, this section
- [create contact](#create-contact), described below
- [create host](#create-host), described below

<a id="create-domain"></a>
#### create domain

This part of the EPP protocol is described in [RFC:5730]. This command adheres to the standard. DK Hostmaster, however, is based on an asynchronous domain creation workflow.

All domain requests are enqueued for further processing and their creation will be in a state of pending.

Please note:

- `authInfo` section is not used for transport of end-user passwords, see also section in : [Implementation Limitations](#implementation-limitations) on [authinfo](#authinfo)

Domains offered from a waiting list can be registered using the `create domain` command. It requires the authorization token issued by DK Hostmaster to the designated registrant. The token has to be transported via the `AuthInfo` field.

The `AuthInfo` token used for registration of domain names offered from a waiting list are a 8 digit hexadecimal case insensitive string. The token is offered to the designated registrant out of band and is valid for 14 days.

As described in the section on [waiting lists]("waiting-list) the token is not necessary if the designated registrant for the application use the user-id of the waiting list customer and designated registrant.

A well-formed request for domain creation will always result in:

```text
1001, “Command completed successfully; action pending”
```

The extension in response will provide a unique tracking number, which can be used to identify the creation request across provisioning channels offered by DK Hostmaster. The result of the further processing will be relayed back via EPP, see [Poll and Messages](#poll-and-messages) below.

The customized response for a domain creation request looks as follows:

The [create domain](#create-domain) command has been extended with a field (`orderconfirmationToken`) making it possible to assign a token indicating that the registrant has agreed to the terms and conditions for DK Hostmaster with the registrar.

```xml
<extension>
	<dkhm:orderconfirmationToken xmlns:dkhm=“urn:dkhm:params:xml:ns:dkhm-4.3”>
		1522744544
	</dkhm:orderconfirmationToken>
</extension>
```

The token is a timestamp in [EPOCH] format, indicating when the agreement was accepted. The [EPOCH] timestamp has to be specify adhering to the POSIX/UNIX standard and has to be specified in seconds. We do not support milliseconds or nanoseconds.

The EPOCH timestamp must not exceed 24 hours into the future compared to local time resolution. In case this exception occurs error code `2004` and a descriptive message.

The `token` is handled the following way:

- If absent DK Hostmaster will require the agreement for the terms and conditions be accepted with DK Hostmaster, this process is handled by DK Hostmaster

- If present. The token will be validated by DK Hostmaster
- if not valid the request with result in an error and the request will be dismissed
- if valid the request will be accepted and processed

The requirement for the registrant to be valid is communicated via the response, using the extension: `dkhm:registrant_validated`.

Please see the command [info contact](#info-contact) for more information.

The state is communicated in this response in order to provide information on the further flow and process of the [create domain](#create-domain) request.

An additional URL is specified in the response via the extension `dkhm:url`, this URL can be presented to the end-user for further processing and for the following scenarios in particular:

1. End-user has not agreed to the terms and conditions
2. End-user has agreed to the terms and conditions, but ID-control is required
3. End-user has agreed to the terms and conditions and ID-control has been completed - no further actions are necessary, self-service access is available and active

As part of the process the final response to a [create domain](#create-domain) is communicated via the message queue. In this response the DK Hostmaster A/S risk assessment is included, it can hold one of the following values:

- `RED` - the registrant is requested to complete successful ID-control before the domain name can become active
- `YELLOW` - the registrant is requested to complete successful ID-control, the domain name becomes active immediately. If ID-control is not completed within the communicated timeframe the domain is made inactive
- `BLUE` - the registrant is requested to complete successful ID-control before the domain name can become active
- `GREEN` - the domain name becomes active immediately
- `N/A` - the risk assessment could not be performed, the registrant is requested to complete successful ID-control before the domain name can become active

The procedures for ID-control are [described on the DK Hostmaster DK website][DKHMIDENT].

The status codes applying to domain are described in the addendum: [Domain Status Codes](#domain-status-codes).

<a id="domain_application_failure"></a>
##### Domain name Application/Creation Failure

As described in the introduction, the existing commands, which are categorized as billable are not changed. Due to the change to the billing procedure however, the application/create operation is extended with a error scenario, for when the prepaid account does not have sufficient funds.

The proposed error message is the following, quoted from [RFC:5730].

> 2104    "Billing failure"
>
> his response code MUST be returned when a server attempts
> to execute a billable operation and the command cannot be
> completed due to a client-billing failure.

The choice of administration model is based on the default set for the registrar account. This can be set in the registrar portal. It can be overwritten per application using the `dkhm:management` extension, which can have one of two values:

- `registrar`, indicating registrar management
- `registrant`, indicating registrant management

The designated registrant (contact) has to be under the same management model or the application will be rejected.

The creation of contacts (registrants) is covered under [create contact](#create-contact).

![Create domain][epp_create_domain]

| Return Code | Description |
| ----------- | ----------- |
| 1001 | Command completed successfully; action pending |
| 2104 | Billing failure |

<a id="create-domain-request"></a>
##### create domain request

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<command>
		<create>
			<domain:create xmlns:domain="urn:ietf:params:xml:ns:domain-1.0" xsi:schemaLocation="urn:ietf:params:xml:ns:domain-1.0 domain-1.0.xsd">
				<domain:name>dk-hostmaster-test-906.dk</domain:name>
				<domain:period unit="y">1</domain:period>
				<domain:ns>
					<domain:hostObj>ns1.dk-hostmaster.dk</domain:hostObj>
					<domain:hostObj>ns2.dk-hostmaster.dk</domain:hostObj>
				</domain:ns>
				<domain:registrant>DKHM1-DK</domain:registrant>
				<domain:authInfo>
					<domain:pw />
				</domain:authInfo>
			</domain:create>
		</create>
		<extension>
			<dkhm:orderconfirmationToken xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3">testtoken</dkhm:orderconfirmationToken>
		</extension>
		<clTRID>92724843f12a3e958588679551aa988d</clTRID>
	</command>
</epp>
```

<a id="create-domain-response"></a>
##### create domain response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1001">
			<msg>Create domain pending for domain1.dk</msg>
		</result>
		<msgQ count="1" id="1"/>
		<extension>
			<dkhm:trackingNo xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3">2013010100030</dkhm:trackingNo>
			<dkhm:domain_confirmed xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3">1</dkhm:domain_confirmed>
			<dkhm:registrant_validated xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3">1</dkhm:registrant_validated>
			<dkhm:url xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3">https://selfservice-dk-hostmaster.dk/6102505a2e8d0cfbe8c3c99ea49977f36e2d4ee3</dkhm:url>
		</extension>
		<trID>
			<clTRID>47a4178679f26909ebcfcfd8572f315c</clTRID>
			<svTRID>EDF4F436-9CC9-11E4-AC57-51CB2AC2711D-2013010100030</svTRID>
		</trID>
	</response>
</epp>
```

This tracking number (`trackingNo`), listed as an extension and does not replace or interfere with the normal use of the EPP transaction keys, `clTRID` and `svTRID`, but are EPP specific, whereas the tracking number is considered global in DK Hostmaster. The tracking number is also appended to the `svTRID` in addition to the listing in the extension part. Please see the last digits following the last dash.

```XML
<svTRID>9917BE58-3D53-11E2-A5BD-C532BF0DC46A-1234</svTRID>
```

An important note is that the `clTRID` is mandatory for this command. Since we use the `clTRID` to report back via the message polling functionality, when the domain creation request changes state.

The default value for domain value, if not specified, is one year.

<a id="poll-and-messages"></a>
##### Poll and Messages

As described above the creation of domain names is not synchronous, after the  creation of a domain
request, resulting in a pending state, will have to be probed using the poll command.

The outcome can be one of two, please see the examples below:

<a id="create-domain-poll-message-for-successful-creation"></a>
###### create domain poll message for successful creation

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="1" id="2">
			<msg>Created domain for eksempel.dk has been approved</msg>
		</msgQ>
		<resData>
			<domain:panData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name paResult="1">eksempel.dk</domain:name>
				<domain:paTRID>
					<clTRID>916e2f64ca0956a1bfc24140b23b8fb3</clTRID>
					<svTRID>001C6E66-761D-11E8-8775-F5EABB5937F7-2018062200008</svTRID>
				</domain:paTRID>
				<domain:paDate>2018-06-22T15:07:00.0Z</domain:paDate></domain:panData>
		</resData>
		<extension>
			<dkhm:risk_assessment xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3">N/A</dkhm:risk_assessment>    </extension>
		<trID>
			<clTRID>4fc3af83a40f85dd01bf5110727ee943</clTRID>
			<svTRID>7F3D4CD8-761D-11E8-8775-F5EABB5937F7</svTRID>    </trID></response>
</epp>
```

<a id="create-domain-poll-message-for-unsuccessful-creation-existing-domain"></a>
###### create domain poll message for unsuccessful creation, existing domain

```xml
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="1" id="1">
			<msg>Object exists</msg>
		</msgQ>
		<resData>
			<domain:creData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name>dk-hostmaster.dk</domain:name>
				<domain:crDate>2018-06-22T14:08:08.0Z</domain:crDate>
				<domain:exDate>2022-03-31T00:00:00.0Z</domain:exDate>
			</domain:creData>
		</resData>
		<extension>
			<dkhm:risk_assessment xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3">N/A</dkhm:risk_assessment>
		</extension>
		<trID>
			<clTRID>71a61d8181fce08fc1c087f409a6168b</clTRID>
			<svTRID>DD118802-761C-11E8-8775-F5EABB5937F7</svTRID>
		</trID>
	</response>
</epp>
```

<a id="role-mapping"></a>
##### Role Mapping

As for the user entities some mappings are made so all relevant roles are specified.

| EPP        | DKHM                       | Fallback   | Note                                                |
| ---------- | -------------------------- | ---------- | --------------------------------------------------- |
| admin      | administrator (fuldmægtig) | registrant | optional, will use fallback                         |
| billing    | billing (betaler)          | registrant | optional, will use fallback                         |
| tech       | keyholder (nøgleansvarlig) |            | optional, will be ignored if keyholder is specified |
| registrant | registrant                 | mandatory  |                                                     |
| registrar  | registrar                  | mandatory  |                                                     |

Please note that the command supports Punycode notation for specifying IDN domain names, but responses are in the specified UTF-8 character set.

![Diagram of role resolution for EPP create domain][epp-role-resolution]

<a id="check-domain"></a>
#### check domain

| Return Code | Description |
| ----------- | ----------- |
| 1000 | If the check domain command is successful |
| 2303 | If the specified domain object does not exist |

<a id="check-domain-request"></a>
##### check domain request

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<command>
		<check>
			<domain:check xmlns:domain="urn:ietf:params:xml:ns:domain-1.0" xsi:schemaLocation="urn:ietf:params:xml:ns:domain-1.0 domain-1.0.xsd">
				<domain:name>dk-hostmaster.dk</domain:name>
			</domain:check>
		</check>
		<clTRID>82d73f4f441bcc5fa50952196bb19de5</clTRID>
	</command>
</epp>
```

<a id="check-domain-response"></a>
##### check domain response

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1000">
			<msg>Check result</msg>
		</result>
		<resData>
			<domain:chkData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:cd>
					<domain:name avail="0">dk-hostmaster.dk</domain:name>
					<domain:reason>In use</domain:reason>
				</domain:cd>
			</domain:chkData>
		</resData>
		<trID>
			<clTRID>82d73f4f441bcc5fa50952196bb19de5</clTRID>
			<svTRID>36FB99DC-F6F3-11E3-867F-A6B052036DCB</svTRID>
		</trID>
	</response>
</epp>
```

In general this part of the EPP protocol is described in [RFC:5731] and this command adheres to the standard.

The available values for the `reason` field are:

- `In use` for domain names registered with the DK Hostmaster registry
- `Enqueued` for domain names awaiting domain name application processing, This can last a few seconds to a few days if the application require accept of terms and conditions from the designated registrant
- `Offered for pos. on waiting list`, for when the domain name has been offered to a designated registrant from a waiting list position

An example for a waiting list position offering would look as follows:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
    <response>
        <result code="1000">
            <msg>Command completed successfully</msg>
        </result>
        <resData>
            <domain:chkData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
                <domain:cd>
                    <domain:name avail="0">waiting-list.dk</domain:name>
                    <domain:reason>Offered for pos. on waiting list</domain:reason>
                </domain:cd>
            </domain:chkData>
        </resData>
        <trID>
            <clTRID>24234ad07890f6961184fd58268904dd</clTRID>
            <svTRID>B8D178A4-097A-11EC-A97C-5241511D3588</svTRID>
        </trID>
    </response>
</epp>
```

<a id="info-domain"></a>
#### info domain

This part of the EPP protocol is described in [RFC:5731]. This command adheres to the standard. In addition the command has been extended with two of the DK Hostmaster extensions:

- `dkhm:domainAdvisory`
- `dkhm:registrant_validated`
- `dkhm:authInfoExDate`

Do note that the response only contains the registrant contact object, if the authenticated user has a relationship via the domain name, which provides access to more information.

The below example could shows the public available information. It could be extended with the following data:

```xml
<domain:contact type="admin">DKHM1-DK</domain:contact>
```

and additionally:

```xml
<domain:contact type="billing">DKHM1-DK</domain:contact>
```

Do note that the billing contact and admin/proxy is displayed if the authenticated user has user has authorization to see this information. The registrant role information for the domain is public available. The authorization requires a relationship via a role on the domain name or a registrar group association

For DNSSEC data the availability is limited to only displaying if the information is public available.

The `domain:clID` field communicates portfolio information about the given domain:

- For registrant managed domain names: <domain:clID>DKHM1-DK<domain:clID>, indicating DK Hostmaster A/S
- For registrar managed domain names:
    - <domain:clID>REG-123456</domain:clID>, as seen by users associated with the registrar account
    - <domain:clID>Example Registry Name</domain:clID>, as seen by users not associated with the registrar account

Please see the [addendum on domain status codes](#domain-status-codes).

| Return Code | Description |
| ----------- | ----------- |
| 1000 | If the info domain command is successful |
| 2303 | If the specified domain object does not exist |

<a id="info-domain-request"></a>
##### info domain request

```XML
<?xml version="1.0" encoding="UTF-8"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<info>
			<domain:info xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name>dk-hostmaster.dk</domain:name>
			</domain:info>
		</info>
		<clTRID>e007d4d21ec089623bd71b65f33f2865</clTRID>
	</command>
</epp>
```

<a id="info-domain-response"></a>
##### info domain response

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Info result</msg>
    </result>
    <resData>
      <domain:infData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>dk-hostmaster.dk</domain:name>
        <domain:roid>DK_HOSTMASTER_DK-DK</domain:roid>
        <domain:status s="serverUpdateProhibited"/>
        <domain:status s="serverTransferProhibited"/>
        <domain:status s="serverDeleteProhibited"/>
        <domain:registrant>DKHM1-DK</domain:registrant>
        <domain:ns>
          <domain:hostObj>auth01.ns.dk-hostmaster.dk</domain:hostObj>
          <domain:hostObj>auth02.ns.dk-hostmaster.dk</domain:hostObj>
          <domain:hostObj>p.nic.dk</domain:hostObj>
        </domain:ns>
		<domain:host>auth01.ns.dk-hostmaster.dk</domain:host>
		<domain:host>auth02.ns.dk-hostmaster.dk</domain:host>
		<domain:host>venteliste1.dk-hostmaster.dk</domain:host>
		<domain:host>venteliste2.dk-hostmaster.dk</domain:host>
		<domain:host>blocked1.ns.dk-hostmaster.dk</domain:host>
		<domain:host>blocked2.ns.dk-hostmaster.dk</domain:host>
        <domain:clID>DKHM1-DK</domain:clID>
        <domain:crID>DKHM1-DK</domain:crID>
        <domain:crDate>1998-01-19T00:00:00.0Z</domain:crDate>
        <domain:exDate>2022-03-31T00:00:00.0Z</domain:exDate>
      </domain:infData>
    </resData>
    <extension>
      <dkhm:registrant_validated xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3">1</dkhm:registrant_validated>
      <secDNS:infData xmlns:secDNS="urn:ietf:params:xml:ns:secDNS-1.1">
        <secDNS:dsData>
          <secDNS:keyTag>25591</secDNS:keyTag>
          <secDNS:alg>8</secDNS:alg>
          <secDNS:digestType>2</secDNS:digestType>
          <secDNS:digest>d4323bf6717060bda3a537d6c43ed2719d2656f21ae53f85028ed78ae18afcf6</secDNS:digest>
        </secDNS:dsData>
        <secDNS:dsData>
          <secDNS:keyTag>25591</secDNS:keyTag>
          <secDNS:alg>8</secDNS:alg>
          <secDNS:digestType>4</secDNS:digestType>
          <secDNS:digest>c72da309c49d2e468298cb04498bce9879eb40e1a486c16cbe73f6c95a1f9ff31ff35efb7d8d74625935a18b4e64fb15</secDNS:digest>
        </secDNS:dsData>
        <secDNS:dsData>
          <secDNS:keyTag>30491</secDNS:keyTag>
          <secDNS:alg>8</secDNS:alg>
          <secDNS:digestType>2</secDNS:digestType>
          <secDNS:digest>6159b7ab1d087360fffb8d75ea394a6533012562291b94a03bd813c7cd2bf68c</secDNS:digest>
        </secDNS:dsData>
        <secDNS:dsData>
          <secDNS:keyTag>30491</secDNS:keyTag>
          <secDNS:alg>8</secDNS:alg>
          <secDNS:digestType>4</secDNS:digestType>
          <secDNS:digest>434f4f1b37cb408ee9337acd36d26871066e17177d38b2f7070fd0088ce8df289641ac2a2e62e860fa91f210d39c883a</secDNS:digest>
        </secDNS:dsData>
      </secDNS:infData>
    </extension>
    <trID>
      <clTRID>fee9352765ab62fefc69558d3f4e0eed</clTRID>
      <svTRID>CF056EB6-D2CA-11E9-8DAB-C853983F0358</svTRID>
    </trID>
  </response>
</epp>
```

<a id="info-domain-response-with-domain-advisory"></a>
##### info domain response with domain advisory

A info domain response can be annotated with information using the extension  `dkhm:domainAdvisory`.

The advisory can currently communicate two advisories:

- `pendingDeletionDate`, indicating that a given domain name is scheduled for deletion
- `offeredOnWaitingList`, indicating that a given domain name has been offered to a designated registrant

If a domain name is marked for pending deletion, this special status is communicated via the `dkhm:domainAdvisory` extension.

```xml
<extension>
    <dkhm:domainAdvisory advisory="pendingDeletionDate" date="2020-10-14T00:00:00.0Z" domain="eksempel.dk" xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3"/>
</extension>
```

The field `advisory` indicates a pending delete date with the string: `pendingDeletionDate` followed by a field containing the actual date.

Do note the date is only guiding, since the actual operation of deletion is handled by an external process and operational circumstances might vary and are executed under the discretion of the registry.

If a domain name is offered to a position on a waiting list, the advisory `offeredOnWaitingList` is used.

```xml
<extension>
    <dkhm:domainAdvisory advisory="offeredOnWaitingList" domain="eksempel.dk" xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3"/>
</extension>
```

Do note that the waiting list status is also used in the [check domain](#check-domain) command, using the `reason` field.

Since a waiting list offering is not a complete domain name registration the responses is limited.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
    <response>
        <result code="1000">
            <msg>Command completed successfully</msg>
        </result>
        <resData>
            <domain:infData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
                <domain:name>eksempel.dk</domain:name>
                <domain:roid>EKSEMPEL_DK-DK</domain:roid>
                <domain:status s="ok"/>
                <domain:registrant>UNKNOWN-DK</domain:registrant>
                <domain:clID>DKHM1-DK</domain:clID>
                <domain:crID>UNKNOWN-DK</domain:crID>
            </domain:infData>
        </resData>
        <extension>
            <dkhm:domainAdvisory advisory="offeredOnWaitingList" domain="eksempel.dk"
                xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3"/>
        </extension>
        <trID>
            <clTRID>733a8410c40f76de6b41c9fa51d48f81</clTRID>
            <svTRID>4B1EF7E6-00E2-11EC-AEDE-C9E35F504B00</svTRID>
        </trID>
    </response>
</epp>
```

<a id="info-domain-response-with-authinfo-token"></a>
#### info domain response with AuthInfo token

When the AuthInfo token has been set it can be retrieved via the EPP command: `info domain`, do note that the retrieval requires authorization and therefor authentication, which can be used the authorization to include the AuthInfo token in the response, it is only visible to users with the privilege to see it.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <domain:infData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>example.com</domain:name>
        <domain:roid>EXAMPLE1-REP</domain:roid>
        <domain:status s="ok"/>
        <domain:registrant>jd1234</domain:registrant>
        <domain:contact type="admin">sh8013</domain:contact>
        <domain:contact type="tech">sh8013</domain:contact>
        <domain:ns>
          <domain:hostObj>ns1.example.com</domain:hostObj>
          <domain:hostObj>ns1.example.net</domain:hostObj>
        </domain:ns>
        <domain:host>ns1.example.com</domain:host>
        <domain:host>ns2.example.com</domain:host>
        <domain:clID>ClientX</domain:clID>
        <domain:crID>ClientY</domain:crID>
        <domain:crDate>1999-04-03T22:00:00.0Z</domain:crDate>
        <domain:upID>ClientX</domain:upID>
        <domain:upDate>1999-12-03T09:00:00.0Z</domain:upDate>
        <domain:exDate>2005-04-03T22:00:00.0Z</domain:exDate>
        <domain:trDate>2000-04-08T09:00:00.0Z</domain:trDate>
        <domain:authInfo>
          <domain:pw>DKHM1-DK-098f6bcd4621d373cade4e832627b4f6</domain:pw>
        </domain:authInfo>
      </domain:infData>
    </resData>
    <extension>
      <dkhm:authInfoExDate xmlns:dkhm="urn:dkhm:xml:ns:dkhm-4.3">2018-11-14T09:00:00.0Z</dkhm:authInfoExDate>
    </extension>
    <trID>
      <clTRID>ABC-12345</clTRID>
      <svTRID>54322-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

TODO: this example needs to be updated.

The response is further extended with the `dkhm:authInfoExDate` extension, communicating the expiration date of the current `AuthInfo` for the domain, again only visible if privileges permit.

```xml
<extension>
    <dkhm:authInfoExDate xmlns:dkhm="urn:dkhm:xml:ns:dkhm-4.3">2018-11-14T09:00:00.0Z</dkhm:authInfoExDate>
</extension>
```

<a id="renew-domain"></a>
#### renew domain

This part of the EPP protocol is described in [RFC:5731]. This command adheres to the standard.

Do note that for period specification, only the unit `y` for year is accepted.

The following values for the period specification are accepted:

- `1`
- `2`
- `3`
- `4`
- `5`
- `6`
- `7`
- `8`
- `9`
- `10`

Not specifying acceptable parameters will result in error code `2005` with a message indicating the error.

Not specifying the period parameters will result in the unit: `y` and the value: `1`.

![Diagram of EPP process for EPP renew domain][epp-renew-domain]

| Return Code | Description |
| ----------- | ----------- |
| 1000 | If the renew domain command is successful |
| 2005 | Syntax of the command is not correct |
| 2105 | If the domain object is not eligible for renewal. The domain name has to be in the state ‘Active’ and the expiration date has to be a at least month into the future from the current date. This will also be reflected in status value `serverRenewProhibited`. See also [ICANN description][ICANNserverRenewProhibited] of status |
| 2201 | If the authenticated user does not hold the privilege to renew the specified domain object. This privilege is given to the billing contact for the domain name (see also the [login command](#login)) |
| 2303 | If the specified domain object does not exist |
| 2306 | If the specified expiry date is not valid. The provided expiration date has to be equal to the current expiration date or we return `2306` |
| 2306 | If the calculated expiry date is not allowed. The new expiration date has to be lower than the current expiration date + 5 years. The maximum period to which the expiration date can be extended is 5 years and 3 months. The current expiration date is available via the [info domain](#info-domain) command as `domain:exDate` |
| 2400 | In case of an exception |

This complete process is atomic and might throw an unrecoverable exception: `2400` either due to unforeseen circumstances or a change in the state of the domain name.

On success we emit the return code `1000`. No further communication is made via the EPP service. An invoice is generated and is distributed out of band for EPP as shown in the sub-process and an additional *message* is sent out of band for EPP to the billing contact and the registrant

The sub-process called, can be depicted as follows:

![Diagram of DKH sub-process for EPP renew domain][dkh-renew-domain]

<a id="renew-domain-request"></a>
##### renew domain request

```XML
<?xml version="1.0" encoding="UTF-8"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<renew>
			<domain:renew xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name>dk-hostmaster.dk</domain:name>
				<domain:curExpDate>2017-03-31</domain:curExpDate>
				<domain:period unit="y">1</domain:period>
			</domain:renew>
		</renew>
		<clTRID>541b6801ab3cecdda7da5f735e4f1473</clTRID>
	</command>
</epp>
```

<a id="renew-domain-response"></a>
##### renew domain response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1000">
			<msg>OK</msg>
		</result>
		<msgQ count="10" id="1">
		</msgQ>
		<trID>
			<clTRID>be781a6d19d320867d06e6e80a84a614</clTRID>
			<svTRID>64278BDE-CC4B-11E6-8068-487D3A107CA1</svTRID>
		</trID>
	</response>
</epp>
```

<a id="update-domain"></a>
#### update domain

This part of the EPP protocol is described in [RFC:5731]. This command does not adhere to the standard

- `authInfo` is used to handle AuthInfo tokens and cannot be used for and is not recommended for transport of end-user passwords
- `contact` object in the  `ns` section is ignored

This command covers a lot of functionality, it can complete operations such as:

- change registrant for domain
- add name server to domain
- remove name server from domain
- add admin contact
- remove admin contact
- add billing contact
- remove billing contact
- remove DSRECORDS
- add DSRECORDS
- set/generate AuthInfo token
- unset/clear AuthInfo token

In addition it supports DNSSEC management capabilities as specified in [RFC:5910].

The command will be evaluated as an atomic command, even though it is dispatched to several sub-commands.

![Diagram of EPP process for EPP update domain][epp-update-domain]

The requirements for the command to commence with processing it that the following data are available:

- a valid domain name
- a sub-command, consisting of either
	- add (`add`)
	- change (`chg`)
	- remove (`rem`)

If the request is not valid the service responds with a `2005`.

If the command is valid, the command is separated into one of more of the following sub-commands (by order of precedence):

1. change registrant
1. remove name server
1. remove admin contact
1. remove billing contact
1. add name server
1. add admin contact
1. add billing contact

1. remove DSRECORDS
1. add DSRECORDS

1. setting authinfo
1. unsetting authinfo

The commands are then executed sequentially (order is dictates the precedence) as a single transaction. If a single sub-command fails, the transaction is rolled-back and the relevant error code is returned (`2XXX`).

The command might be stopped if the sub-commands cannot be executed. For example if one of the sub-commands is a: change registrant, none of the other commands can be executed, since role changes will be implicit.

Do note that the change of billing contact, if inserting a registrar-user, will be _silent_, meaning no e-mails will be sent to the registrant or existing billing contact or other contacts.

When the command succeeds either `1000` or `1001` is returned the latter if one of the operations initiated by the sub-command require additional actions to be taken, `1001` will have precedence over `1000`. If a `1001` is returned the status code `pendingUpdate` might be set if an additional **update domain** command is issued.

![Diagram of EPP process for EPP update domain command evaluation][epp-update-domain-evaluate]

| Return Code  | Description |
| ------------ | ------------ |
| 1000 | If the update domain command is successful |
| 1001 | If the update domain command awaits acknowledgement by 3rd. party |
| 2005 | Syntax of the command is not correct |
| 2102 | Change of status for host object is not supported |
| 2201 | If the authenticated user does not hold the privilege to update the specified domain object |
| 2303 | If the specified domain name does not exist |
| 2303 | If the specified host name does not exist, for when adding a new name server |
| 2303 | If the specified host name does not exist, for when removing a name server |
| 2303 | If the specified contact-id  does not exist, for when adding a new billing contact |
| 2304 | If the specified host name does not link with the specified domain name, for when removing a name server |
| 2307 | Unimplemented object service, the service does not support change of registrant on a domain |
| 2308 | The number of name servers are below the required limit |

Please see the below sections for details on the different sub-commands.

The command might be blocked and the status code: `serverUpdateProhibited` is returned indicating that an update is not possible. The status code `clientUpdateProhibited` will be returned if the issued update request cannot be fulfilled due to a domain lock with the registry. See also [ICANN description][ICANN] of status codes.

<a id="update-domain-request"></a>
##### update domain request

```XML
<?xml version="1.0" encoding="UTF-8"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<update>
			<domain:update xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name>eksempel.dk</domain:name>
				<domain:add/>
				<domain:rem/>
				<domain:chg/>
			</domain:update>
		</update>
		<clTRID>c6a678333c526109dea562b42a678398</clTRID>
	</command>
</epp>
```

REF: [issue #9](https://github.com/DK-Hostmaster/epp-service-specification/issues/9)

<a id="update-domain-response"></a>
##### update domain response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1000">
			<msg>Command completed successfully</msg>
		</result>
		<msgQ count="10" id="1">
		</msgQ>
		<trID>
			<clTRID>16465c9766e24e1d1d92d5254a3f3717</clTRID>
			<svTRID>B9B4777A-CC4A-11E6-84D4-467D3A107CA1</svTRID>
		</trID>
	</response>
</epp>
```

<a id="change-registrant"></a>
##### change registrant

The change of registrant operation results in all privileges and rights being transferred to another entity. A registrar only holds the privileges to complete such an operation for domains in own portfolio.

This mean the following prerequisites have to be met:

- The domain name has to be in the portfolio of the registrar requesting the change

- The registrant on the domain name has to be in the portfolio of the registrar requesting the change

- The domain name has to be in a state where it can have the registrant changed

- The contact designated to be appointed as the new registrant are in the portfolio of the registrar requesting the change

- The contact designated to be appointed as the new registrant has to be in a state where it can be assigned the role of registrant

The command can be issued in two variations:

1. With the use of the `dkhm:orderconfirmationToken`, where the designated registrant has approved Terms and Condition for DK Hostmaster with the registrar.

1. As standard without any use of extensions

The two above methods reflect the same usage as for domain name application using [domain create](#domain-create).

Here follows an example of a request to change the registrant using the seconds method.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <command>
        <update>
            <domain:update xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
                <domain:name>eksempel.dk</domain:name>
                <domain:chg>
                    <domain:registrant>DKHM1-DK</domain:registrant>
                </domain:chg>
            </domain:update>
        </update>
        <clTRID>ABC-12345</clTRID>
    </command>
</epp>
```

The response indicates that the operation has one or more pending actions

1. DK Hostmaster require that the designated registrant accepts terms and conditions

1. DK Hostmaster require that ID-control is successfully completed

If the registrant already has completed ID-control, the second action will not be required.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <response>
        <result code="1001">
	        <msg>Command completed successfully; action pending</msg>
        </result>
        <trID>
            <clTRID>ABC-12345</clTRID>
            <svTRID>54321-XYZ</svTRID>
        </trID>
    </response>
</epp>
```

To assist the registrant the registrar can offer collect the accept of terms and conditions for DK Hostmaster and indicate the accept of these via the extension: `dkhm:orderconfirmationToken`.

Then the request would have to be extended with the use of the mentioned extension:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <command>
        <update>
            <domain:update xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
                <domain:name>eksempel.dk</domain:name>
                <domain:chg>
                    <domain:registrant>DKHM1-DK</domain:registrant>
                </domain:chg>
            </domain:update>
        </update>
		<extension>
			<dkhm:orderconfirmationToken xmlns:dkhm=“urn:dkhm:params:xml:ns:dkhm-4.3”>
				1522744544
			</dkhm:orderconfirmationToken>
		</extension>
        <clTRID>ABC-12345</clTRID>
    </command>
</epp>
```

The below example of a response, when the accept of terms and conditions has been collected and ID-control has been completed beforehand.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <response>
        <result code="1000">
            <msg>Command completed successfully</msg>
        </result>
        <trID>
            <clTRID>ABC-12345</clTRID>
            <svTRID>54321-XYZ</svTRID>
        </trID>
    </response>
</epp>
```

| Return Code | Description |
| ----------- | ------------ |
| 1000        | If the update domain command is successful |
| 1001        | If the update domain command is successful, but an action is pending |
| 2005        | Syntax of the command is not correct |
| 2201        | If the authenticated user does not hold the privilege to update the specified domain object |
| 2303        | If the specified domain name does not exist |

<a id="add-name-server"></a>
##### add name server

The addition of a new name server to a domain name or a re-delegation requires that the new name server must offer resolution for the domain name in question.

Note as of version of 4.X.X the commands to change name servers (addition and removal) require AuthInfo token. The AuthInfo token is either provided out of band or can be obtained using the `info domain` command. It can also be generated using the `update domain` command, please see the section on setting AuthInfo.

With this process change, the change of name servers operation using [update domain](#update-domain), also delete all DSRECORDS.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<update>
			<domain:update
			 xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name>eksempel.dk</domain:name>
				<domain:add>
					<domain:ns>
						<domain:hostObj>ns2.example.com</domain:hostObj>
					</domain:ns>
				</domain:add>
			</domain:update>
		</update>
		<clTRID>ABC-12345</clTRID>
	</command>
</epp>
```

![Update domain - Add name server][epp-update-domain-add-ns]

| Return Code | Description |
| ----------- | ------------ |
| 1000        | If the update domain command is successful |
| 2005        | Syntax of the command is not correct |
| 2201        | If the authenticated user does not hold the privilege to update the specified domain object |
| 2303        | If the specified domain name does not exist |
| 2303        | If the specified host name does not exist, for when adding a new name server |

<a id="remove-name-server"></a>
##### remove name server

The removal of a existing name server from a domain name requires that at least two other name servers are offering resolution for the domain in question, else the command will fail.

Since the update domain command can contain several sub-commands, this could be accompanied by an *add name server* (see above), so the policy requirement is met and resolution is kept.

As noted under "add name server", since version of 4.X.X the commands to change name servers (addition) require AuthInfo token. The AuthInfo token is either provided out of band or can be obtained using the `info domain` command. It can also be generated using the `update domain` command, please see the section on setting AuthInfo.

With this process change, the change of name servers operation using [update domain](#update-domain), also delete all DSRECORDS.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<update>
			<domain:update
			 xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name>eksempel.dk</domain:name>
				<domain:rem>
					<domain:ns>
						<domain:hostObj>ns1.example.com</domain:hostObj>
					</domain:ns>
					<domain:contact type="tech">sh8013</domain:contact>
				</domain:rem>
			</domain:update>
		</update>
		<clTRID>ABC-12345</clTRID>
	</command>
</epp>
```

![Update domain - Remove name server][epp-update-domain-remove-ns]

| Return Code | Description |
| ----------- | ------------ |
| 1000        | If the update domain command is successful |
| 2005        | Syntax of the command is not correct |
| 2201        | If the authenticated user does not hold the privilege to update the specified domain object |
| 2303        | If the specified domain name does not exist |
| 2303        | If the specified host name does not exist, for when removing a name server |
| 2304        | If the specified host name does not link with the specified domain name, for when removing a name server |
| 2308        | The number of name servers are below the required limit |

<a id="add-contact"></a>
##### add contact

The addition of a new contact has to adhere to some policies.

1. If the contact is the admin, only the billing role can be added
2. If the authenticated user is a registrar only billing can be added
3. The new contact is requested to accept the role, so the operation is asynchronous

Adding new users require special privileges, currently only with the registrant, apart from the policy listed above.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<update>
			<domain:update
			 xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name>eksempel.dk</domain:name>
				<domain:add>
					<domain:contact type="tech">mak21</domain:contact>
				</domain:add>
			</domain:update>
		</update>
		<clTRID>ABC-12345</clTRID>
	</command>
</epp>
```

![Update domain - Add billing/admin contact][epp-update-domain-add-contact]

![Update domain - Add billing/admin contact sub-process][dkh-update-domain-add-contact]

<a id="remove-contact"></a>
##### remove contact

The removal of a existing contact is possible for both billing and admin contacts.

1. If the contact is the admin, both billing and admin roles can be removed
2. The admin can add a new billing role (see above)
3. If no addition the role defaults to the registrant becoming the inhabitant of the role, no request is made, the registrant is only informed of the change out of band

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<update>
			<domain:update
			 xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name>eksempel.dk</domain:name>
				<domain:rem>
					<domain:contact type="tech">sh8013</domain:contact>
				</domain:rem>
			</domain:update>
		</update>
		<clTRID>ABC-12345</clTRID>
	</command>
</epp>
```

![Update domain - Remove billing/admin contact][epp-update-domain-remove-contact]

![Update domain - Remove billing/admin contact sub-process][dkh-update-domain-remove-contact]

<a id="remove-dsrecords"></a>
##### Remove DSRECORDS

Example with removal of existing DSRECORDS and adding a new DSRECORD.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <command>
        <update>
            <domain:update xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
                <domain:name>eksempel.dk</domain:name>
                <domain:add/>
                <domain:rem/>
                <domain:chg/>
            </domain:update>
        </update>
        <extension>
            <secDNS:update xmlns:secDNS="urn:ietf:params:xml:ns:secDNS-1.1">
                <secDNS:rem>
                    <secDNS:all>true</secDNS:all>
                </secDNS:rem>
                <secDNS:add>
                    <secDNS:dsData>
                        <secDNS:keyTag>52378</secDNS:keyTag>
                        <secDNS:alg>13</secDNS:alg>
                        <secDNS:digestType>4</secDNS:digestType>
                        <secDNS:digest>ed3ef3e1787f797a538abf130fd90d7499713976f7da7c05e51826554560fd42bba5e66dbd2f573a75d77eb0b05124c4</secDNS:digest>
                    </secDNS:dsData>
                </secDNS:add>
            </secDNS:update>
        </extension>
        <clTRID>a84e346d7b5b1242f8e0d28fa8fde565</clTRID>
    </command>
</epp>
```

| Return Code  | Description |
| ------------ | ------------ |
| 1000         | If the update domain command is successful |
| 2005         | Syntax of the command is not correct |
| 2201         | If the authenticated user does not hold the privilege to update the specified domain object |
| 2303         | If the specified domain name does not exist |
| 2303         | If DSRECORDS do not exist, when removing DSRECORDS |

<a id="add-dsrecords"></a>
##### Add DSRECORDS

Example with removal of existing DSRECORDS and adding a new DSRECORD.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <command>
        <update>
            <domain:update xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
                <domain:name>eksempel.dk</domain:name>
                <domain:add/>
                <domain:rem/>
                <domain:chg/>
            </domain:update>
        </update>
        <extension>
            <secDNS:update xmlns:secDNS="urn:ietf:params:xml:ns:secDNS-1.1">
                <secDNS:rem>
                    <secDNS:all>true</secDNS:all>
                </secDNS:rem>
                <secDNS:add>
                    <secDNS:dsData>
                        <secDNS:keyTag>52378</secDNS:keyTag>
                        <secDNS:alg>13</secDNS:alg>
                        <secDNS:digestType>4</secDNS:digestType>
                        <secDNS:digest>ed3ef3e1787f797a538abf130fd90d7499713976f7da7c05e51826554560fd42bba5e66dbd2f573a75d77eb0b05124c4</secDNS:digest>
                    </secDNS:dsData>
                </secDNS:add>
            </secDNS:update>
        </extension>
        <clTRID>a84e346d7b5b1242f8e0d28fa8fde565</clTRID>
    </command>
</epp>
```

<a id="setting-authinfo"></a>
##### Setting AuthInfo

Setting the AuthInfo is done using the `update domain` command. The AuthInfo token is not set as such, but is generated using a keyword indicating the authorization scope, currently supported keywords are:

- `autoredel`
- `autotransfer`

The AuthInfo token and hence the authorization holds a lifespan of 14 days. It can be ended prematurely by unsetting it, please see below.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <domain:update xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>example.com</domain:name>
        <domain:chg>
          <domain:authInfo>
            <domain:pw>autoredel</domain:pw>
          </domain:authInfo>
        </domain:chg>
      </domain:update>
    </update>
    <clTRID>ABC-12345</clTRID>
  </command>
</epp>
```

The generation of an AuthInfo token can be accomplished by all users with privileges to do so.

When the authorisation has been created it is visible to the users with the privilege to generate it, not just the requester.

The token is accessible in:

- The service portal in the domain detail page
- The registrar portal in the domain detail page
- Via EPP using the [domain info](#domain-info) command

<a id="unsetting-authinfo"></a>
##### Unsetting AuthInfo

The requester (setter) of a an AuthInfo authorization might have an interest in ending the life of a AuthInfo token prematurely

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <domain:update xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>example.com</domain:name>
        <domain:chg>
          <domain:authInfo>
            <domain:null>
          </domain:authInfo>
        </domain:chg>
      </domain:update>
    </update>
    <clTRID>ABC-12345</clTRID>
  </command>
</epp>
```

Do note that this can be accomplished by all users with privileges to accomplish this. It adheres to [RFC:5731], which states:

> A domain:null element can be used within the domain:authInfo element to remove authorization information.

The command simply unsets (removes/clears) an AuthInfo token if it exists.

<a id="authinfo-token-format"></a>
##### AuthInfo Token Format

AuthInfo tokens are keys to delegate authorization for a single one-time operation. AuthInfo tokens are limited to work on a single object and perform a single operation, they expire after a specified time, or when used or if they are retracted.

The **AuthInfo** token is generated upon request by DK Hostmaster and will adhere to the following proposed format:

`<handle>-<unique key>`

E.g.

An **AuthInfo** token set request by DK Hostmaster A/S (`DKHM-1`), will resemble the following:

`DKHM1-DK-098f6bcd4621d373cade4e832627b4f6`

The requirements are:

- Unpredictable (is secure to the extent possible and for the given TTL time frame)
- Human pronounceable (can be communicated over telephone call)
- Usable (constrained on length and format)

If a AuthInfo token is generated by a registrar, it is translated to a simpler format:

`REG-0-098f6bcd4621d373cade4e832627b4f6`

This is planned to be changed to a more expressive format at some point.

For registration of domain names offered from a waiting list, the authorization is using `AuthInfo`, the token here is however simpler and is currently formatted as a 8 character string of case insensitive hexadecimal characters.

<a href="delete-domain"></a>
#### delete domain

The default `delete domain` command behaviour is to deactivate immediately, which complies with [RFC:5731]. Not being able to complete the request will result in a error, also in compliance with [RFC:5731]. Please see below for more information on the business process for deletion.

The current expiration date can be obtained using the `info domain` command and is specified in the `domain:exDate` field. The date conforms with the required format.

The alternative approach to deletion is to set auto expire, which will cancel the domain name automatically at expiration.

Do note that it is not possible to delete a domain name on the or after the expiration date of a domain name. Domain name deletion is not prohibited on the expiration date, but due to technical constraints it is recommended to set automatic expiration instead, which will have the same result.

The deletion of a domain name results in a 30 day suspension, which is regarded as a redemption period where it is possible to restore the suspended domain name using the [restore domain](#restore-domain) command.

<a id="delete-domain-request"></a>
##### delete domain request

The complete command will look as follows (example lifted from [RFC:5731]):

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
  <epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <command>
      <delete>
        <domain:delete xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
          <domain:name>eksempel.dk</domain:name>
          </domain:delete>
      </delete>
      <clTRID>ABC-12345</clTRID>
    </command>
  </epp>
```

<a id="delete-domain-response"></a>
##### delete domain response

And the complete command with a deletion date specification (example lifted from [RFC:5731] and modified):

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
  <epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <command>
      <delete>
        <domain:delete xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
          <domain:name>eksempel.dk</domain:name>
          </domain:delete>
      </delete>
      <clTRID>ABC-12345</clTRID>
    </command>
  </epp>
```

Domain names are not deleted immediately, but are flagged as _scheduled for deletion_. This of the `delete command` is successful, the domain name will be flagged for deletion within the timeframe specified by the business rules implemented by DK Hostmaster.

The response for a `delete domain` command will be `1001`.

Response example (example lifted from [RFC:5731] and modified):

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
  <epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <response>
      <result code="1001">
        <msg>Command completed successfully; action pending</msg>
      </result>
      <trID>
        <clTRID>ABC-12345</clTRID>
        <svTRID>54321-XYZ</svTRID>
      </trID>
    </response>
  </epp>
```

The expiration date will be adjusted accordingly and a status `pendingDelete` with an advisory date will be applied and made available via the response to the `info domain` command, via the DK Hostmaster extension: `domainAdvisory`.

Example:

```xml
<extension>
    <dkhm:domainAdvisory advisory="pendingDeletionDate" date="2021-01-31T00:00:00.0Z" domain="eksempel.dk" xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3"/>
</extension>
```

Do note that a `delete domain` command will disable auto renewal if enabled.

Do note that if subordinates exist these will block for a delete and the request will result in an error: `2305`.

<a id="restore-domain"></a>
#### restore domain

As described in [RFC:3915], with a support for grace periods, it is possible to restore a domain name scheduled for deletion, (in the state `pendingDelete`).

DK Hostmaster will support the ability to restore for two use-cases:

1. Get a domain name back to the state active from a pending deletion specified by an explicit deletion request (delete command) or a automatic expiration
1. Get a domain name back to state active from a pending deletion, caused by missing financial settlement

Domain names might be suspended for other reasons, these will no be recoverable using the described restore facility, this will be indicated using the `serverUpdateProhibited` status.

Restoration has to take place during the redemption period and will not be possible after the grace period has expired.

The restoration is requested using the update domain command.

##### restored domain request

Based on feedback from our users, we have decided to not implement the `request` operation part of the restoration process, so you have to skip directly to the `report` part of the process.

This mean we will not support the state of  `pendingRestore` for the restore process.

The step to actually complete the restoration is the `report` operation, which look as follows:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0
     epp-1.0.xsd">
    <command>
        <update>
            <domain:update xmlns:domain="urn:ietf:params:xml:ns:domain-1.0" xsi:schemaLocation="urn:ietf:params:xml:ns:domain-1.0 domain-1.0.xsd">
                <domain:name>example.com</domain:name>
                <domain:chg/>
            </domain:update>
        </update>
        <extension>
            <rgp:update xmlns:rgp="urn:ietf:params:xml:ns:rgp-1.0" xsi:schemaLocation="urn:ietf:params:xml:ns:rgp-1.0 rgp-1.0.xsd">
                <rgp:restore op="report">
                    <rgp:report>
                        <rgp:preData></rgp:preData>
                        <rgp:postData></rgp:postData>
                        <rgp:delTime>2003-07-10T22:00:00.0Z</rgp:delTime>
                        <rgp:resTime>2003-07-20T22:00:00.0Z</rgp:resTime>
                        <rgp:resReason></rgp:resReason>
                        <rgp:statement></rgp:statement>
                    </rgp:report>
                </rgp:restore>
            </rgp:update>
        </extension>
        <clTRID>ABC-12345</clTRID>
    </command>
</epp>
```

Example is lifted from [RFC:3915].

The proposal is to the the report part act as an acknowledgement. The domain name is restored _as-is_ if possible, so the mandatory fields:

- `rgp:preData`
- `rgp:postData`
- `rgp:resReason`
- `rgp:statement`

Have to be specified, but values are ignored. As are the optional field, which however is optional and does not have to be specified:

- `rgp:other`

The mandatory fields:

- `rgp:delTime`
- `rgp:resTime`

Have to be specified and will be evaluated according to [RFC:3915].

- The `rgp:delTime` value has to match the deletion date and time.
- The `rgp:resTime` value will be ignored since, we do not handle the `request` operation.

As described in [RFC:3915], multiple report requests can be submitted, until success and within the allowed timeframe of possible restoration.

<a id="restore-domain-response"></a>
##### restore domain response

A response indicating unsuccessful restoration attempt will look as follows:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <response>
        <result code="2004">
            <msg>Delete date is incorrect</msg>
        </result>
        <trID>
            <clTRID>ABC-12345</clTRID>
            <svTRID>54321-XYZ</svTRID>
        </trID>
    </response>
</epp>
```

Example lifted from [RFC:5730] and modified.

A response indicating successful restoration attempt will look as follows:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0
     epp-1.0.xsd">
    <response>
        <result code="1000">
            <msg lang="en">Command completed successfully</msg>
        </result>
        <extension>
            <rgp:upData xmlns:rgp="urn:ietf:params:xml:ns:rgp-1.0" xsi:schemaLocation="urn:ietf:params:xml:ns:rgp-1.0 rgp-1.0.xsd">
                <rgp:rgpStatus s="pendingRestore"/>
            </rgp:upData>
        </extension>
        <trID>
            <clTRID>ABC-12345</clTRID>
            <svTRID>54321-XYZ</svTRID>
        </trID>
    </response>
</epp>
```

Example is lifted from [RFC:3915].

<a id="xsd-definition"></a>
##### XSD Definition

The `restore` command is an extension to `update domain`. All is described in [RFC:3915]. The XSD has been included in our EPP XSD repository as `rgs-1.0.xsd` all lifted from [RFC:3915], please see [the repository][DKHMXSD] for details.

<a id="transfer-domain"></a>
#### transfer domain

The transfer command is only available to registrars. The command should used in the following use-cases.

- Transfer from DK Hostmaster to a future registrar
- Transfer from the current registrar to a future registrar

The implementation is based on a _pull_ model and both operations require authorization via the use of an AuthInfo token and designated domain name. The operation has to be initiated by the future registrar. who has acquired the authorization (AuthInfo token) from the current registrar or the registrant.

The transfer from DK Hostmaster to a new registrar implies a change of administrative model from "registrant management" to "registrar management". Whereas the transfer from registrar to registrar is only a change of administrative party not the administrative model.

The registrar always have the option to withdraw from the role of registrar for a given domain name, this change does not require an authorization. The operation is implemented using the [withdraw](#withdraw-domain) command, described below in details. This command set the registrar to be the registry (DK Hostmaster) and the administrative model changes from "registrar management" to "registrant management".

Also the registrant has the option to exchange the current registrar. This operation implies a change of administrative model from "registrar management" to "registrant management" and is limited to change of model. A change of registrar, requires authorization of a third party by the registrant and that the designated registrar executes the operation of taking the role of administrator as described initially in this section.

The administration of authorizations is described in detail under the [update-domain](#update-domain) command.

- [Setting AuthInfo](#setting-authinfo)
- [Unsetting AuthInfo](#unsetting-authinfo)

Do also see the general description of the [AuthInfo](#authinfo) implementation.

The transfer process differs from the one outlined in the [RFC:5731], the following status codes should only be the ones observed:

- `serverApproved`

The following should not be observed (ref: `domain:trStatus`), since the process does not implement the same transactional model:

- `clientApproved`
- `clientCancelled`
- `clientRejected`
- `pending`
- `serverCancelled`

Upon transfer, the contact object referring to the registrant role, is being cloned to avoid issues with _disappearing data_ and _sponsorship_ in cross-portfolio operations. Do note that DK Hostmaster does not implement direct transfer of contact objects as described in the "Implementation Limitations" section.

A contact object (registrant) is cloned without additional relations bound to other objects within the registry or another portfolio, only the key object, the domain name is transferred, together with potential subordinate objects such as name servers.

The cloning is a _best-effort_ cloning, since the ID-control status cannot be guaranteed to be consistent in the case where a contact object is locked to a register, but has limitations in access to data due to policies in regard to disclosure etc.

The clone might be deleted if these relations are terminated or removed, please see the description of the contact object deletion policy described in the section on the [delete contact command](#delete-contact) for details.

<a id="transition-period"></a>
#### Transition Period

The transition period is a special period following the introduction of the transfer command with version 4.0.0 of the EPP service.

During the transition period, registrars can do a transfer without providing the else mandatory AuthInfo token with the [transfer domain](#transfer-domain) command.

The following prerequisites has to be in place for this operation to be possible:

- The designated domain name has to be linked to a name server belonging to the requesting registrar
- This ownership is identified as the name server administrator for the specific name server is linked to the registrar group of the requesting registrar
- The transfer has to be completed by a user with the registrar role, which can be appointed in the registrar portal
- The registrar will have to have collected a consent from the registrant of the designated domain name

The moment a transfer from registrant management is completed, it is no longer viable for this transfer, even if the name servers are administered by others that the registrar

If a domain name is transferred back to the registry, it will become eligible for the transfer again, of course the mentioned conditions will have to be met and the consent will have to be collected again.

<a id="transfer-domain-request"></a>
##### transfer domain request

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <transfer op="request">
      <domain:transfer xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk</domain:name>
        <domain:period unit="y">1</domain:period>
        <domain:authInfo>
          <domain:pw>DKHM1-DK-098f6bcd4621d373cade4e832627b4f6</domain:pw>
        </domain:authInfo>
      </domain:transfer>
    </transfer>
    <clTRID>ABC-12345</clTRID>
  </command>
</epp>
```

Example is lifted from [RFC:5731] and modified.

<a id="transfer-domain-response"></a>
##### transfer domain response

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <domain:trnData xmlndomain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk</domain:name>
        <domain:trStatus>serverApproved</domain:trStatus>
        <domain:reID>ClientX</domain:reID>
        <domain:reDate>2000-06-08T22:00:00.0Z</domain:reDate>
        <domain:acID>ClientY</domain:acID>
        <domain:acDate>2000-06-13T22:00:00.0Z</domain:acDate>
        <domain:exDate>2002-09-08T22:00:00.0Z</domain:exDate>
      </domain:trnData>
    </resData>
    <trID>
      <clTRID>ABC-12345</clTRID>
      <svTRID>54322-XYZ</svTRID>
    </trID>
  </response>
</epp>
```

Example is lifted from [RFC:5731] and modified.

| Return Code  | Description |
| ------------ | ------------ |
| 1000         | If the update domain command is successful |
| 2005         | Syntax of the command is not correct |
| 2201         | If the authenticated user does not hold the privilege to transfer the specified domain object |
| 2201         | If the provided AuthInfo information is not valid |
| 2303         | If the specified domain name does not exist |

<a id="withdraw"></a>
#### Withdraw

DK Hostmaster support the option for registrars of transferring out, so where the regular transfer command (described above) is a _pull_ operation. The registrar can _push_ a domain name from it's portfolio to DK Hostmaster, when and if a registrar requires so.

The process resembles the transfer, but with the receiving account being DK Hostmaster.

The implementation is based on the extension developed by Norid, the registry for the ccTLD for Norway (.no). The specification is listed in the references section below.

Since this extension is at a higher level than the other extensions defined by DK Hostmaster. The definition look as follows:

```xsd
<!-- dkhm-4.3.xsd -->
<element name="withdraw" type="dkhm:withdrawType"/>
<complexType name="withdrawType">
  <sequence>
    <element name="name" type="eppcom:labelType"/>
  </sequence>
</complexType>
```

Ref: [`dkhm-4.3.xsd`][DKHMXSD]

```xsd
<?xml version="1.0" encoding="UTF-8"?>

<schema targetNamespace="urn:dkhm:params:xml:ns:dkhm-domain-4.3"
        xmlns:dkhm-domain="urn:dkhm:params:xml:ns:dkhm-domain-4.3"
        xmlns:epp="urn:ietf:params:xml:ns:epp-1.0"
        xmlns:eppcom="urn:ietf:params:xml:ns:eppcom-1.0"
        xmlns="http://www.w3.org/2001/XMLSchema"
        elementFormDefault="qualified">

  <import namespace="urn:ietf:params:xml:ns:eppcom-1.0" schemaLocation="eppcom-1.0.xsd"/>
  <import namespace="urn:ietf:params:xml:ns:epp-1.0" schemaLocation="epp-1.0.xsd"/>

  <annotation>
    <documentation>Extensible Provisioning Protocol v1.0 provisioning schema. DKHM extension v4.3 for domain</documentation>
  </annotation>

  <element name="withdraw" type="dkhm-domain:withdrawType"/>

  <complexType name="withdrawType">
    <sequence>
      <element name="name" type="eppcom:labelType"/>
    </sequence>
  </complexType>
</schema>
```

Ref: [`dkhm-domain-4.3.xsd`][DKHMXSD]

<a id="withdraw-request"></a>
##### withdraw request

An example of a withdraw XML request would look as follows (example lifted from Norid specification and modified):

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <extension>
    <command xmlns="urn:dkhm:params:xml:ns:dkhm-4.3">
      <withdraw>
        <domain:withdraw xmlns:domain="urn:dkhm:params:xml:ns:dkhm-domain-4.3">
          <domain:name>eksempel.dk</domain:name>
        </domain:withdraw>
      </withdraw>
      <clTRID>ABC-12345</clTRID>
    </command>
  </extension>
</epp>
```

<a id="withdraw-request"></a>
##### withdraw response

<a id="contact"></a>
### Contact

The default behavior of the EPP `create contact` command as described in [RFC:5733], will attach the client-ID (`CLID`) of the authenticated party to the object created, just as for the domain creation described above.

The contact object will be under the sponsoring party throughout it's _life-cycle_ and transfer of contact objects will not be explicitly supported.

As for the `create domain` command (above) the default behaviour can be defined in RP. Where the option "registrant management", will create contact objects sponsored by DK Hostmaster instead instead of the registrar.

Deletion will not be supported and will work as it currently is implemented in the DK Hostmaster EPP service and described in the specification. See the section: "[Unimplemented commands](#unimplemented-commands)" for details. Contact objects are automatically deleted, under the following policy:

- The contact object is not in use
- It holds not roles/association with other objects
- The associated financial account holds a balance of 0
- It has been inactive for 45 days

The maintenance, meaning changes and updates to data, will also be limited. DK Hostmaster locks contact objects for changes if these have been matched to a register for name and address information, this applies to:

- Danish citizens of the type individual, bound to the CPR register
- Danish companies, public organizations and associations of the types, bound to the CVR register

The following contact types are also limited, due to the VAT number validation:

- EU companies, public organizations and associations of the types, bound to the EU Vies register

All other types has to be maintained by the sponsoring client, with the exception of the name attribute.

<a id="create-contact"></a>
#### create contact

This part of the EPP protocol is described in [RFC:5733].

This command has been extended with the following fields:

- `dkhm:usertype`, which has to be one of:
	- `company`, indicating a company
	- `public_organization`, indicating a public organization
	- `association`, indicating an association
	- `individual`, indicating an individual

The user type will result in context-specific interpretation of the following fields:

- EAN - this number is only supported for user types: `company`, `public_organization` and `association`. It is only mandatory for `public_organization` and optional for `company` and `association`. [EAN][EAN] is used by the public sector in Denmark for electronic invoicing, private companies can also be assigned [EAN], but this it not so widespread at this time. EAN is required by law for public sector organizations, so this field has to be completed and it has to validate for this type.
- CVR - (VAT number) this is only supported for user types: `company`, `public_organization` and `association`. The number is **required** for handling VAT correctly, The rules for indication of the field is specified in the table below.
- p-number - (production unit number) this is only supported for user types: `company`, `public_organization` and `association`. The number is used for handling validation correctly and it relates to the CVR (Vat number field) the field is optional.

This field is validated on the server side, it is however recommended to perform a [check contact](#check-contact) on the requested contact-id prior to the [create domain](#create-domain) request if a contact-id is already known from a contact create or previous domain creation/application.

The `contact-id` field is auto-generated and assigned by DK Hostmaster. EPP do however open for providing a contact-id in the context of the create contact command, this is not supported by DK Hostmaster at this point, see also: [Implementation Limitations](#implementation-limitations).

The choice of administration model is based on the default set for the registrar account, this influences domain application and contact creation and this can be set in the registrar portal. It can be overwritten per request using the `dkhm:management` extension, which can have one of two values:

- `registrar`, indicating registrar management
- `registrant`, indicating registrant management

<a id="cvr--vat-number-indication"></a>
##### CVR / Vat Number Indication

|   | Mandatory | Note |
|---|---|---|
| `company`/`public_organization`/`association` with address in Denmark and EU/EØS | Yes | Has to be specified |
| `company`/`public_organization`/`association` with address EU/EØS | No | Can be specified if VAT handling is required |
| `company`/`public_organization`/`association` with address outside Denmark and EU/EØS | No | Can be specified |
| `individual` with address in Denmark and EU/EØS | No | Not supported |
| `individual` with address outside Denmark and EU/EØS | No | Not supported |

<a id="forced-and-smart-contact-creation"></a>
##### Forced and Smart Contact Creation

For contact creation DK Hostmaster supports two ways:

1. Smart creation, where the data provided is used to inquire if an existing user with the same data is present. If no user is found a new contact is created. This is accomplished using the keyword: `auto`
2. Forced creation, where a new contact is created. This is accomplished using the keyword: `force`

Specification of a user-id / handle for the contact creation is not supported. The user-id / handle is auto-generated and assigned by DK Hostmaster.

For _smart_ creation:

```XML
<contact:id>auto</contact:id>
```

For _forced_ creation:

```XML
<contact:id>force</contact:id>
```

Please note that the `auto` and `force` keywords are in lower-case.

The match for the _smart_ creation are applicable for the following data:

- `<dkhm:userType>`
- `<dkhm:CVR>`
- `<contact:name>`
- `<contact:street>`
- `<contact:email>`
- `<contact:pc>`
- `<contact:cc>`

The match has to be exact in order for the command to return an existing user-id / handle.

![Diagram for contact creation][epp_create_contact]

<a id="address-handling"></a>
##### Address Handling

Contact creation under EPP opens for the ability to represent postal information in both local and international representations. Due to the representation in DK Hostmaster's system for handling contacts the following rules are applied to postal information.

For Denmark the local representation is chosen and the international representation is discarded. For other countries the international representation is chosen and the local representation is discarded. Please see the table below.

| Denmark                      | Other country                    |
| ---------------------------- | -------------------------------- |
| **Local representation**     | Local representation             |
| International representation | **International representation** |

This is a diagram depicting the general algorithm used for resolving the address data. The algorithm presupposes that at least one address is present.

![Diagram of address resolution for contact creation][epp-address-resolution]

It is important to note that if the international representation is specified, but data are provided in local representation or only local representation is provided for an international address, communication to the specified address might prove unreliable.

The handling of name and organization is also a special case. Where the following mapping is made based on the user type.

<table>
<tr>
		<th></th><th colspan="2">Name and Organization Provided</th><th>Only name provided</th>
<tr>
		<th>User type</th><th>Name (<i>mandatory</i>)</th><th>organization (<i>optional</i>)</th><th>Name (<i>mandatory</i>)</th>
</tr>
<tr>
		<td>C (Company)</td><td>attention</td><td>name</td><td>name</td>
</tr>
<tr>
		<td>P (Public organization)</td><td>attention</td><td>name</td><td>name</td>
</tr>
<tr>
		<td>A (Association)</td><td>attention</td><td>name</td><td>name</td>
</tr>
<tr>
		<td>I (Individual)</td><td>name</td><td>-</td><td>name</td>
</tr>
</table>

The data is collected as required by Danish legislation. See also the data collection policy section below.

Please note:

- `authInfo` section is ignored is not recommended for transport of end-user passwords, see also [AuthInfo](#authinfo).
- Contact creation is silent and the designated contact object is not notified about the the creation, unless this is a part of the process of associating the user with other objects

<a id="create-contact-request"></a>
##### create contact request

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<command>
		<create>
			<contact:create xmlns:contact="urn:ietf:params:xml:ns:contact-1.0" xsi:schemaLocation="urn:ietf:params:xml:ns:contact-1.0 contact-1.0.xsd">
				<contact:id>auto</contact:id>
				<contact:postalInfo type="loc">
					<contact:name>Johnny Login</contact:name>
					<contact:org>DK Hostmaster A/S</contact:org>
					<contact:addr>
						<contact:street>Kalvebod brygge 45, 3. sal</contact:street>
						<contact:city>København V</contact:city>
						<contact:pc>1560</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:postalInfo type="int">
					<contact:name>Johnny Login</contact:name>
					<contact:org>DK Hostmaster A/S</contact:org>
					<contact:addr>
						<contact:street>Kalvebod brygge 45, 3.</contact:street>
						<contact:city>Copenhagen V</contact:city>
						<contact:pc>1560</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:fax />
				<contact:email>info@dk-hostmaster.dk</contact:email>
				<contact:authInfo>
					<contact:pw />
				</contact:authInfo>
			</contact:create>
		</create>
		<extension>
			<dkhm:userType xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3">company</dkhm:userType>
			<dkhm:CVR xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3">1234567891231</dkhm:CVR>
		</extension>
		<clTRID>8cced469f2bfdbb0dcad16b875d87c99</clTRID>
	</command>
</epp>
```

Do note that the `authInfo` part is ignored, but cannot be omitted, since it is specified as mandatory by the EPP protocol in [RFC:5733].

<a id="create-contact-response"></a>
##### create contact response

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1000">
			<msg>Contact created.</msg>
		</result>
		<msgQ count="1" id="400">    </msgQ>
		<resData>
			<contact:creData xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DHA484-DK</contact:id>
				<contact:crDate>2015-03-25T17:08:25.0Z</contact:crDate>
			</contact:creData>
		</resData>
		<trID>
			<clTRID>8cced469f2bfdbb0dcad16b875d87c99</clTRID>
			<svTRID>8B9461A4-D311-11E4-B79D-DB67C33995C9</svTRID>
		</trID>
	</response>
</epp>
```

<a id="check-contact"></a>
#### check contact

This part of the EPP protocol is described in [RFC:5733]. This command adheres to the standard.

<a id="check-contact-request"></a>
##### check contact request

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<command>
		<check>
			<contact:check xmlns:contact="urn:ietf:params:xml:ns:contact-1.0" xsi:schemaLocation="urn:ietf:params:xml:ns:contact-1.0 contact-1.0.xsd">
				<contact:id>DKHM1-DK</contact:id>
			</contact:check>
		</check>
		<clTRID>d4d94d2e1d6f613cb276865c49c3d0b7</clTRID>
	</command>
</epp>
```

<a id="check-contact-response"></a>
##### check contact response

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1000">
			<msg>Check result</msg>
		</result>
		<msgQ count="6" id="884">
			<msg>Create domain pending for domain2xyz.dk</msg>
		</msgQ>
		<resData>
			<contact:chkData xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:cd>
					<contact:id avail="0">DKHM1-DK</contact:id>
					<contact:reason>In use</contact:reason>
				</contact:cd>
			</contact:chkData>
		</resData>
		<trID>
			<clTRID>d4d94d2e1d6f613cb276865c49c3d0b7</clTRID>
			<svTRID>3268EB00-F6F7-11E3-867F-A6B052036DCB</svTRID>
		</trID>
	</response>
</epp>
```

<a id="info-contact"></a>
#### info contact

This part of the EPP protocol is described in [RFC:5733]. This command has been extended with information on whether the contact in queried has been validated according to requirements and policies with DK Hostmaster.

See the extension: [`dkhm:contact_validated`](#dkhmcontact_validated) extension used in the response.

Please note that the email address (`contact:email`) is masked and the value: `anonymous@dk-hostmaster.dk` is always returned for this field, Unless the authenticated user has a relationship via the domain name or a registrar group association, which provides access to more information.

The info contact command response is only available for the registrant contact object, unless the authenticated user has a relationship via the domain name or a registrar group association, which provides access to more information or additional contact objects.

<a id="info-contact-request"></a>
##### info contact request

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<command>
		<info>
			<contact:info xmlns:contact="urn:ietf:params:xml:ns:contact-1.0" xsi:schemaLocation="urn:ietf:params:xml:ns:contact-1.0 contact-1.0.xsd">
				<contact:id>DKHM1-DK</contact:id>
			</contact:info>
		</info>
		<clTRID>3d65841027692e64c24118ac5988e03c</clTRID>
	</command>
</epp>
```

<a id="info-contact-response"></a>
##### info contact response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1000">
			<msg>Info result</msg>
		</result>
		<resData>
			<contact:infData xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverUpdateProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:status s="linked"/>
				<contact:status s="serverDeleteProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>DK Hostmaster A/S</contact:name>
					<contact:addr>
						<contact:street>Kalvebod Brygge 45,3</contact:street>
						<contact:city>København V</contact:city>
						<contact:pc>1560</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>anonymous@dk-hostmaster.dk</contact:email>
				<contact:clID>DKHM1-DK</contact:clID>
				<contact:crID>n/a</contact:crID>
				<contact:crDate>2013-01-24T15:40:37.0Z</contact:crDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3">1</dkhm:contact_validated>
		</extension>
		<trID>
			<clTRID>76edfef5b78cdaefe8fb426eb8d74b75</clTRID>
			<svTRID>C8C5E496-9CC8-11E4-9F91-D0BF2AC2711D</svTRID>
		</trID>
	</response>
</epp>
```

<a id="update-contact"></a>
#### update contact

This part of the EPP protocol is described in [RFC:5733]. This command adheres to the standard. In addition to the standard the command allows for manipulation of the extensions associated with contact objects, meaning that it is possible to update the following fields:

- [`dkhm:userType`](#dkhmusertype)
- [`dkhm:EAN`](#dkhmean)
- [`dkhm:CVR`](#dkhmcvr)
- [`dkhm:pnumber`](#dkhmpnumber)
- [`dkhm:mobilephone`](#dkhmmobilephone)
- [`dkhm:secondaryEmail`](#dkhmsecondaryEmail)

These of course all controlled by relevant privileges.

- Name / organization
- Address
- Country
- Phone (voice)
- Fax
- Email
- Secondary email
- Mobile phone

![Diagram of EPP update contact][epp-update-contact]

Please note:

- `authInfo` section is ignored is not recommended for transport of end-user passwords, see also [AuthInfo](#authinfo).

<a id="update-contact-request"></a>
##### update contact request

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<update>
			<contact:update
			 xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>sh8013</contact:id>
				<contact:add>
					<contact:status s="clientDeleteProhibited"/>
				</contact:add>
				<contact:chg>
					<contact:postalInfo type="int">
						<contact:org/>
						<contact:addr>
							<contact:street>124 Example Dr.</contact:street>
							<contact:street>Suite 200</contact:street>
							<contact:city>Dulles</contact:city>
							<contact:sp>VA</contact:sp>
							<contact:pc>20166-6503</contact:pc>
							<contact:cc>US</contact:cc>
						</contact:addr>
					</contact:postalInfo>
					<contact:voice>+1.7034444444</contact:voice>
					<contact:fax/>
					<contact:authInfo>
						<contact:pw>2fooBAR</contact:pw>
					</contact:authInfo>
					<contact:disclose flag="1">
						<contact:voice/>
						<contact:email/>
					</contact:disclose>
				</contact:chg>
			</contact:update>
		</update>
		<extension>
				<dkhm:secondaryEmail xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3">email@eksempel.dk</dkhm:secondaryEmail>
				<dkhm:mobilephone xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3">+1.7034444445</dkhm:mobilephone>
		</extension>
		<clTRID>ABC-12345</clTRID>
	</command>
</epp>
```

Do note that the `authInfo` part is ignored, but cannot be omitted, since it is specified as mandatory by the EPP protocol in [RFC:5733].

<a id="update-contact-response"></a>
##### update contact response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<response>
		<result code="1000">
			<msg>Command completed successfully</msg>
		</result>
		<trID>
			<clTRID>ABC-12345</clTRID>
			<svTRID>54321-XYZ</svTRID>
		</trID>
	</response>
</epp>
```

<a id="delete-contact"></a>
#### delete contact

**This command is not supported.**

This command will always return: `2101`, indicating unimplemented command.

The deletion of contact objects is handled automatically by DK Hostmaster. The following status flags will be set:

- `clientDeleteProhibited`
- `serverDeleteProhibited`

The later will only be lifted when the contact object is not linked to any other objects and automatic deletion is scheduled.

<a id="delete-contact-request"></a>
##### delete contact request

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<delete>
			<contact:delete
			 xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>sh8013</contact:id>
			</contact:delete>
		</delete>
		<clTRID>ABC-12345</clTRID>
	</command>
</epp>
```

<a id="delete-contact-response"></a>
##### delete contact response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<response>
		<result code="2101">
			<msg>Unimplemented command</msg>
		</result>
		<trID>
			<clTRID>ABC-12345</clTRID>
			<svTRID>54321-XYZ</svTRID>
		</trID>
	</response>
</epp>
```

<a id="transfer-contact"></a>
#### transfer contact

Transfer of contacts is not supported in the .dk registry.

When a [transfer domain](#transfer-domain) or [withdraw](#withdraw) operation is done. The contact object appointed to the registrar role is cloned and the clone is allocated to the new portfolio and the original contact object is left intact in the original portfolio.

<a id="host"></a>
### Host

The default behavior of the EPP `create host` command as described in [RFC:5732], will attach the client-ID (`CLID`) of the authenticated party to the created host object.

The `create host` command will be limited in that sense that it will not be possible to create a host object, which is subordinate to a domain name (superordinate), which is sponsored by another registrar.

This mean that the association with the registrar is based on the administrative model.

This limitation will only be enforced for domain names under the `.dk` TLD. Domain names under other TLDs will not be subject to this limitation.

As for the `create domain` and `create contact` commands (above) the default behaviour can be defined in RP. Where the option "registrant management", will create host objects sponsored by DK Hostmaster instead of the registrar.

Responsibility and privileges for maintenance (`update host`) of the host object is assigned to the name server administrator as described in the [create host section](#create-host) section.

If the name server responsible is allocated to the registrar account (group), this can be handled via RP and EPP.

The deletion of host objects are under a similar regime, as specified in the [delete host](#delete-host) section.

<a id="create-host"></a>
#### create host

This part of the EPP protocol is described in [RFC:5732]. This command adheres to the standard. The command can be extended to specify another name server administrator than the authenticated user.

:point_right: Please note that IP addresses are required for domain names ending in '.dk', please refer to the [glue record policy][DKHMDNSSPECGLUE].

:warning: By default the authenticated user is attempted used as designated name server administrator, It is however not possible to assign a registrar account as name server administrator, so a regular WHOIS handle pointing to a contact object has to be specified using the extension `dkhm:requestedNsAdmin`, alternatively you can authenticate using a WHOIS handle and the use of the extension can be avoided.

![Diagram of EPP create host][epp_create_host]

The command can be used in two scenarios:

1. The command is used as described in the RFC and the authenticated user is appointed as administrator for the name server created
2. The command is extended with a contact object pointing to an existing user, which is requested to take the role as name server administrator for the host object requested created

| Return Code | Description |
| ----------- | ----------- |
| 1000        | If the create host command is successful |
| 1001        | If the create host command awaits acknowledgement by the contact-id specified in `dkhm:requestedNsAdmin` |
| 2003        | If required IP address is not specified |
| 2004        | If the specified IP addresses are non-public addresses |
| 2005        | Syntax of the command is not correct |
| 2201        | If the authenticated user does not hold the privilege to update the specified host object |
| 2302        | If the specified host object already exist |
| 2303        | If the contact-id pointed to in `dkhm:requestedNsAdmin` points to a non-existing contact object |
| 2303        | If the domain name for the host is not registered |
| 2306        | If the specified name server administrator is a registrar account |

As for update domain `1001` holds higher precedence than `1000`, so if any of the sub-commands require additional review and are _pending_, the return code will be `1001`.

![Diagram of DKH create host][dkh_create_host]

<a id="create-host-request"></a>
##### create host request

Request to create a host object, using both IPv4 and IPv6 addresses and the authenticated user is the registrant of the specified domain name and requested administrator of the host object.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<create>
			<host:create
			 xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name>ns1.eksempel.dk</host:name>
				<host:addr ip="v4">192.0.2.2</host:addr>
				<host:addr ip="v4">192.0.2.29</host:addr>
				<host:addr ip="v6">1080:0:0:0:8:800:200417A</host:addr>
			</host:create>
		</create>
		<clTRID>ABC-12345</clTRID>
	</command>
</epp>
```

<a id="create-host-response"></a>
##### create host response

Response to the above request. The response indicates a successful creation, since the operation could be completed successfully without requiring offline evaluation.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<response>
		<result code="1000">
			<msg>Command completed successfully</msg>
		</result>
		<resData>
			<host:creData
			 xmlnhost="urn:ietf:params:xml:ns:host-1.0">
				<host:name>ns1.eksempel.dk</host:name>
				<host:crDate>1999-04-03T22:00:00.0Z</host:crDate>
			</host:creData>
		</resData>
		<trID>
			<clTRID>ABC-12345</clTRID>
			<svTRID>54322-XYZ</svTRID>
		</trID>
	</response>
</epp>
```

<a id="create-host-request-with-request-to-new-administrator"></a>
##### create host request with request to new administrator

Request to create a host object, requesting a different administrator of the host object, hence requiring offline evaluation.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<create>
			<host:create
			 xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name>ns1.eksempel.dk</host:name>
				<host:addr ip="v4">192.0.2.2</host:addr>
				<host:addr ip="v4">192.0.2.29</host:addr>
				<host:addr ip="v6">1080:0:0:0:8:800:200417A</host:addr>
			</host:create>
		</create>
		<extension>
			<dkhm:requestedNsAdmin xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3">ADMIN2-DK</dkhm:requestedNsAdmin>
		</extension>
		<clTRID>ABC-12345</clTRID>
	</command>
</epp>
```

<a id="create-host-response-from-request-to-new-administrator"></a>
##### create host response from request to new administrator

Response to the above request. The response indicates a successful accept of the request, but requires offline evaluation by the designated administrator of the host object, so the response indicates that the operation is pending.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<response>
		<result code="1001">
			<msg>Command completed successfully; action pending</msg>
		</result>
		<resData>
			<host:creData
			 xmlnhost="urn:ietf:params:xml:ns:host-1.0">
				<host:name>ns1.eksempel.dk</host:name>
				<host:crDate>1999-04-03T22:00:00.0Z</host:crDate>
			</host:creData>
		</resData>
		<trID>
			<clTRID>ABC-12345</clTRID>
			<svTRID>54322-XYZ</svTRID>
		</trID>
	</response>
</epp>
```

<a id="delayed-create-host-response-from-request-to-new-administrator"></a>
##### Delayed create host response, from request to new administrator

If the creation of the host has resulting in a delayed operation, pending the designated name server administrator, the below example shows what a poll message for the final state of the operation would look like.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="5" id="12345">
			<qDate>1999-04-04T22:01:00.0Z</qDate>
			<msg>Pending action completed successfully.</msg>
		</msgQ>
		<resData>
			<host:panData
			 xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name paResult="1">ns1.eksempel.dk</host:name>
				<host:paTRID>
					<clTRID>ABC-12345</clTRID>
					<svTRID>54322-XYZ</svTRID>
				</host:paTRID>
				<host:paDate>1999-04-04T22:00:00.0Z</host:paDate>
			</host:panData>
		</resData>
		<trID>
			<clTRID>BCD-23456</clTRID>
			<svTRID>65432-WXY</svTRID>
		</trID>
	</response>
</epp>
```

Please note the `paResult`, where `1` indicates an accept and `0` would indicate a decline.

<a id="create-host-request-with-request-to-registrant-of-host-domain-name"></a>
##### create host request, with request to registrant of host domain name

Request to create a host object, where the authenticated use is not the registrant of the domain name naming the host object, hence requiring offline evaluation.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<create>
			<host:create
			 xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name>ns1.eksempel.dk</host:name>
				<host:addr ip="v4">192.0.2.2</host:addr>
				<host:addr ip="v4">192.0.2.29</host:addr>
				<host:addr ip="v6">1080:0:0:0:8:800:200417A</host:addr>
			</host:create>
		</create>
		<clTRID>ABC-12345</clTRID>
	</command>
</epp>
```

<a id="create-host-response-from-request-to-registrant-of-domain-name"></a>
##### create host response, from request to registrant of domain name

Response to the above request. The response indicates a successful accept of the request, but requires offline evaluation by the registrant of the specified domain name, so the response indicates that the operation is pending.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<response>
		<result code="1001">
			<msg>Command completed successfully; action pending</msg>
		</result>
		<resData>
			<host:creData
			 xmlnhost="urn:ietf:params:xml:ns:host-1.0">
				<host:name>ns1.eksempel.dk</host:name>
				<host:crDate>1999-04-03T22:00:00.0Z</host:crDate>
			</host:creData>
		</resData>
		<trID>
			<clTRID>ABC-12345</clTRID>
			<svTRID>54322-XYZ</svTRID>
		</trID>
	</response>
</epp>
```

<a id="delayed-create-host-response-from-request-to-registrant-of-domain-name"></a>
##### Delayed create host response, from request to registrant of domain name

If the creation of the host has resulting in a delayed operation, pending the designated name server administrator, the below example shows what a poll message for the final state of the operation would look like.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="5" id="12345">
			<qDate>1999-04-04T22:01:00.0Z</qDate>
			<msg>Pending action completed successfully.</msg>
		</msgQ>
		<resData>
			<host:panData
			 xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name paResult="1">ns1.eksempel.dk</host:name>
				<host:paTRID>
					<clTRID>ABC-12345</clTRID>
					<svTRID>54322-XYZ</svTRID>
				</host:paTRID>
				<host:paDate>1999-04-04T22:00:00.0Z</host:paDate>
			</host:panData>
		</resData>
		<trID>
			<clTRID>BCD-23456</clTRID>
			<svTRID>65432-WXY</svTRID>
		</trID>
	</response>
</epp>
```

Please note the `paResult`, where `1` indicates an accept and `0` would indicate a decline.

<a id="check-host"></a>
### check host

This part of the EPP protocol is described in [RFC:5732]. This command adheres to the standard.

<a id="check-host-request"></a>
#### check host request

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<command>
		<check>
			<host:check xmlns:host="urn:ietf:params:xml:ns:host-1.0" xsi:schemaLocation="urn:ietf:params:xml:ns:host-1.0 host-1.0.xsd">
				<host:name>ns1.dk-hostmaster.dk</host:name>
			</host:check>
		</check>
		<clTRID>7ede02eed2113c5fe82b404876f2c35f</clTRID>
	</command>
</epp>
```

<a id="check-host-response"></a>
#### check host response

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1000">
			<msg>Check result</msg>
		</result>
		<resData>
			<host:chkData xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:cd>
					<host:name avail="0">ns1.dk-hostmaster.dk</host:name>
					<host:reason>In use</host:reason>
				</host:cd>
			</host:chkData>
		</resData>
		<trID>
			<clTRID>7ede02eed2113c5fe82b404876f2c35f</clTRID>
			<svTRID>5FD9F3BE-F6F6-11E3-867F-A6B052036DCB</svTRID>
		</trID>
	</response>
</epp>
```

<a id="info-host"></a>
### info host

This part of the EPP protocol is described in [RFC:5732]. This command adheres to the standard.

Please note that according to the RFC [section 3.1.2][RFC:5732-3.1.2], the `CLID` points to the sponsoring client.

This field supports the two administrative models as follows:

- For registrar managed host object, the `CLID` points to the registrar, see also [Disclosure of Client ID](#disclosure-of-client-id)
- For registrar managed host objects, DK Hostmaster interprets this as the technical contact for the name server identified by the host object

<a id="info-host-request"></a>
#### info host request

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<command>
		<info>
			<host:info xmlns:host="urn:ietf:params:xml:ns:host-1.0" xsi:schemaLocation="urn:ietf:params:xml:ns:host-1.0 host-1.0.xsd">
				<host:name>ns1.dk-hostmaster.dk</host:name>
			</host:info>
		</info>
		<clTRID>c109ef580c81dfca17b4680ddcde72c9</clTRID>
	</command>
</epp>
```

<a id="info-host-response"></a>
#### info host response

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1000">
			<msg>Info result</msg>
		</result>
		<resData>
			<host:infData xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name>ns1.dk-hostmaster.dk</host:name>
				<host:roid>NS1_DK-HOSTMASTER_DK-DK</host:roid>
				<host:status s="linked" />
				<host:status s="serverDeleteProhibited" />
				<host:addr ip=“v4”>4.3.2.1</host:addr>
				<host:clID>DKHM1-DK</host:clID>
				<host:crID>n/a</host:crID>
				<host:crDate>2003-07-07T13:47:47.0Z</host:crDate>
			</host:infData>
		</resData>
		<trID>
			<clTRID>c109ef580c81dfca17b4680ddcde72c9</clTRID>
			<svTRID>0C96C812-F6F6-11E3-867F-A6B052036DCB</svTRID>
		</trID>
	</response>
</epp>
```

<a id="update-host"></a>
#### update host

This part of the EPP protocol is described in [RFC:5732]. This command adheres to the standard, but is extended to service one special usage scenario.

<a id="process"></a>
##### process

This is the overall process, the process is divided into sub-processes, please see the processes below for details.

![Diagram of EPP update host][epp_update_host]

<a id="change-hostname-sub-process"></a>
##### Change hostname sub-process

The process of changing a host name is unsupported by DK Hostmaster and will always result in an error code: `2102`.

![Diagram of EPP update host change hostname][epp_update_host_change_hostname]

| Return Code | Description |
| ----------- | ------------ |
| 2102        | Change of hostname is not supported |

<a id="add-ip-address-sub-process"></a>
##### Add IP Address sub-process

Addition of IP addressed supports the additional of IPv4 and IPv6 addresses. These might be required as part of our [glue record policy][DKHMDNSSPECGLUE]. If additional status elements are added to this command it will fail.

| Return Code | Description |
| ----------- | ------------ |
| 1000        | If the update host command is successful |
| 2004        | If the specified IP addresses are non-public addresses  |
| 2005        | Syntax of the command is not correct |
| 2102        | Change of status for host object is not supported |

![Diagram of EPP update host add IP][epp_update_host_add_ip]

<a id="remove-ip-address-sub-process"></a>
##### Remove IP Address sub-process

Addition of IP addressed supports the additional of IPv4 and IPv6 addresses. These might be required as part of our [glue record policy][DKHMDNSSPECGLUE]. If additional status elements are added to this command it will fail.

| Return Code | Description |
| ----------- | ------------ |
| 1000        | If the update host command is successful |
| 2005        | Syntax of the command is not correct |
| 2102        | The command contains status elements |
| 2304        | The number of IP addresses are below the required limit |

![Diagram of EPP update host remove IP][epp_update_host_remove_ip]

<a id="change-admin-sub-process"></a>
##### Change admin sub-process

![Diagram of EPP update host change admin][epp_update_host_change_admin]

The command can be used in two scenarios:

1. The command is used as described in the RFC and IP addresses can be administered
2. The command is extended with a contact object pointing to an existing user, which is requested to takeover the role as name server administrator for the host object requested updated

The update of a host object can only be requested by the administrator of the given host.

| Return Code | Description |
| ----------- | ------------ |
| 1000        | If the update host command is successful |
| 1001        | If the update host command awaits acknowledgement by the contact-id specified in `dkhm:requestedNsAdmin` |
| 2004        | If the specified IP addresses are non-public addresses  |
| 2005        | Syntax of the command is not correct |
| 2102        | The command contains status elements |
| 2201        | If the authenticated user does not hold the privilege to update the specified host object |
| 2303        | If the specified host object does not exist |
| 2303        | If the contact-id pointed to in `dkhm:requestedNsAdmin` points to a non-existing contact object |
| 2304        | The number of IP addresses are below the required limit |

As for update host `1001` holds higher precedence than `1000`, so if any of the sub-commands require additional review and are _pending_, the return code will be `1001`.

As described in Implementation Limitations, the service does not support setting of status via update host.

![Diagram of DKH update host][dkh_update_host]

<a id="update-host-request-with-request-to-new-administrator"></a>
##### update host request with request to new administrator

Request to update a host object, requesting a different administrator of the host object, hence requiring offline evaluation.

```XML
<?xml version="1.0" encoding="UTF-8"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<update>
			<host:update xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name>ns1.eksempel.dk</host:name>
			</host:update>
		</update>
		<extension>
			<dkhm:requestedNsAdmin xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.3">DKHM1-DK</dkhm:requestedNsAdmin>
		</extension>
		<clTRID>7a4ac69d335ae661e29fc2c262c5800e</clTRID>
	</command>
</epp>
```

<a id="update-host-response-with-request-to-new-administrator"></a>
##### update host response with request to new administrator

Response to the above request. The response indicates a successful accept of the request, but requires offline evaluation by the designated administrator of the host object, so the response indicates that the operation is pending.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1001">
			<msg>Command completed successfully; action pending</msg>
		</result>
		<trID>
			<clTRID>6e95dc191e922be727fd5af4c2d20bc5</clTRID>
			<svTRID>631DABC6-CC49-11E6-A165-4F7D3A107CA1</svTRID>
		</trID>
	</response>
</epp>
```

<a id="delayed-update-host-response-from-request-to-new-administrator"></a>
##### Delayed update host response from request to new administrator

If the creation of the host has resulting in a delayed operation, pending the designated name server administrator, the below example shows what a poll message for the final state of the operation looks like.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="5" id="12345">
			<qDate>1999-04-04T22:01:00.0Z</qDate>
			<msg>Pending action completed successfully.</msg>
		</msgQ>
		<resData>
			<host:panData
			 xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name paResult="1">ns1.example.com</host:name>
				<host:paTRID>
					<clTRID>ABC-12345</clTRID>
					<svTRID>54322-XYZ</svTRID>
				</host:paTRID>
				<host:paDate>1999-04-04T22:00:00.0Z</host:paDate>
			</host:panData>
		</resData>
		<trID>
			<clTRID>BCD-23456</clTRID>
			<svTRID>65432-WXY</svTRID>
		</trID>
	</response>
</epp>
```

Please note the `paResult`, where `1` indicates an accept and `0` would indicate a decline.

<a id="delete-host"></a>
#### delete host

This part of the EPP protocol is described in [RFC:5732]. This command adheres to the standard.

![Diagram of EPP delete host][epp_delete_host]

The deletion of a host object can only be requested by the administrator.

| Return Code | Description |
| ----------- | ------------ |
| 1000        | If the delete host command is successful |
| 2201        | If the authenticated user does not hold the privilege to delete the specified host object |
| 2303        | If the specified host object does not exist |
| 2305        | If the specified host object links to domain name objects |

<a id="delete-host-request"></a>
##### delete host request

Request to delete a host object, the authenticated user is the current administrator of the specified host object.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<delete>
			<host:delete
			 xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name>ns1.eksempel.dk</host:name>
			</host:delete>
		</delete>
		<clTRID>ABC-12345</clTRID>
	</command>
</epp>
```

<a id="delete-host-response"></a>
##### delete host response

Response to the above request. Since the authenticated user is the current administrator and all requirements are met the command completes successfully.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<response>
		<result code="1000">
			<msg>Command completed successfully</msg>
		</result>
		<trID>
			<clTRID>ABC-12345</clTRID>
			<svTRID>54321-XYZ</svTRID>
		</trID>
	</response>
</epp>
```

<a id="data-collection-policy"></a>
## Data Collection Policy

This chapter describes the data collection policy announced via the greeting available using the hello command.

Please refer to the [greeting response example](#greeting) included in the [Appendices](#Appendices).

<a id="access"></a>
### Access

The EPP service provides access to identified data relating to all available entities (personal and organizational) under the terms and conditions that anonymity will be applied as specified by the entities in question, and in accordance with [General Terms and Conditions][DKHMTAC] and legislation.

<a id="purpose-statement"></a>
### Purpose Statement

The collected data will be used solely for provisioning and administrative purposes. As specified under access above, and in the recipient statement below, some data are required to be publicly available and therefore some data will be accessible to the public under the circumstances specified in the referred sections.

Address data and contact information is collected as required by Danish legislation.

<a id="recipient-statement"></a>
### Recipient Statement

Recipients of data are specified as other and unrelated. As specified in the purpose statement section and under access, identified data is made publicly available, therefore DK Hostmaster will not be able to control how the publicly available information is used.

<a id="retention-statement"></a>
### Retention Statement

Data will be retained with DK Hostmaster as required by Danish legislation.

<a id="references"></a>
## References

List of references used in this document in alphabetical order.

1. [DK Hostmaster: "Terms and conditions for the right of use to a .dk domain name"][DKHMTAC]
1. [DK Hostmaster: "New basis for collaboration between registrars and DK Hostmaster"][CONCEPT]
1. [DK Hostmaster: EPP General Information][DKHMEPP]
1. [DK Hostmaster: ID-control General Information][DKHMIDENT]
1. [DK Hostmaster: Waiting list General Information][DKHMWAITLIST]
1. [DK Hostmaster: Name Service Specification][DKHMDNSSPEC]
1. [DK Hostmaster: RESTful WHOIS Service Specification][DKHMWHOISRESTSPEC]
1. [DK Hostmaster: WHOIS Service Specification][DKHMWHOISSPEC]
1. [DK Hostmaster: Sandbox Environment Specification][DKHMSANDBOX]
1. [DK Hostmaster: EPP XSD File Repository][DKHMXSD]
1. [ICANN: "EPP Status Codes | What Do They Mean, and Why Should I Know?"][ICANN]
1. [IANA: "Extensions for the Extensible Provisioning Protocol (EPP)"][IANA]
1. [RFC:3339: "Date and Time on the Internet: Timestamps"][RFC:3339]
1. [RFC:3735: "Guidelines for Extending Extensible Provisioning Protocol"][RFC:3735]
1. [RFC:3915: "Domain Registry Grace Period Mapping for the Extensible Provisioning Protocol (EPP)"][RFC:3915]
1. [RFC:5730: "EPP Basic Protocol"][RFC:5730]
1. [RFC:5731: "EPP Domain Name Mapping"][RFC:5731]
1. [RFC:5732: "EPP Host Mapping"][RFC:5732]
1. [RFC:5733: "EPP Contact Mapping"][RFC:5733]
1. [RFC:5910: "Domain Name System (DNS) Security Extensions for the Extensible Provisioning Protocol"][RFC:5910]
1. [RFC:7451: "Extension Registry for the Extensible Provisioning Protocol"][RFC:7451]
1. [RFC:8748: "Registry Fee Extension for the Extensible Provisioning Protocol (EPP)"][RFC:8748]
1. [Verisign: "Balance Mapping for the Extensible Provisioning Protocol (EPP)"][BALANCE]

<a id="resources"></a>
## Resources

A list of resources for DK Hostmaster EPP service support is located below.

<a id="xsd-xml-schemas"></a>
### XSD/XML Schemas

This is a list in alphabetical order of the schemas currently used in the DKHM EPP Service described in this document. Please note that the XSD implementation preserves the original namespace and does not make alterations to this apart from adding the already described XML elements.

- `balance-1.0.xsd`
- `contact-1.0.xsd`
- `dkhm-3.0.xsd`
- `domain-1.0.xsd`
- `epp-1.0.xsd`
- `eppcom-1.0.xsd`
- `host-1.0.xsd`
- `rgp-1.0.xsd`
- `secDNS-1.1.xsd`

The files are all available for [download][DKHMXSD]. Details on version history is available in the [EPP XSD Repository][DKHMXSD]

<a id="mailing-list"></a>
### Mailing list

DK Hostmaster operates a mailing list for discussion and inquiries  about the DK Hostmaster EPP implementation. To subscribe to this list, write to the address below and follow the instructions. Please note that the list is for technical discussion only, any issues beyond the technical scope will not be responded to, please send these to the contact issue reporting address below and they will be passed on to the appropriate entities within DK Hostmaster.

- tech-discuss+subscribe@liste.dk-hostmaster.dk

<a id="issue-reporting"></a>
### Issue Reporting

For issue reporting related to this specification, the EPP implementation or test, sandbox or production environments, please contact us. You are of course welcome to post these to the mailing list mentioned above, otherwise use the regular support channels.

<a id="demotest-client"></a>
### Demo/Test Client

We have developed a demo/test client, which is freely available and open sourced under a MIT license.

The client is [available on GitHub][EPPDEMOCLIENT].

<a id="additional-information"></a>
### Additional Information

More generic information on EPP is available at the [DK Hostmaster website][DKHMEPP].

<a id="appendices"></a>
## Appendices

<a id="greeting"></a>
### Greeting

Do note the service version is available in the `svID` tag, meaning you can see what given version of the
EPP service is running in the environment queried.

```XML
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <greeting>
        <svID>DK Hostmaster EPP Service: 2.2.3</svID>
        <svDate>2016-12-27T15:19:26.0Z</svDate>
        <svcMenu>
            <version>1.0</version>
            <lang>en</lang>
            <objURI>urn:ietf:params:xml:ns:host-1.0</objURI>
            <objURI>urn:ietf:params:xml:ns:domain-1.0</objURI>
            <objURI>urn:ietf:params:xml:ns:contact-1.0</objURI>
            <svcExtension>
                <extURI>urn:ietf:params:xml:ns:secDNS-1.1</extURI>
                <extURI>urn:dkhm:params:xml:ns:dkhm-4.3</extURI>
            </svcExtension>
        </svcMenu>
        <dcp>
            <access>
                <personalAndOther/>
            </access>
            <statement>
                <purpose>
                    <admin/>
                    <prov/>
                </purpose>
                <recipient>
                    <other/>
                    <unrelated/>
                </recipient>
                <retention>
                    <legal/>
                </retention>
            </statement>
        </dcp>
    </greeting>
</epp>
```

<a id="status-codes"></a>
### Status Codes

<a id="domain-status-codes"></a>
#### Domain Status Codes

This list of EPP domain status codes is based on information from [RFC:5731] and the ICANN status code interpretation: ["EPP Status Codes | What Do They Mean, and Why Should I Know?"][ICANN].

The description is the status, use and interpretation by DK Hostmaster.

As a general business rule, DK Hostmaster does not support the `client*` statuses, see also: [Unsupported Domain Status Codes](#unsupported-domain-status-codes) in the [Implementation Limitations](#implementation-limitations) section.

| Status Code                | Description  |
| -------------------------- | ------------ |
| `addPeriod`                | *unsupported* the status is not described in [RFC:5731] only in [ICANN resource][ICANN] |
| `autoRenewPeriod`          | *unsupported* the status is not described in [RFC:5731] only in [ICANN resource][ICANN] |
| `clientDeleteProhibited`   | *unsupported* |
| `clientHold`               | *unsupported* |
| `clientRenewProhibited`    | *unsupported* |
| `clientTransferProhibited` | *unsupported* |
| `clientUpdateProhibited`   | *unsupported* |
| `inactive`                 | *unsupported* domain names in the DK Hostmaster registry **must** have associated name servers |
| `ok`                       | Exclusive for all other status codes |
| `pendingCreate`            | Indication that a the given domain is enqueue for possible creation, see [domain create](#domain-create) or is awaiting allocation with DK Hostmaster |
| `pendingDelete`            | Deletion is pending, see [domain create](#domain-create). An advisory date is applicable, see: [`dkhm:delDate`](dkhmdeldate) |
| `pendingRenew`             | *unsupported* as renewal is instantaneous |
| `pendingRestore`           | *unsupported* as restoration is instantaneous |
| `pendingTransfer`          | *unsupported* as transfer is instantaneous |
| `pendingUpdate`            | The domain has active asynchronous requests, see [domain update](#domain-update) |
| `redemptionPeriod`         | This status is applied when a domain name has `pendingDelete` and the delete operation can be redeemed |
| `renewPeriod`              | *unsupported* the status is not described in [RFC:5731] |
| `serverDeleteProhibited`   | Indicates whether the registrant or registrar can delete the domain |
| `serverHold`               | Given domain name is not active, it can hold a number of different _internal_ states rendering it on hold |
| `serverRenewProhibited`    | Indicates a transient status where the billing or registrar contact is not able to renew the domain |
| `serverTransferProhibited` | Indicates status where the registrant or registrar contact is not able to transfer the domain |
| `serverUpdateProhibited`   | Indicates whether the registrant or registrar for a given domain can have ownership transferred, can appoint new proxy/admin contact, can appoint new billing contact, change name servers and can associate DS Records |
| `transferPeriod`           | *unsupported* the status is not described in [RFC:5731] only in [ICANN resource][ICANN] |

<a id="contact-status-codes"></a>
#### Contact Status Codes

This list of EPP contact status codes is based on information from [RFC:5733].

The description is the status, use and interpretation by DK Hostmaster.

As a general business rule, DK Hostmaster does not support the `client*` statuses, see also: [Unsupported Contact Status Codes](#unsupported-contact-status-codes) in the [Implementation Limitations](#implementation-limitations) section.

| Status Code                | Description   |
| -------------------------- | ------------- |
| `clientDeleteProhibited`   | *unsupported* |
| `clientTransferProhibited` | *unsupported* |
| `clientUpdateProhibited`   | *unsupported* |
| `linked`                   | Object is linked to other objects |
| `ok`                       | Exclusive for all other status codes, except `linked` |
| `pendingCreate`            | *unsupported* as creation is instantaneous |
| `pendingDelete`            | *unsupported* |
| `pendingTransfer`          | *unsupported* as transfer is instantaneous |
| `pendingUpdate`            |               |
| `serverDeleteProhibited`   | if a user is use (_linked_) it cannot be deleted, the delete command is not supported |
| `serverTransferProhibited` | *unsupported* as users cannot be transferred |
| `serverUpdateProhibited`   | Object cannot be updated currently |

<a id="host-status-codes"></a>
#### Host Status Codes

This list of EPP host status codes is based on information from [RFC:5732].

The description is the status, use and interpretation by DK Hostmaster.

As a general business rule, DK Hostmaster does not support the `client*` statuses, see also: [Unsupported Host Status Codes](#unsupported-host-status-codes) in the [Implementation Limitations](#implementation-limitations) section.

| Status Code              | Description   |
| ------------------------ | ------------- |
| `clientDeleteProhibited` | *unsupported* |
| `clientUpdateProhibited` | *unsupported* |
| `linked`                 | Object is linked to other objects |
| `ok`                     | No pending or prohibited operations. Exclusive for all other status codes, except `linked` |
| `pendingCreate`          | Awaiting accept from registrant if required |
| `pendingDelete`          | Host object has been marked for deletion via deletion of superordinate domain name |
| `pendingTransfer`        | *unsupported* as transfer is instantaneous |
| `pendingUpdate`          | Awaiting accept from appointed name server administrator  |
| `serverDeleteProhibited` | If the host is linked is it not eligible for deletion |
| `serverUpdateProhibited` | If the host is marked for deletion (see `pendingDelete` this status will be set |

<a id="privilege-matrix-registrant-managed-objects"></a>
### Privilege Matrix for Registrant Managed Objects

| Command                           | Sub-command            | Registrar              | Domain name admin      | Domain name billing    | Name server admin           |
| --------------------------------- | ---------------------- |:----------------------:|:----------------------:|:----------------------:|:---------------------------:|
| [login](#login)                   |                        | :white_check_mark:     | :white_check_mark: \*1 | :white_check_mark: \*1 | :white_check_mark: \*1      |
| [create domain](#create-domain)   |                        | :white_check_mark:     |                        |                        |                             |
| [update domain](#update-domain)   |                        |                        | :white_check_mark: \*2 |                        | :white_check_mark: \*2      |
|                                   | add billing contact    | :white_check_mark: \*8 | :white_check_mark: \*3 |                        |                             |
|                                   | remove billing contact | :white_check_mark: \*4 | :white_check_mark: \*4 | :white_check_mark: \*4 |                             |
|                                   | add admin contact      |                        | :white_check_mark: \*5 |                        |                             |
|                                   | remove admin contact   |                        | :white_check_mark: \*4 |                        |                             |
|                                   | change registrant      |                        | :white_check_mark: \*6 |                        |                             |
|                                   | add name server        |                        | :white_check_mark: \*6 |                        | :white_check_mark: \*6/\*10 |
|                                   | remove name server     |                        | :white_check_mark: \*6 |                        | :white_check_mark: \*6/\*10 |
|                                   | add DSRECORDS          |                        | :white_check_mark:     |                        | :white_check_mark: \*10     |
|                                   | remove DSRECORDS       |                        | :white_check_mark:     |                        | :white_check_mark: \*10     |
| [renew domain](#renew-domain)     |                        |                        |                        | :white_check_mark:     |                             |
| [delete domain](#delete-domain)   |                        |                        | :white_check_mark: \*6 |                        |                             |
| [restore domain](#restore-domain) |                        |                        | :white_check_mark:     |                        |                             |
| [info domain](#info-domain)       |                        | :white_check_mark: \*9 | :white_check_mark: \*9 | :white_check_mark: \*9 | :white_check_mark: \*9      |
| [check domain](#check-domain)     |                        | :white_check_mark:     | :white_check_mark:     | :white_check_mark:     | :white_check_mark:          |
| [create contact](#create-contact) |                        | :white_check_mark:     | :white_check_mark:     | :white_check_mark:     | :white_check_mark:          |
| [update contact](#update-contact) |                        | :white_check_mark: \*7 |                        |                        | :white_check_mark: \*7      |
| [delete contact](#delete-contact) |                        |                        |                        |                        |                             |
| [info contact](#info-contact)     |                        | :white_check_mark: \*9 | :white_check_mark: \*9 | :white_check_mark: \*9 | :white_check_mark: \*9      |
| [check contact](#check-contact)   |                        | :white_check_mark:     | :white_check_mark:     | :white_check_mark:     | :white_check_mark:          |
| [create host](#create-host)       |                        | :white_check_mark:     |                        |                        | :white_check_mark:          |
| [update host](#update-host)       |                        |                        |                        |                        | :white_check_mark:          |
| [delete host](#delete-host)       |                        |                        |                        |                        | :white_check_mark:          |
| [info host](#info-host)           |                        | :white_check_mark:     |                        |                        | :white_check_mark:          |
| [check host](#check-host)         |                        | :white_check_mark:     |                        |                        | :white_check_mark:          |

- \*1 as registrar, meaning a user associated with a registrar group
- \*2 see sub-commands
- \*3 request to new billing contact
- \*4 defaults to registrant
- \*5 request to to registrant and new admin contact
- \*6 request to registrant
- \*7 only own profile
- \*8 can only assign self
- \*9 can only see contact information for authorized objects, access to registrant is authorized as public other roles require authorization via relation
- \*10 changes status of existing DSRECORDS

<a id="privilege-matrix-registrar-managed-objects"></a>
### Privilege Matrix for Registrar Managed Objects

| Command                           | Sub-command            | Registrar              | Name server admin      |
| --------------------------------- | ---------------------- |:----------------------:|:----------------------:|
| [login](#login)                   |                        | :white_check_mark:     | :white_check_mark: \*1 |
| [create domain](#create-domain)   |                        | :white_check_mark:     |                        |
| [update domain](#update-domain)   |                        | :white_check_mark: \*2 | :white_check_mark: \*2 |
|                                   | add billing contact    |                        |                        |
|                                   | remove billing contact |                        |                        |
|                                   | add admin contact      |                        |                        |
|                                   | remove admin contact   |                        |                        |
|                                   | change registrant      | :white_check_mark:     |                        |
|                                   | add name server        | :white_check_mark: \*3 | :white_check_mark: \*3 |
|                                   | remove name server     | :white_check_mark: \*3 | :white_check_mark: \*3 |
|                                   | add DSRECORDS          | :white_check_mark: \*3 | :white_check_mark: \*3 |
|                                   | remove DSRECORDS       | :white_check_mark: \*3 | :white_check_mark: \*3 |
| [renew domain](#renew-domain)     |                        | :white_check_mark: \*7 |                        |
| [delete domain](#delete-domain)   |                        | :white_check_mark: \*7 |                        |
| [restore domain](#restore-domain) |                        | :white_check_mark: \*7 |                        |
| [info domain](#info-domain)       |                        | :white_check_mark: \*4 | :white_check_mark: \*4 |
| [check domain](#check-domain)     |                        | :white_check_mark:     | :white_check_mark:     |
| [create contact](#create-contact) |                        | :white_check_mark:     | :white_check_mark:     |
| [update contact](#update-contact) |                        | :white_check_mark: \*5 | :white_check_mark: \*6 |
| [delete contact](#delete-contact) |                        |                        |                        |
| [info contact](#info-contact)     |                        | :white_check_mark: \*4 | :white_check_mark: \*4 |
| [check contact](#check-contact)   |                        | :white_check_mark:     | :white_check_mark:     |
| [create host](#create-host)       |                        | :white_check_mark: \*8 |                        |
| [update host](#update-host)       |                        | :white_check_mark: \*8 | :white_check_mark: \*9 |
| [delete host](#delete-host)       |                        | :white_check_mark: \*8 | :white_check_mark: \*9 |
| [info host](#info-host)           |                        | :white_check_mark:     | :white_check_mark:     |
| [check host](#check-host)         |                        | :white_check_mark:     | :white_check_mark:     |

- \*1 as registrar, meaning a user associated with a registrar group
- \*2 see sub-commands
- \*3 changes status of existing DSRECORDS
- \*4 can only see contact information for authorized objects, access to registrant is authorized as public other roles require authorization via relation
- \*5 only data not locked by business rules and under external registry administration such as CPR, CVR and VIES registers
- \*6 only own profile
- \*7 registrar
- \*8 subordinate
- \*9 NSA

<a id="compatibility-matrix"></a>
### Compatibility Matrix

This is a high level overview of the EPP commands offered by the DK Hostmaster EPP service, please see the specific commands for details.

The version numbers used in the matrix are major numbers only, e.g. 1.X.X.

| EPP Command  | Available since version | Exceptions and notes |
| ------------ | ------------ | ------------ |
| [Log in](#login) | 1 | Only password authentication is supported |
| [Log out](#logout) | 1 | |
| [Create domain](#create-domain) | 1 | Asynchronous, requires order confirmation by the registrant. VID product not supported, PO numbers not supported |
| [Check Domain](#check-domain) | 1 | |
| [Info Domain](#info-domain) | 1 / 3 | Billing contact not disclosed, Admin contact not disclosed since version 3. EPP status codes not supported completely |
| [Update Domain](#update-domain) | 2 | Change of name server is asynchronous, requires approval by the registrant. Change of registrant is not supported |
| [Renew Domain](#renew-domain) | 2 | Requires that the requesting user is a registrar and billing contact for the domain. The domain name must not have any financial outstanding |
| [Transfer Domain](#transfer-domain) | 4 | |
| [Delete Domain](#delete-domain) | 4 | Only registrars administering the domain name |
| [Restore Domain](#restore-domain) | 4 | Only registrars administering the domain name |
| [Create Contact](#create-contact) | 1 | Supplied handle/user-id is not supported |
| [Check Contact](#check-contact) | 1 / 3 | Only registrants disclosed, additional contacts require relation to authenticated user |
| [Info Contact](#info-contact) | 1 / 3 | Only registrants disclosed, additional contacts require relation to authenticated user |
| [Update Contact](#update-contact) | 2 | Updating email is asynchronous and is regarded as non-atomic due to the email validation process |
| [Transfer Contact](#transfer-contact) | N/A | Contact objects cannot be transferred only domain names. Contact objects are cloned upon transfer |
| [Delete Contact](#delete-contact) | N/A | Deletion of contacts is an automated process |
| [Create Host](#create-host) | 2 | Asynchronous, requires accept of the registrant of the domain name if the domain is under the .dk TLD and requires that the requesting user accepts the responsibility as name server administrator |
| [Check Host](#check-host) | 1 | |
| [Info Host](#info-host) | 1 | |
| [Update Host](#update-host) | 2 |  Asynchronous, requires that the requested administrator accepts the responsibility as name server administrator |
| [Delete Host](#delete-host) | 2 | |
| [Poll](#poll-and-message-queue) | 1 | |
| [Balance](#balance-and-prepaid-account) | 4 | |

[DKHMLOGO]: https://www.dk-hostmaster.dk/sites/default/files/dk-logo_0.png
[GHAMKDBADGE]: https://github.com/DK-Hostmaster/epp-service-specification/workflows/Markdownlint%20Action/badge.svg
[GHASPLLBADGE]: https://github.com/DK-Hostmaster/epp-service-specification/workflows/Spellcheck%20Action/badge.svg

[epp-update-contact]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_contact_v1.0.png
[epp-role-resolution]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp-role-resolution_v1.0.png
[epp-address-resolution]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp-address-resolution_v1.0.png
[epp_create_host]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_create_host_v1.2.png
[dkh_create_host]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/dkh_create_host_v1.0.png
[epp_update_host]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_host_v1.2.png
[epp_update_host_add_ip]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_host_add_ip_v1.0.png
[epp_update_host_change_admin]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_host_change_admin_v1.0.png
[epp_update_host_change_hostname]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_host_change_hostname_v1.0.png
[epp_update_host_remove_ip]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_host_remove_ip_v1.0.png
[dkh_update_host]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/dkh_update_host_v1.0.png
[epp_delete_host]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_delete_host_v1.1.png
[dkh_delete_host]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/dkh_delete_host_v1.0.png
[epp-renew-domain]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_renew_domain_v1.1.png
[dkh-renew-domain]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/dkh_renew_domain_v1.1.png
[epp-update-domain]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_domain_v1.2.png
[epp-update-domain-evaluate]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_domain_evaluate_command_v1.0.png
[epp-update-domain-add-contact]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_domain_add_contact_v1.0.png
[dkh-update-domain-add-contact]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/dkh_update_domain_add_contact_v1.0.png
[epp-update-domain-remove-contact]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_domain_remove_contact_v1.1.png
[dkh-update-domain-remove-contact]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/dkh_update_domain_remove_contact_v1.0.png
[epp-update-domain-add-ns]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_domain_add_ns_v1.0.png
[epp-update-domain-remove-ns]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_domain_remove_ns_v1.1.png
[epp-update-domain-change-registrant]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_domain_change_registrant_v1.2.png
[epp_create_domain]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_create_domain_v1.0.png
[epp_create_contact]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_create_contact_v1.0.png
[RFC:3339]: https://tools.ietf.org/html/rfc3339
[RFC:3735]: https://tools.ietf.org/html/rfc3735
[RFC:3915]: https://tools.ietf.org/html/rfc3915
[RFC:5730]: https://tools.ietf.org/html/rfc5730
[RFC:5731]: https://tools.ietf.org/html/rfc5731
[RFC:5732]: https://tools.ietf.org/html/rfc5732
[RFC:5732-3.1.2]: https://tools.ietf.org/html/rfc5732#section-3.1.2
[RFC:5733]: https://tools.ietf.org/html/rfc5733
[RFC:5910]: https://tools.ietf.org/html/rfc5910
[RFC:5910-3.3]: https://tools.ietf.org/html/rfc5910#section-3.3
[RFC:7451]: https://tools.ietf.org/html/rfc7451
[RFC:8748]: https://tools.ietf.org/html/rfc8748
[IANA]: https://www.iana.org/assignments/epp-extensions/epp-extensions.xhtml
[ICANN]: https://www.icann.org/resources/pages/epp-status-codes-2014-06-16-en/
[ICANNserverRenewProhibited]: https://www.icann.org/resources/pages/epp-status-codes-2014-06-16-en/#serverRenewProhibited
[EAN]: https://en.wikipedia.org/wiki/International_Article_Number_(EAN)
[DKHMXSD]: https://github.com/DK-Hostmaster/epp-xsd-files
[DKHMDNSSPEC]: https://github.com/DK-Hostmaster/dkhm-name-service-specification
[DKHMDNSSPECGLUE]: https://github.com/DK-Hostmaster/dkhm-name-service-specification#glue-records
[DKHMEPPWIKI]: https://github.com/DK-Hostmaster/epp-service-specification/wiki
[EPOCH]: https://en.wikipedia.org/wiki/Unix_time
[CONCEPT]: https://www.dk-hostmaster.dk/en/new-basis-collaboration-between-registrars-and-dk-hostmaster
[DKHMWHOISSPEC]: https://github.com/DK-Hostmaster/whois-service-specification
[DKHMWHOISRESTSPEC]: https://github.com/DK-Hostmaster/whois-rest-service-specification
[BALANCE]: https://www.verisign.com/assets/epp-sdk/verisign_epp-extension_balance_v01.html
[DKHMWAITLIST]: https://www.dk-hostmaster.dk/en/waiting-list
[DKHMIDENT]: https://www.dk-hostmaster.dk/en/identification
[DKHMEPP]: https://www.dk-hostmaster.dk/en/epp
[DKHMSANDBOX]: https://github.com/DK-Hostmaster/sandbox-environment-specification
[EPPDEMOCLIENT]: https://github.com/DK-Hostmaster/epp-demo-client-mojolicious
[DKHMTAC]: https://www.dk-hostmaster.dk/en/general-conditions
[DKHMMAIL]: https://www.dk-hostmaster.dk/en/mailing-lists
