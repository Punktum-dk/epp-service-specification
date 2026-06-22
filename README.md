![Punktum dk Logo][DKHMLOGO]

# Punktum dk EPP Service Specification

![Markdownlint Action][GHAMKDBADGE]
![Spellcheck Action][GHASPLLBADGE]

2026-07-01 Revision: 5.3

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
  - [Session Termination Conditions](#session-termination-conditions)
  - [Available Environments](#available-environments)
    - [Production](#production)
    - [Sandbox](#sandbox)
- [Implementation Requirements](#implementation-requirements)
  - [Client Transaction ID \(`clTRID`\)](#client-transaction-id-cltrid)
  - [IP Whitelisting](#ip-whitelisting)
  - [Service Users](#service-users)
  - [Registrar Account Settings](#registrar-account-settings)
- [Implementation Extensions](#implementation-extensions)
  - [`dkhm:authInfo`](#dkhmauthinfo)
  - [`dkhm:autoRenew`](#dkhmautorenew)
  - [`dkhm:contact_validated`](#dkhmcontact_validated)
  - [`dkhm:contact_verification`](#dkhmcontactverification)
  - [`dkhm:CVR`](#dkhmcvr)
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
  - [`dkhm:sole_proprietorship`](#dkhmsoleproprietorship)
  - [`dkhm:trackingNo`](#dkhmtrackingno)
  - [`dkhm:userType`](#dkhmusertype)
  - [`dkhm:vid`](#dkhmvid)
- [Implementation Limitations](#implementation-limitations)
  - [Authentication](#authentication)
  - [AuthInfo](#authinfo)
  - [Contact Creation](#contact-creation)
  - [Contact Info](#contact-info)
  - [Contact Update](#contact-update)
  - [Disclosure of Client ID](#disclosure-of-client-id)
  - [DNSSEC](#dnssec)
  - [Domain Info](#domain-info)
  - [Domain Transfer](#domain-transfer)
  - [Domain Update](#domain-update)
  - [Encoding and IDN domains](#encoding-and-idn-domains)
  - [Host Info](#host-info)
  - [Host Update](#host-update)
  - [Information Disclosure](#information-disclosure)
  - [Unimplemented Commands](#unimplemented-commands)
    - [Unimplemented Command: Delete Contact](#unimplemented-command-delete-contact)
    - [Unimplemented Command: Transfer Contact](#unimplemented-command-transfer-contact)
  - [Unsupported Contact Status Codes](#unsupported-contact-status-codes)
  - [Unsupported Domain Status Codes](#unsupported-domain-status-codes)
  - [Unsupported Host Status Codes](#unsupported-host-status-codes)
  - [Waiting List](#waiting-list)
- [Supported Object Transform and Query Commands](#supported-object-transform-and-query-commands)
  - [hello and greeting](#hello-and-greeting)
  - [login](#login)
    - [login request](#login-request)
    - [login response](#login-response)
  - [logout](#logout)
    - [logout request](#logout-request)
    - [logout response](#logout-response)
  - [Poll and Message Queue](#poll-and-message-queue)
    - [poll req request](#poll-req-request)
    - [poll req response](#poll-req-response)
    - [poll ack request](#poll-ack-request)
    - [poll ack response](#poll-ack-response)
    - [poll ack response for non-existent message \(or previously acknowledged message\)](#poll-ack-response-for-non-existent-message-or-previously-acknowledged-message)
  - [Balance and Prepaid Account](#balance-and-prepaid-account)
    - [balance request](#balance-request)
    - [balance response](#balance-response)
  - [Domain](#domain)
    - [create domain](#create-domain)
      - [create domain request](#create-domain-request)
      - [create domain response](#create-domain-response)
      - [Poll and Messages](#poll-and-messages)
        - [create domain poll message for successful creation](#create-domain-poll-message-for-successful-creation)
        - [create domain poll message for successful creation with pending mandatory ID check](#create-domain-poll-message-for-successful-creation-pending-id-check)
        - [create domain poll message for unsuccessful creation, existing domain](#create-domain-poll-message-for-unsuccessful-creation-existing-domain)
        - [create domain poll message for unsuccessful creation, application cancelled](#create-domain-poll-message-for-unsuccessful-creation-cancelled)
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
      - [remove and add name server](#remove-and-add-name-server)
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
    - [transfer contact](#transfer-contact)
  - [Host](#host)
    - [create host](#create-host)
      - [create host request](#create-host-request)
      - [create host response](#create-host-response)
      - [create host request with request to new administrator](#create-host-request-with-request-to-new-administrator)
      - [create host response from request to new administrator](#create-host-response-from-request-to-new-administrator)
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
  - [Issue Reporting](#issue-reporting)
  - [Additional Information](#additional-information)
- [Appendices](#appendices)
  - [Status Codes](#status-codes)
    - [Domain Status Codes](#domain-status-codes)
    - [Contact Status Codes](#domain-status-codes)
    - [Host Status Codes](#domain-status-codes)
  - [Privilege Matrix for Registrant Managed Objects](#privilege-matrix-registrant-managed-objects)
  - [Privilege Matrix for Registrar Managed Objects](#privilege-matrix-registrar-managed-objects)
  - [Feature and Meta-role Matrix](#feature-and-meta-role-matrix)
  - [Compatibility Matrix](#compatibility-matrix)

<!-- /MarkdownTOC -->

<a id="introduction"></a>

## Introduction

This document outlines Punktum dk’s implementation of the Extensible Provisioning Protocol (EPP) for interaction with the central registry of the .dk country code top-level domain (ccTLD). It is intended for a technical audience with prior knowledge of domain name system (DNS) operations and EPP. 

<a id="about-this-document"></a>

### About this Document

About this Document 

This specification describes version 5.x.x of the Punktum dk EPP Implementation. Future releases will be reflected in updates to this document; please refer to the [Document History](#document-history) below for changes.

The document describes the current Punktum dk EPP implementation, for more general documentation on the EPP protocol, EPP client development or configuration, please refer to the RFCs and additional resources in the [References](#references) and [Resources](#resources) chapters below.

Do note that the specification aims to describes the latest release of the service. Service version is listed in the [Document History](#document-history),
so given changes implemented in the service are reflected in the specification. Do note that a service might be released to the sandbox environment
prior to being released to production after a grace period.

This document is not the authoritative source for business and policy rules and possible discrepancies between this and any authoritative sources are regarded as errors in this document. This document is aimed at being the external technical specification and describes the implementation facing the users and is an interpretation of authoritative sources and can therefor be erroneous.

The actively used XSD file is indicated in the [EPP service specification][DKHMEPPWIKI], the [EPP XSD file repository][DKHMXSD] might contain changes not actively used by the service.

The current service version can be obtained from the [Greeting](#greeting) message, from the service.

Any future extensions and possible additions and changes to the implementation are not within the scope of this document and will not be discussed or mentioned throughout this document.

The specification mentions the **Registrar Portal** service (RP), which complements the EPP service for consistency between services.

This document is owned and maintained by Punktum dk A/S and must not be distributed without this information.

All examples provided in the document are fabricated/modified from real data to demonstrate commands etc. any resemblance to actual data are coincidental.

Diagrams to support feature descriptions can be seen by clicking the :eye_speech_bubble: icon, where made available.

<a id="license"></a>

### License

This document is copyright by Punktum dk A/S and is licensed under the MIT License, please see the separate LICENSE file for details.

<a id="document-history"></a>

### Document History

- 5.3 2026-07-01
  - The [renew domain](#renew-domain) command now only supports a renewal period of one year. In addition, a domain name can no longer be renewed earlier than 60 days before its expiry date.
  - The Withdraw domain command has been removed and is no longer supported.
  - The [create domain](#create-domain) command now requires registrar management. It is no longer possible to create a domain name under registrant management.
  - The documentation for the following extensions has been removed, as they are no longer used. They remain present in the XSD for the time being:
    - `dkhm:authInfoExDate`
    - `dkhm:delDate`
    - `dkhm:url`
  - The entire specification has been reviewed and rewritten for clarity and consistency.

- 5.2.3 2026-05-20

  - Added section [Session Termination Conditions](#session-termination-conditions)

- 5.2.2 2026-03-11

  - Added poll messages regarding email bounce.
  - Wording changed on domain update registrant poll messages.

- 5.2.1 2026-02-16

  - Clarified interpretation of risk_assessment values.
  - Updated default behavior and documentation for [`dkhm:requestedNsAdmin`](#dkhmrequestednsadmin).

- 5.2 2025-12-23

  - Added two new extensions related to NIS2:
    - [`dkhm:sole_proprietorship`](#dkhmsoleproprietorship)
    - [`dkhm:contact_verification`](#dkhmcontactverification)
  - All phone numbers wil now be syntax validated in accordance with the rules dictated by the country code part of the phone number.
  - Added poll messages regarding mandatory verification of contact id and/or data (currently data only includes email) and required confirmation in order to change primary/secondary email.

- 5.1 2025-10-07

  - Use of the `dkhm:orderconfirmationToken` extension is now always required by the [change registrant](#change-registrant) command.
  - Is it now possible to use a REG-handle as an admin contact on a registranthandled domain. Previously this was only possible for the billing contact on a registranthandled domain.
  - It is now possible to use a REG-handle as a name server manager.

- 5.0.1 2025-09-24

  - The optional child element qDate was unintentionally removed from poll messages. It has now been readded.
  - Correction of the message returned if a [change registrant](#change-registrant) command results in a pending accept by the new registrant.
  - Updated the example for the [restore domain extension response](#restore-domain-response)

- 5.0 2025-09-02

  - Our EPP servers have been moved to a new and more futureproof technical platform.
  - The quality of data has been increased to ensure, to the extent possible, that data is compliant with the standards.
    - Optional fields, where we have no meaningful data in our database, have been removed from the data
    - Local timestamps, which we used to present as beeing UTC timestamps, even though they were not actually converted to UTC, are now correctly beeing converted to UTC
    - redemptionPeriod for a domain is now only set if it actually possible to restore the domain using the [restore domain](#restore-domain) extension
  - [Almost all poll messages have been revised](Poll-Message-Reference-Guide.md)
  - All poll messages will now include more complete and meaningfull resData, in this context a special focus has been on providing correct panData.
  - All examples have been updated to reflect the above changes in data.

- 4.4.1 2024-11-29

  - Corrected typos, dead links and/or misleading examples.
  - Added missing sections on [dkhm:authInfo](#dkhmauthinfo) and [dkhm:vid](#dkhmvid) extentions.
  - Updated section on [transfer domain](#transfer-domain) to clarify that `clientApproved` and `serverApproved` both are possible statuscodes
  - Updated examples of changing name servers on a domain to include the use of Authinfo token.

- 4.4 2021-12-06

  - Updated section on [AuthInfo Token Format](#authinfo-token-format) to describe new and improved format

- 4.3 2021-10-25

  - Added description of [limitations of domain transfer](#domain-transfer) in regard to the optional period field

- 4.2 2021-10-21
  - All diagrams moved out of the document and linked instead of displayed
  - Added section on [Service Users](#service-users) to the [Implementation Requirements](#implementation-requirements) section
  - Added section on [Registrar Account Settings](#registrar-account-settings) to the [Implementation Requirements](#implementation-requirements) section
  - Added [Contact Update](#contact-update) section under [Implementation Limitations](#implementation-limitations)
  - Overall clean up and clarification of the documentation and all [Appendices](#appendices)
    - Removing obsolete information
    - Clarifying business rules
  - Added example of first poll message to a [create domain](#create-domain), indicating the pending operation
  - Updated example of [info domain](#info-domain) response with information on the `AuthInfo` token and expiration date using the `dkhm:authInfoExDate` extension
  - Added missing example of [withdraw response](#withdraw-response)
  - Added overview of contact objects and information on locking of data, see [Contact](#contact)

- 4.1 2021-09-24
  - Added documentation for new error scenario for [create domain](#create-domain) for a registrar managed domain name, specifying other contacts than the registrant will result in an error `2306`
  - Added a description of possible challenge with auto matching user for [create contact](#create-contact), since ID-control can alter data as part of the validation
  - This revision of the specification describes version 4.0.2 of the EPP service

- 4.0 2021-09-19
  - Introduction of support for registrar/registrant administration
  - Outlined business rules for [`dkhm:orderconfirmationToken`](#dkhmorderconfirmationtoken)
  - The procedures for renewal and application/creation are not being changed, in regard to use and protocol, however
    - The business policies in relation to these operations, do however change, since the billing operation changes, please see the [create domain](#create-domain) and [renew domain](#renew-domain) commands
    - The introduction of registrar support influences the business rules for [create domain](#create-domain)
  - Added information on setting/unsetting autorizations using AuthInfo tokens, see [setting AuthInfo](#setting-authinfo) and [unsetting AuthInfo](#unsetting-authinfo)
  - Added information on `dkhm:management` extension for [create domain](#create-domain) and [create contact](#create-contact), which overrides account default
  - Added description of new and improved change name server process, both using authorization and under registrar administration
  - Added documentation on the extension to [info domain](#info-domain) with information on the `AuthInfo` expiration date using the `dkhm:authInfoExDate` extension
  - Added description of the changed [create contact](#create-contact) process, handling registrar administration
  - Introduction of support for [delete domain](#delete-domain) command
  - Introduction of support for [restore domain](#restore-domain) via [update domain](#update-domain) command
  - Introduction of support for [balance](#balance-and-prepaid-account) command
  - Introduction of support for [transfer domain](#transfer-domain) command
  - Added clarifications on status codes for domains, contacts and hosts
  - Removed XSD Version History, referencing original source in [EPP XSD repository][DKHMXSD], so there is a single source for this information
  - This version of the specification is based on the Punktum dk EPP XSD version 4.3
  - Addition of disclaimer, setting the scope and frame for this specification
  - Addition of [Feature and Meta-role Matrix](#feature-and-meta-role-matrix)
  - This revision of the specification describes version 4.0.0 of the EPP service

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
  - More information on extensions for possible registration of the Punktum dk extensions with IANA in relation to [RFC:7451]
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

Punktum dk is the registry for the Danish country-code top-level domain (.dk) and maintains the central DNS registry.

Due to Danish legislation and the applied registry model, certain restrictions apply compared to the general scope of the EPP protocol. These implementation limitations are described in detail in the [Implementation Limitations](#implementation-limitations) chapter and further explained in the individual command descriptions where deviations from the EPP standard occur. In addition, a small number of extensions have been introduced to support DNS registrations under Danish law, and these are documented under the relevant commands.

Punktum dk offer two management models, please see [the section on management models][CONCEPT] for details.

In brief the two models are:

- **Registrar managed**, where the registrar takes the administrative role of the domain name and the associated contact. Also referred to as “Registrar management”.
- **Registrant managed**, where the registrant administrates the domain and user directly through Punktum dk. Also referred to as “Registrant management”.

:warning: Please note that Punktum dk is currently in a transition period towards a fully registrar-managed model. As a result, it is only possible to create domains under the registrar-managed model.

The EPP service is the same, but the capabilities and business roles vary depending on choice of administrative model. This is described in detail under the different commands and outlined in:

- [Privilege Matrix for Registrant Managed Objects](#privilege-matrix-registrant-managed-objects)
- [Privilege Matrix for Registrar Managed Objects](#privilege-matrix-registrar-managed-objects)

Our EPP extensions are registered with the [IANA EPP Extension Repository][IANA] as by [RFC:7451].

<a id="epp-in-brief"></a>

## EPP in Brief

EPP is an XML-based protocol designed for provisioning data between domain name registries and registrars. It is intended for machine-to-machine communication in a client–server setup.

EPP is defined by the IETF in a set of RFCs, primarily [RFC 5730] (core protocol), along with object mappings such as [RFC 5731] (domains), [RFC 5732] (hosts), and [RFC 5733] (contacts). 

Using EPP, clients can perform operations such as creating, updating, transferring, and deleting domain names, as well as managing contacts and name servers. Communication is session-based and follows a request–response model, where commands are sent from the client and processed by the server. 

For detailed specifications and references, please refer to the [References](#references) chapter.

Please note that XML entity expansion is not supported on the server side, as this feature poses security risks. 

<a id="epp-service"></a>

## EPP Service

Punktum dk’s EPP service is designed as a provisioning interface for external parties needing to manage objects within Punktum dk.

Using the service requires an EPP client, which may need to be developed by the user. The creation or customization of EPP client software is outside the scope of this specification; this document focuses on the API and supporting resources.

To support integration, Punktum dk provides tools and documentation, including a sandbox environment, to help users and developers test and implement their EPP clients efficiently. 

The service follows these key principles:

- Standards Compliance: Follow the EPP standard wherever possible. Non-intrusive or mandatory extensions may be used to meet specific service requirements. 
- In-Band Communication: All requests sent via EPP are responded to via EPP, unless an alternative has been explicitly specified by the end-user. 
- Clear Error Handling: Standard EPP error codes are used wherever possible to ensure consistent and unambiguous communication of system state. 

<a id="ssltls-support"></a>

### SSL/TLS Support

The EPP service supports the following protocols for transport security:

- TLSv1.2

<a id="session-termination-conditions"></a>

### Session Termination Conditions

The server may disconnect the TCP connection in the following situations.

**Too many errors**

After three errors of the same type, the server disconnects the TCP connection.

The final response code is changed to indicate disconnect.
Example: `2400` → `2500`

The following text is appended to the result message:

```XML
; Disconnect due to too many errors
```

**Oversized Packet**

If a packet exceeds 262144 bytes (256 KB), the server disconnects immediately.

- No response packet is returned.
- The TCP connection is closed directly.

**Pending Server Shutdown**

When the server enters a pending shutdown state:

- No new commands are accepted.
- Already received commands are processed.
- The final response code is modified to indicate disconnect:

Examples:
- `1000` → `1500`
- `1001` → `1501`
- `2000` → `2500`
- `2303` → `2503`

The following text is appended to the result message:

```XML
; Disconnect due to server shutdown
```

<a id="available-environments"></a>

### Available Environments

Punktum dk offers the following two environments:

- Production
- Sandbox

Updates to both environments are announced via our statuspage and the registrar newsletters.

Please see the [information page][DKHMMAIL] for details on subscribing etc.

<a id="production"></a>

#### Production

- Please see [EPP service specification Wiki][DKHMEPPWIKI] for up to date information for the production environment.

**Production environment is accessible at**:

- `epp.dk-hostmaster.dk`
- port `700`

The EPP production environment is the live environment where registrars perform real operations. It is protected by IP whitelisting, meaning only authorized registrars have access.

Info and check commands return actual production data. Create requests are carried out if they comply with business rules and general terms. Approved domains may be activated and propagated into the zone.

Contacts (users) created in this environment become available in other systems, including the self-service portal. Hosts (name servers) are also processed for possible activation.

The Change Password operation is supported, and changes are immediately reflected across all production systems.

This environment is the operative system where registrars work directly with live .dk domain data.

<a id="sandbox"></a>

#### Sandbox

- Please see [EPP service specification Wiki][DKHMEPPWIKI] for up to date information for the production environment.

**Production environment is accessible at**:

- `epp-sandbox.dk-hostmaster.dk`
- port `700`

The EPP sandbox environment is designed for client development and testing. It is accessible exclusively to registrars.

Info and check commands return sandbox data, and in some cases, static test data supplied by Punktum dk. Create requests are accepted if they contain valid syntax and data, but they are only serialized for testing purposes. Domains may appear as if they are being processed and results are returned via poll messages, but no propagation into the DNS zone takes place.

Contacts created in the sandbox only exist within this environment and are not available in production or other systems. The Change Password command is supported, but any change applies only within the sandbox and does not affect production. 

To create an EPP sandbox user, you must have access to the RP sandbox and create a Service API user within the portal. If you do not have access to the RP sandbox, please contact Punktum dk. 

For more information on the sandbox environment please see the [OTE specification][DKHMSANDBOX].

<a id="implementation-requirements"></a>

## Implementation Requirements

This section outlines the overall requirements in regard to implementing an EPP client to work with the Punktum dk EPP service. 

<a id="client-transaction-id-cltrid"></a>

### Client Transaction ID (`clTRID`)

In order to ensure transactional integrity and due to the asynchronous nature of some of the EPP commands, we rely on the client transaction id to be unique. This is unique as per client id. This assists in ensuring that a delayed response can be easily identified by simple means.

The clTRID is recommended to be unique for all transactions.

<a id="ip-whitelisting"></a>

### IP Whitelisting

Access to the EPP service production environment requires IP whitelisting of IP addresses.

Maintenance of IP addresses is done in the **Registrar Portal** (RP) and requires an active registrar account for access.

Please see the [Registrar Portal Service Specification][DKHMRPSPEC] for details.

<a id="service-users"></a>

### Service Users

Access to the EPP service production environment requires IP whitelisting of IP addresses.

Creation and maintenance of service users has to be done in the **Registrar Portal** (RP) and requires an active registrar account for access.

Please see the [Registrar Portal Service Specification][DKHMRPSPEC] for details.

<a id="registrar-account-settings"></a>

### Registrar Account Settings

EPP works only on objects managed by the registrar, not on the registrar account itself. 

- Domain names
- Contacts (users)
- Name servers (hosts)

Some object-related commands indirectly affect the registrar account, mainly for:

- Billable operations – e.g., domain creation or renewal. 
- Change of relations between objects

The EPP does not have any commands that work on the account level, except for the `balance` command, please see the section: "[Balance and Prepaid Account](#balance-and-prepaid-account)" for details.

**Create domain renewal setting**

[create domain](#create-domain) is reacting to the setting of the default renewal model. Settings can be overwrited using extension:

- [`dkhm:autoRenew`](#dkhmautorenew)

**Create and update domain verification setting**

[create contact](#create-contact), [update contact](#update-contact) is reacting to the verification responsible setting. Status of contact verification can be set using extension:

- [`dkhm:contact_verification`](#dkhmcontactverification)

Specification and setting the registrar account settings are reserved to the **Registrar Portal** (RP) and requires an active registrar account for access.

Please see the [Registrar Portal Service Specification][DKHMRPSPEC] for details.

<a id="implementation-extensions"></a>

## Implementation Extensions

The EPP service implemented by Punktum DK includes several extensions, with documentation provided where relevant to specific commands and functionalities. 

This section provides an overview of the extensions as a whole. For detailed implementation information, please refer to the [EPP XSD file repository][DKHMXSD].

Below is a list of the extensions. Each extension is described individually and in detail in the following sections. 

- [`dkhm:authInfo`](#dkhmauthinfo)
- [`dkhm:autoRenew`](#dkhmautorenew)
- [`dkhm:contact_validated`](#dkhmcontact_validated)
- [`dkhm:contact_verification`](#dkhmcontactverification)
- [`dkhm:CVR`](#dkhmcvr)
- [`dkhm:domain_confirmed`](#domain_confirmed)
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
- [`dkhm:sole_proprietorship`](#dkhmsoleproprietorship)
- [`dkhm:trackingNo`](#dkhmtrackingno)
- [`dkhm:userType`](#dkhmusertype)
- [`dkhm:vid`](#dkhmvid)

<a id="dkhmauthinfo"></a>

### `dkhm:authInfo`

This extension is used to display the AuthInfo token for a domain name if a valid token is currently active.

The extension works with the Domain Info command, allowing retrieval of the current AuthInfo token for a domain name, if there is an active token.

If an active AuthInfo token exists, its expiration date is also included in the Domain Info command response.

The AuthInfo token is used for:

- Registration of a domain name offered from the [waiting list](#waiting-list)
- Transfer of a registrant-managed domain name to registrar management
- Transfer of a domain name from one registrar to another

AuthInfo token looks as the following in a domain info response:

```XML
<dkhm:authInfo
 op="transfer" expdate="2026-05-20T10:21:50.0Z" xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>REG-TRANSFER-d12d457d342430ae26359b2d9a61897c
 </dkhm:authInfo>
```

The AuthInfo for a domain name can be generated using the [Setting AuthInfo](#setting-authinfo) command.

<a id="dkhmautorenew"></a>

### `dkhm:autoRenew`

This extension allows users to configure whether a domain name should be automatically renewed or set to expire at the end of its term. 

You can determine the domain’s configuration using the [info domain](#info-domain) command.
If autorenew is set to `true`, it will be reflected so in the info domain response as follows:

```XML
<dkhm:autoRenew
 xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>true
 </dkhm:autoRenew>
```

The autorenewal setting can be set in the [create domain](#create-domain) command, allowing the default registrar configuration to be overridden by explicitly specifying a value during domain creation.

The autorenewal setting for a domain name can also be updated using the [update domain](#update-domain) command.

The choice of renewal policy is based on the registrar account default for the specific registrar account. The default can be overridden per request by using the dkhm:autoRenew extension. 

Dkhm:autorenew has two values:

- **true**, indicating that the specific domain name is to be renewed automatically.
- **false**, indicating that the specific domain name is to expire automatically by the end of the term.

The default for a registrar account is auto-renewal = `true`. The default can be changed in the registrar portal.

<a id="dkhmcontact_validated"></a>

### `dkhm:contact_validated`

This extension is used to indicate whether the contact has been validated or not. 

When using the [info contact](#info-contact) command, this field indicates whether the contact object has been successfully validated according to Punktum dks policies. 

dkhm:contact_validated has two values:

- **0**, indicating that the contact is not validated.
- **1**, indicating that the contact is validated.

An example of a verified contact will include the following in the info contact response:

```XML
<dkhm:contact_validated
 xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1
 </dkhm:contact_validated>
```

<a id="dkhmcontactverification"></a>

### `dkhm:contact_verification`

Contact structure related to verifying contact ID and email.

Contains the following fields:

- [`dkhm:contact_verification / responsible`](#dkhmcontactverificationresponsible)
- [`dkhm:contact_verification / verified_id`](#dkhmcontactverificationverifiedid)
- [`dkhm:contact_verification / verified_email`](#dkhmcontactverificationverifiedemail)
- [`dkhm:contact_verification / confirm_email`](#dkhmcontactverificationconfirmemail)
- [`dkhm:contact_verification / confirm_secondary_email`](#dkhmcontactverificationconfirmsecondaryemail)

Please see:

- the [create contact](#create-contact) command for setting the verification status for a given contact on creation
- the [update contact](#update-contact) command for changing the verification status for a given contact
- the [info contact](#info-contact) command, to see verification status for a given contact

<a id="dkhmcontactverificationresponsible"></a>

### `dkhm:contact_verification` / `dkhm:responsible`

Specifies who handles the contact verification.

- `registrar`, indicates that registrar handles verification of the contact. This value is only available for registrar handled contacts (see [`dkhm:management`](#dkhmmanagement)).

```XML
            <dkhm:contact_verification xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
                <dkhm:responsible>registrar</dkhm:responsible>
                <dkhm:verified_id  status="notRequired" >false</dkhm:verified_id>
                <dkhm:verified_email  status="notRequired" >false</dkhm:verified_email>
            </dkhm:contact_verification>
```

- `registry`, indicates that Punktum dk handles verification of the contact.

```XML
            <dkhm:contact_verification xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
                <dkhm:responsible>registry</dkhm:responsible>
                <dkhm:verified_id  status="notRequired" >false</dkhm:verified_id>
                <dkhm:verified_email  status="notRequired" >false</dkhm:verified_email>
            </dkhm:contact_verification>
```

If a contact is registrar handled this value always reflects the [registrar account verification setting](#registrar-account-settings)

<a id="dkhmcontactverificationverifiedid"></a>

### `dkhm:contact_verification` / `dkhm:verified_id`

This element states if the identity has been verified - the name and address is correct and belongs to the customer.

- `true`, indicates that the identity has been verified
- `false`, indicates that the identity has not yet been verified

On  [info contact](#info-contact) the following attributes may be presented.

- `status` indicates the verification status, using the following values
   - `notRequired`, No verification is required.
   - `pending`, Verification is currently in progress.
   - `expired`, Verification has timed out.
   - `completed`, Verification has been completed.
- `expdate` - Only if verification is in progress: the date and time that the verification is going to expire if not completed.

If contact ID is pending, it will look as the following:

```XML
            <dkhm:contact_verification xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
                <dkhm:responsible>registry</dkhm:responsible>
                  <dkhm:verified_id  status="pending"  responsible="registry"  expdate="2026-06-05T21:59:59.0Z" >false</dkhm:verified_id>
                <dkhm:verified_email  status="completed" >true</dkhm:verified_email>
            </dkhm:contact_verification>
```

This may be omitted in [update contact](#update-contact). If
   - `Responsible = registry`, verified_id will go through our risk engine and determine if verified_id = `false` or `true`
   - `Responsible = registrar`, verified_id will become `true`, as we assume you have verified contacts id.


<a id="dkhmcontactverificationverifiedemail"></a>

### `dkhm:contact_verification` / `dkhm:verified_email`

This element states if the primary email has been verified - the email address is correct and belongs to the customer.

- `true`, indicates that registrar has verified the email address.
- `false`, indicates that the registrar could not verify the email address.

On  [info contact](#info-contact) the following attributes may be presented.

- `status` indicates the verification status, using the following values
   - `notRequired`, No verification is required.
   - `pending`, Verification is currently in progress.
   - `expired`, Verification has timed out.
   - `completed`, Verification has been completed.
- `expdate` - Only if verification is in progress: the date and time that the verification is going to expire if not completed.

If contact email is pending, it will look as the following:

```XML
       <dkhm:contact_verification xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
                <dkhm:responsible>registry</dkhm:responsible>
                 <dkhm:verified_id  status="notRequired" >false</dkhm:verified_id>
                  <dkhm:verified_email  status="pending"  responsible="registry"  expdate="2026-06-05T21:59:59.0Z" >false</dkhm:verified_email>
       </dkhm:contact_verification>
```

This may be omitted in [update contact](#update-contact). If
   - `Responsible = registry`, verified_email will become `false`
   - `Responsible = registrar`, verified_email will become `true`, as we assume you have verified contact email

<a id="dkhmcontactverificationconfirmemail"></a>

### `dkhm:contact_verification` / `dkhm:confirm_email`

This element states if there is an active request on change of primary email. This is only shown if a request is active.

If the request is not completed, the primary email address is not changed.

On [info contact](#info-contact) the following attributes is presented.

   - `pending`, Verification is currently in progress.
and
   - `expdate`, the date and time that the verification is going to expire.
and
   - `responsible`, states who is responsible for authentification.
and
   - `email address`, the email address with an active request.

Example of active request to change primary email on [info contact response](#info-contact-response) will look like under `contact_verification`:

```XML
<dkhm:confirm_email expdate="2025-11-28T22:59:59.0Z" responsible="registry" status="pending">new_email@eksempel.dk</dkhm:confirm_email>
```

<a id="dkhmcontactverificationconfirmsecondaryemail"></a>

### `dkhm:contact_verification` / `dkhm:confirm_secondary_email`

This element states if there is an active request on change or addition of secondary email. This is only shown if a request is active.

If the request is not completed, the secondary email address is not changed or added.

On [info contact](#info-contact) the following attributes is presented.

   - `pending`, Verification is currently in progress.
and
   - `expdate`, the date and time that the verification is going to expire.
and
   - `responsible`, states who is responsible for authentification.
and
   - `email address`, the email address with an active request.

Example of active request to change/add secondary email on [info contact response](#info-contact-response) will look like under `contact_verification`:

```XML
<dkhm:confirm_secondary_email expdate="2025-11-28T22:59:59.0Z" responsible="registry" status="pending">new_secondary_email@eksempel.dk</dkhm:confirm_secondary_email>
```

<a id="dkhmcvr"></a>

### `dkhm:CVR`

This extension is for transporting VAT (CVR) registration numbers. The number is used for validation and VAT accounting. 

The Danish CVR number is also used to register-lock entities such as companies, public institutions, and associations. This process involves fetching the official name and address from the CVR register and binding the entity to this data. The Danish VAT number is 8 digits long.  

This extension is used for the [create contact](#create-contact) command and also displayed in [info contact response](#info-contact-response). See example here:

```XML
<dkhm:CVR
 xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">24210375
 </dkhm:CVR>
```

<a id="dkhmdomain_confirmed"></a>

### `dkhm:domain_confirmed`

This extension is returned in the response to the [create domain](#create-domain) command and will always hold the value `1`.

The extension was used when registrars were not required to obtain the registrant's acceptance of Punktum dk's Terms and Conditions. As this acceptance is now mandatory — see the [`dkhm:orderconfirmationToken`](#dkhmorderconfirmationtoken) extension — a domain name is always confirmed, and the value is therefore always `1`.

```XML
<dkhm:domain_confirmed
 xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1
</dkhm:domain_confirmed>
```

<a id="dkhmdomainadvisory"></a>

### `dkhm:domainAdvisory`

This extension is used to communicate any special circumstances related to a domain name.  

We currently have two advisories that are communicated through the response of a [info domain](#info-domain) command: 

- **PendingDeletionDate**, indicating that a given domain name is scheduled for deletion
- **OfferedOnWaitingList**, indicating that a given domain name has been offered to a designated registrant

Domain names scheduled for deletion are placed in a 30-day redemption period. This period may be initiated by:

- **Automatic expiration**, if the given domain name is not set for auto-renewal
- **Manual cancellation**, if the given domain name has been cancelled manually
- **Missing payment**, if the given domain name has been suspended due to a lack of payment
- **ID-control expired**, if the registrant for the given domain name has not completed the mandatory ID-control
- **Restoration unconfirmed**, if the registrant has not confirmed the user rights for a domain name after the first suspension 

Throughout the redemption period, it is possible to restore the domain name using the [restore domain](#restore-domain) command. It is not possible to restore the domain name with restore domain if the 30-day redemption period is caused by an expired ID-control or restoration unconfirmed.

The domainAdvisory indicating `pendingDeletionDate` can be found in the [info contact response](#info-contact-response) as follows:

```XML
<dkhm:domainAdvisory
 advisory="pendingDeletionDate" date="2025-08-27T22:00:00.0Z" domain="eksempel.dk"
xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"/>
```

The domainAdvisory field includes `offeredOnWaitingList`, which can be found in the info domain response as follows:

```XML
<dkhm:domainAdvisory advisory="offeredOnWaitingList" domain="eksempel.dk"
```

Domain names offered on the waiting list must be registered using the [create domain](#create-domain) command, with the authentication token provided by Punktum dk. 

<a id="dkhmean"></a>

### `dkhm:EAN`

This extension is used to specify an EAN number for a contact.  

It is only relevant for Danish public entities that utilize EAN for electronic invoicing. 

The EAN number must correspond to the CVR/VAT number associated with the contact information. 

If the EAN number does not match the registered CVR/VAT number, the request will be rejected and will result in an error. 

⚠️ Please note, that this extension is irrelevant for registrar managed contacts.

This can be used for [create contact](#create-contact) and [update contact](#update-contact), and looks as the following:

```XML
<dkhm:EAN
 xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">5798009780690
 </dkhm:EAN>
```

<a id="dkhmmanagement"></a>

### `dkhm:management`

This extension is used to define how a contact is managed.

The default registrar management for contacts can be overridden per request or application using this extension, which supports two values:

- **Registrar**, indicating registrar management 
- **Registrant**, indicating registrant management

This extension can only be used for [create contact](#create-contact) command.

You can see an example of the registrar default being overridden with create contact here:

```XML
<dkhm:management
 xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">registrant
 </dkhm:management>
```

⚠️ Do note, that registration of domain names is not supported under `registrant management`. This command is intended solely for creating handles for name server administrators.

To change the management model for an existing registrant managed domain, please see the [transfer domain](#transfer-domain) command.

<a id="dkhmmobilephone"></a>

### `dkhm:mobilephone`

This extension is used to add a mobile phone number in addition to a voice number for contacts. 

This field is optional for contacts to have, and be used for [create contact](#create-contact) and [update contact](#update-contact). 

You can see an example of this extension below:

```XML
      <dkhm:mobilephone
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">+45.51270675
      </dkhm:mobilephone>
```

<a id="dkhmorderconfirmationtoken"></a>

### `dkhm:orderconfirmationToken`

This extension is used to indicate when a registrant has accepted Punktum dks Terms and Conditions through the registrar. 

This extension is **mandatory** for the following commands: 

- [create domain](#create-domain)
- [change registrant](#change-registrant)

The token is a timestamp in EPOCH format, indicating when the agreement was accepted. The EPOCH timestamp has to be specified adhering to the POSIX/UNIX standard and has to be specified in seconds. We do not support milliseconds or nanoseconds. 

The EPOCH / UNIX timestamp must not exceed 24 hours into the future compared to the local time resolution, and it must not be more than 30 days prior to the domain registration. If this condition is not met, the system will return error code 2004 along with a descriptive error message. 

The token is handled the following way:

- If absent, the request will result in an error and the request will be dismissed 
- If present. The token will be validated by Punktum dk 
- If not valid the request will result in an error and the request will be dismissed 
- If valid the request will be accepted and processed 

You can see an example of the orderconfirmationToken below:

```XML
<dkhm:orderconfirmationToken
 xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">1773222271
</dkhm:orderconfirmationToken>
```

<a id="dkhmpnumber"></a>

### `dkhm:pnumber`

This extension holds the P-number (production unit number) assigned by the Danish Business Authority. It identifies the specific physical location of a business unit under a shared CVR number and is relevant for entities operating across multiple addresses. 

This extension is applicable to the [create contact](#create-contact) and [update contact](#update-contact) commands for contacts with an associated CVR number, and is shown in [info contact](#info-contact).

The following example shows the P-number extension:

```XML
<dkhm:pnumber
 xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">1006410698
</dkhm:pnumber>
```

<a id="dkhmregistrant_validated"></a>

### `dkhm:registrant_validated`

This extension indicates whether the registrant is validated for a domain name. 

This extension can response in two values: 

- **0**, indicating that the registrant is not validated 
- **1**, indicating that the registrant is validated 

The procedures for ID-control are described on the [described on the Punktum dk DK website][DKHMIDENT].

`dkhm:registrant_validated` is used in the response of two commands: 

- [create domain response](#create-domain-response)
- [info domain response](#info-domain-response)

Example indicating that the registrant is validated:

```XML
<dkhm:registrant_validated
 xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1
</dkhm:registrant_validated>
```

<a id="dkhmrequestednsadmin"></a>

### `dkhm:requestedNsAdmin`

This extension is used for [update host](#update-host) and [create host](#create-host), where it is possible to request another name server administrator than the authenticated user. 

If the request designates a user other than the registrar account, the specified user must approve the transfer or creation of the host. 

If this extension is not included, the authenticated registrar will automatically become the name server administrator.

An example of the extension is found below:

```XML
<dkhm:requestedNsAdmin
 xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">DKHM1-DK
</dkhm:requestedNsAdmin>
```

<a id="dkhmrisk_assessment"></a>

### `dkhm:risk_assessment`

This extension is used in response for [create domain](#create-domain) in poll message. 

The extension provides information on the risk assessment made by Punktum dk. 

The risk_assessment value must be interpreted in combination with the poll message received.

If the poll message is: 
`%.dk has been registered and activated`

then risk_assessment can be either BLUE or GREEN. 

- If risk_assessment = `BLUE`, the domain name is activated and the registrant (Danish or foreign) has completed the required ID and data validation. 
- If risk_assessment = `GREEN`, the domain name is activated and the registrant is foreign, ID validation is not required, and data validation has been completed. 

If the poll message is:
`%.dk has been registered, but not activated due to pending ID and/or data check`

then risk_assessment can be either BLUE or RED (YELLOW). 

- If risk_assessment = `BLUE`, the registrant is Danish and must complete ID and/or data validation before the domain name can be activated. 
- If risk_assessment = `RED` (or ´YELLOW´), the registrant is foreign and must complete ID and/or data validation before the domain name can be activated. 
- If risk_assessment = `N/A`, the risk assessment could not be performed. The registrant is requested to complete successful ID validation before the domain name can become active. 

The procedures for ID-control are [described on the Punktum dk DK website][DKHMIDENT].

An example from a poll message with risk_assessment is shown below:

```XML
      <dkhm:risk_assessment
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">BLUE
      </dkhm:risk_assessment>
```

<a id="dkhmsecondaryemail"></a>

### `dkhm:secondaryEmail`

This extension is used to add a secondary email in addition to the primary email for contacts. 

This field is optional for contacts to have and can be used for [create contact](#create-contact) and [update contact](#update-contact)

If the contact is registered with a CVR/VAT number, the secondary email will be displayed as the public email in WHOIS.

You can see an example of this extension below:

```XML
      <dkhm:secondaryEmail
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">email@eksempel.dk
      </dkhm:secondaryEmail>
```

<a id="dkhmsoleproprietorship"></a>

### `dkhm:sole_proprietorship`

In relation to NIS2 a company with a sole proprietorship (owned by a single individual) has the same protection as an individual, so email will not be visible in the public whois and other services providing unprivileged access to the contact data.

For Danish companies this status is based on the company type in the CVR register. For companies outside Denmark it is possible to use this extension to mark a contact as a sole proprietorship.

Please see:

- the [info contact](#info-contact) command, for inspecting the value for a given contact.
- the [create contact](#create-contact) command for setting the value for a given contact.
- the [update contact](#update-contact) command for changing the value for a given contact.

The extension support two values:

- **true**, indicating that the contact is marked as a sole proprietorship
- **false**, indicating that the contact is not marked as a sole proprietorship

Example of sole_proprietorship is shown below:

```XML
      <dkhm:sole_proprietorship
        xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>true
      </dkhm:sole_proprietorship>
```

<a id="dkhmtrackingno"></a>

### `dkhm:trackingNo`

A unique tracking ID for a domain registration, used to ensure alignment with RP. EPP is not the only registration channel, so to support registrations across multiple systems, a unique tracking ID is assigned to each request. 

This tracking number is included as an extension and does not replace or interfere with the standard EPP transaction identifiers, clTRID and svTRID. While those are specific to EPP, the tracking number is considered a global identifier within Punktum dk. 

This extension is only mentioned in the [create domain response](#create-domain-response). 

Example of dkhm:trackingNo is shown below:

```XML
<dkhm:trackingNo
 xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>2026050600004
</dkhm:trackingNo>
```

<a id="dkhmusertype"></a>

### `dkhm:userType`

This extension is used to categorize the type of contact. As data requirements vary between user types, this classification is necessary to distinguish between the different types of contacts. 

This extension is used for the [create contact](#create-contact) command and is **mandatory** to include in the request.

Dkhm:userType has the following values:

- **company**
- **individual**
- **public organization**
- **association**

Example of dkhm:userType valued as `company` is shown below:

```XML
<dkhm:userType
 xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">company
</dkhm:userType>
```

<a id="dkhmvid"></a>

### `dkhm:vid`

This extension is used to expose the VID service flag for a domain name.

Please see:

- the [info domain](#info-domain) response, for inspecting the setting for a given domain name.

The extension support two values:

- **true**, indicating the domain name has VID service
- **false**, indicating the domain name does not have VID service

⚠️ Please note that the VID service is currently being phased out and replaced by a similar service. VID is no longer available for purchase by registrants, and only existing VID services remain active.

👉 If a domain name has the VID value sat to **true**, then the domain can not be transferred to registrar management at the moment.

Example of VID service with value `false`:

```XML
<dkhm:vid
 xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
</dkhm:vid>
```

<a id="implementation-limitations"></a>

## Implementation Limitations

The Punktum dk EPP service implementation has certain limitations. A high-level overview is available in the [Compatibility Matrix](compatibility-matrix) in the appendices.

The service is not localized; all EPP-related messages and error responses are delivered in English.

<a id="authentication"></a>

### Authentication

The Punktum dk EPP service only supports username/password authentication. 

Access to the EPP service also requires that your IP-address is whitelisted. You can manage your whitelist through the Registrar Portal (RP), or by contacting Punktum dk support. 

See also the [login](#login) command for details and the section on [Service Users](#service-users).

<a id="authinfo"></a>

### AuthInfo

`AuthInfo` is used to authorize actions performed by third parties, such as domain transfers between registrars

It is currently supported for the following commands:

- [update domain](#update-domain), for changing name servers for a domain name
- [transfer domain](#transfer-domain), for changing the registrar for a domain name
- [create domain](#create-domain), for registering a domain name offered from a waiting list

Please see the individual command descriptions for more details.

When registering a domain name that has been allocated via the waiting list, the `AuthInfo` element is required in the  [create domain](#create-domain) command. In this specific context, the value follows a simplified format defined by the registry.

For all other domain registrations, including the authInfo element in the create domain command has no functional impact and is ignored. 

Similarly, specifying authInfo in a [create contact](#create-contact) command is not recommended, as it has no functional impact and is not processed by the system.

The Punktum dk EPP service does not fully adhere to the RFC standard regarding contact:authInfo element. 

From [RFC:5733]:

> A <contact:authInfo> element that contains authorization
> information to be associated with the contact object.  This
> mapping includes a password-based authentication mechanism, but
> the schema allows new mechanisms to be defined in new schemas.

The contact:authInfo element is mandatory and must always be included, but it may be empty. It should **not** contain sensitive information.

<a id="contact-creation"></a>

### Contact Creation

Contact create does not support the feature of providing a predefined contact-id / user-id. . The contact-id has to be specified as `auto` or `force` and the contact-id is assigned by Punktum dk. See also information on the [create contact](#create-contact) command.

<a id="contact-info"></a>

### Contact Info

The command [info contact](#info-contact) will only supply the registrant information. For other contact objects, only if the requesting user and authenticated has a relationship to the designated contact object, either via a host or domain role or registrar group.

<a id="contact-update"></a>

### Contact Update

The use of the contact:status element for setting or removing status values is not supported in this implementation. All contact status values are system-assigned by Punktum dk based on internal policy. 

For further details, refer to the documentation on the [update contact](#update-contact) command, as well as the appendix on [Contact Status Codes](#contact-status-codes).

<a id="disclosure-of-client-id"></a>

### Disclosure of Client ID

As specified in [RFC:5731], [RFC:5732] and [RFC:5733] the info commands all display a reference to the sponsoring registrar.

This same approach will be implemented in both the Registrar Portal and the end-user self-service portal to ensure consistent data presentation across all platforms.

The public-facing interfaces are also expected to show the registrar relationship. This means that information about the associated registrar will be made available through: 

- WHOIS, see [Punktum dk WHOIS Service Specification][DKHMWHOISSPEC]
- [www.punktum.dk][DKHM], see - [Punktum dk RESTful WHOIS Service Specification][DKHMWHOISRESTSPEC]

The registrar's REG-handle is publicly accessible to their clients. When users log into the self-service portal, they will be able to view the associated registrar, including the registrar’s REG-handle.

<a id="dnssec"></a>

### DNSSEC

I accordance with [RFC:5910]. We support DS only and not DNSKEY. In addition the maximum signature lifetime (`secDNS:maxSigLife`) is disregarded. See [section 3.3][RFC:5910-3.3]) in the referenced RFC.

Not all algorithms are supported, please refer to the [Punktum dk Name Service specification][DKHMDNSSPEC] for a complete list of supported algorithms.

Change of name servers using [update domain](#update-domain), removes all current DSRECORDS as part of the operation.

<a id="domain-info"></a>

### Domain Info

The [info domain](#info-domain) command will return contact information only for the contact object assigned the registrant role.

Contact objects assigned to other roles (e.g. proxy, billing) will be omitted from the response unless the requesting client holds the necessary privileges — either through a direct association with the domain or contact object, or via registrar group.

Availability of DNSSEC information and status is currently limited to public available data.

<a id="domain-transfer"></a>

### Domain Transfer

In [RFC:5731 section 3.2.4][RFC:5731-3.2.4] is it described that an optional period can be specified. Punktum dk does not support the extension of a period via a transfer.

<a id="domain-update"></a>

### Domain Update

This command does not support setting or removing status values using the `domain:status` XML element. Domain status values are system-assigned by Punktum dk and cannot be manually modified by clients.

For more information, refer to the [update domain](#update-domain) command and the appendix on [Domain Status Codes](#domain-status-codes).

<a id="encoding-and-idn-domains"></a>

### Encoding and IDN domains

Punktum dk supports IDN domain names and the EPP commands support Punycode notation for this in **requests**.

Punktum dk does not support Punycode notation in **responses**.

For details on supported characters, please see: [the Punktum dk Name Service specification][DKHMDNSSPEC].

<a id="host-info"></a>

### Host Info

The command [info host](#info-host) will only supply the name server administrator information if the requesting and authenticated user has a relationship to the user, either via a domain role or registrar group, which provides authorization to access the information.

<a id="host-update"></a>

### Host Update

This command does not support the setting and removal of status using the XML element: `host:status`. The status is assigned by Punktum dk. 

See also information on the [update host](#update-host) command and the appendix with [Host Status Codes](#domain-status-codes).

<a id="information-disclosure"></a>

### Information Disclosure

Please note that some information is not disclosed when using Object Query Commands. See the specific commands for more information.

Additionally Punktum dk does not implement the optional `contact:disclose` element.

> An OPTIONAL <contact:disclose> element that allows a client to
> identify elements that require exceptional server-operator
> handling to allow or restrict disclosure to third parties.  See
> Section 2.9 for a description of the child elements contained
> within the <contact:disclose> element.

From [RFC:5733].

<a id="unimplemented-commands"></a>

### Unimplemented Commands

<a id="unimplemented-command-delete-contact"></a>

#### Unimplemented Command: Delete Contact

The `contact:delete` command is not implemented by Punktum dk. Unlinked contacts are automatically deleted by Punktum dk. 

<a id="unimplemented-command-transfer-contact"></a>

#### Unimplemented Command: Transfer Contact

The `contact:transfer` command is not implemented by Punktum dk, Contacts are transferred with their domain name, please see the [transfer domain](#transfer-domain) command.

<a id="unsupported-contact-status-codes"></a>

### Unsupported Contact Status Codes

Several of the contact status codes described in [RFC:5733] are not supported.

Punktum dk does not support client-managed status codes in our EPP implementation. All status values are managed exclusively by Punktum dk and applied in accordance with our internal policies. Registrars are therefore not able to add, remove, or modify status codes on domain or contact objects. The followng contact status codes are not supported:

- `clientDeleteProhibited`
- `clientUpdateProhibited`
- `clientTransferProhibited`

Since the administrative model does not support user enforced restraints.

- `pendingCreate`
- `pendingDelete`
- `pendingTransfer`

The operation to create a contact is instantaneous.

The listed pending-states for [delete contact](#unimplemented-command-delete-contact) and [transfer contact](#unimplemented-command-transfer-contact) are not supported in the Punktum dk registry system.

<a id="unsupported-domain-status-codes"></a>

### Unsupported Domain Status Codes

Several of the domain status codes described in [RFC:5731] and the [ICANN status code description][ICANN] are not supported.

Punktum dk does not support client-managed status codes in our EPP implementation. All status values are managed exclusively by Punktum dk and applied in accordance with our internal policies. Registrars are therefore not able to add, remove, or modify status codes on domain or contact objects. The followng contact status codes are not supported:

- `clientDeleteProhibited`
- `clientHold`
- `clientRenewProhibited`
- `clientTransferProhibited`
- `clientUpdateProhibited`

Since the administrative model does not support user enforced restraints.

- `addPeriod`
- `autoRenewPeriod`
- `renewPeriod`
- `transferPeriod`

These are ICANN-defined status codes that are not supported in the Punktum dk registry system. They are not considered standard in our implementation and are not used within our EPP service.

- `pendingRenew`
- `pendingRestore`
- `pendingTransfer`

The operations for [renew domain](#renew-domain), [restore domain](#restore-domain) and [transfer domain](#transfer-domain) are processed instantaneously. Therefore, the listed pending states do not correspond to the operational processes within the Punktum dk registry system. 

- `inactive`

This state is unsupported, since domain names in the Punktum dk registry **must** have associated name servers, please see the Name Service Specification.  [Name Service Specification][DKHMDNSSPEC].

The [domain status codes listing](#domain-status-codes) holds a complete listing of all the status codes.

<a id="unsupported-host-status-codes"></a>

### Unsupported Host Status Codes

Several of the host status codes described in [RFC:5732] are not supported.

All of the `client*` status codes are note supported:

- `clientDeleteProhibited`
- `clientUpdateProhibited`

The administrative model does not support user enforced restraints.

- `pendingTransfer`

The transfers of hosts are a part of the [domain transfer](#domain-transfer) operation, which is instantaneous and as outlined in [RFC:5732] transfer of host objects does not apply.

From [RFC:5732]:

> Transfer semantics do not directly apply to host objects, so there is
> no mapping defined for the EPP <transfer> command.  Host objects are
> subordinate to an existing superordinate domain object and, as such,
> they are subject to transfer when a domain object is transferred.

<a id="waiting-list"></a>

## Waiting List

Punktum dk provides a waiting list service for domain names. When a domain becomes available and reaches the first position on a waiting list, it must be registered through the standard registration process, either via the Registrar Portal or EPP. 

This is done using the[create domain](#create-domain) command, which must include the authorization token issued by Punktum dk. 

The state that a domain name is offered to a waiting list can be inspected via the [info domain](#info-domain) via the [`dkhm:domainAdvisory`](#dkhmdomainadvisory) extension:

```XML
  <dkhm:domainAdvisory advisory="offeredOnWaitingList" domain="eksempel.dk"
    xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"/>
```

Waiting list positions for domain names are not defined in the RFC standards and are handled according to our internal procedures. 

Please refer to the Punktum dk [website][DKHMWAITLIST] for more information.

<a id="supported-object-transform-and-query-commands"></a>

## Supported Object Transform and Query Commands

The following section outlines the EPP commands supported by our system. 

As previously noted, several commands include extensions that go beyond the standard EPP functionality. These extensions are described individually under each command and are defined in the [DKHMXSD][DKHMXSD] listed in the [Resources](#resources) chapter. A overview of the extensions is also available in the [Implementation Extensions](#implementation-extensions) section.

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
- `poll`, including acknowledgment of messages
- `balance` command extension
- `restore` extension to update domain command

Commands that have not been extended are only briefly described. For full details, please refer to the general EPP documentation available through the IETF RFCs listed in the [References](#references) section.

<a id="hello-and-greeting"></a>

### hello and greeting

This part of the EPP protocol is described in [RFC:5730]. This command adheres to the standard. For a more detailed explanation of the data collection policy announced via the greeting, please see the [Data Collection Policy](#data-collection-policy) section.

As announced in the [greeting](#greeting), the following objects are available:

- `Host`, see also the section on [Host assets](#host)
- `Domain`, see also the section on [Domain assets](#domain)
- `Contact`, see also the section on [Contact assets](#contact)
- `Balance`, see also the section on [Balance and Prepaid account](#balance-and-prepaid-account)

With regard to extensions, the following are available:

- [secDNS-1.1][DKHMXSD]
- [dkhm-4.4][DKHMXSD]
- [dkhm-4.5][DKHMXSD]
- [dkhm-domain-4.4][DKHMXSD]

The following example shows a greeting announcement:

```XML
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <greeting>
        <svID>DK Hostmaster EPP Service (production): 5.3.0</svID>
        <svDate>2026-05-11T08:50:11.0Z</svDate>
        <svcMenu>
            <version>1.0</version>
            <lang>en</lang>
            <objURI>urn:ietf:params:xml:ns:host-1.0</objURI>
            <objURI>urn:ietf:params:xml:ns:domain-1.0</objURI>
            <objURI>urn:ietf:params:xml:ns:contact-1.0</objURI>
            <objURI>http://www.verisign.com/epp/balance-1.0</objURI>
            <svcExtension>
                <extURI>urn:ietf:params:xml:ns:secDNS-1.1</extURI>
                <extURI>urn:dkhm:params:xml:ns:dkhm-4.4</extURI>
                <extURI>urn:dkhm:params:xml:ns:dkhm-4.5</extURI>
                <extURI>urn:dkhm:params:xml:ns:dkhm-domain-4.4</extURI>
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

⚠️ Do note the service version is available in the svID tag, meaning you can see what given version of the EPP service is running in the environment queried.

<a id="login"></a>

### login

This part of the EPP protocol is described in [RFC:5730]. This command adheres to the standard.

The login process uses Punktum dk’s general Authentication, Authorization, and Access (AAA) framework. This means that, in addition to validating the username and password provided in the [login request](#login-request), the system also attempts to authorize the authenticated user for access to the EPP service and any subsequent operations. 

[Service Users](#service-users) are reserved for specific services, such as EPP. Only the administrator of a registrar group can create them. Service Users are the sole account type permitted for logging in to the service.

Punktum dk supports the change of passwords via EPP. Please refer to the chapter [Available Environments](#available-environments) for any special circumstances. 

Password should adhere to the following requirements:

EPP supports a password with at least 6 and a maximum 16 characters, where Punktum dk supports 8 - 64 characters. The password must include at least three of these four character types:

- Lower-case letters
- Upper-case letters
- Numbers
- Special Characters

The following characters are legal special characters in passwords:

```text
% ` ' ( ) * + - , . / : ; < > = ! _ & ~ { } | ^ ? $ # @ " [ ]
```

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <login>
      <clID>EPP-123</clID>
      <pw>Test-1234</pw>
      <newPW>1234-Test</newPW>
      <options>
        <version>1.0</version>
        <lang>en</lang>
      </options>
      <svcs>
        <objURI>domainurn:ietf:params:xml:ns:domain-1.0urn:ietf:params:xml:ns:domain-1.0 domain-1.0.xsd</objURI>
        <objURI>hosturn:ietf:params:xml:ns:host-1.0urn:ietf:params:xml:ns:host-1.0 host-1.0.xsd</objURI>
        <objURI>contacturn:ietf:params:xml:ns:contact-1.0urn:ietf:params:xml:ns:contact-1.0 contact-1.0.xsd</objURI>
      </svcs>
    </login>
    <clTRID>a1c1f2503692b1c03210de405fd28520</clTRID>
  </command>
</epp>
```

Successful authentication established a session with a life span of 700 seconds (5 minutes), it can be kept alive by sending additional hello commands or similar.

The overall life span is 28800 seconds (8 hours) after this the session is terminated and should be reestablished with a new authentication (login). 

A connection can be prematurely terminated if the service gets in a unstable state.

<a id="login-request"></a>

#### login request

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <login>
      <clID>EPP-123</clID>
      <pw>1234-Test</pw>
      <options>
        <version>1.0</version>
        <lang>en</lang>
      </options>
      <svcs>
        <objURI>urn:ietf:params:xml:ns:domain-1.0</objURI>
        <objURI>urn:ietf:params:xml:ns:host-1.0</objURI>
        <objURI>urn:ietf:params:xml:ns:contact-1.0</objURI>
        <svcExtension>
          <extURI>urn:dkhm:params:xml:ns:dkhm-4.5</extURI>
        </svcExtension>
</svcs>
    </login>
    <clTRID>8cdc0a2d5c9b70147177b2ff2d4596ff</clTRID>
  </command>
</epp>
```

<a id="login-response"></a>

#### login response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
    <response>
        <result code="1000">
            <msg>User EPP-123 logged in. </msg>
        </result>
        <trID>
            <clTRID>8cdc0a2d5c9b70147177b2ff2d4596ff</clTRID>
            <svTRID>AC94431E-4D0E-11F1-8180-F90E831702FB</svTRID>
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
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <command>
    <logout />
    <clTRID>9450488c8280671c051f273285d7bec7</clTRID>
  </command>
</epp>
```

<a id="logout-response"></a>

#### logout response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
    <response>
        <result code="1500">
            <msg>User logged out. Closing Connection.</msg>
        </result>
        <trID>
            <clTRID>9450488c8280671c051f273285d7bec7</clTRID>
            <svTRID>0E5ED8F8-4D18-11F1-8A5E-F661C8591FE7</svTRID>
        </trID>
    </response>
</epp>
```

<a id="poll-and-message-queue"></a>

### Poll and Message Queue

This part of the EPP protocol is described in [RFC:5730]. This command adheres to the standard.

There are no special additions or alterations to the specification or use of this command.

For clarification `2303` is returned in case a provided message-id (`msgID`) point to a non-existing message.

EPP poll messages provide an asynchronous way for the server to deliver events to a registrar. 

Use `<poll op="req">` to retrieve the next message from the queue. 

The server returns the oldest pending message (e.g., domain renewals, status changes, contact updates, or system notifications). 

Acknowledge and remove the message with `<poll op="ack" msgID="...">`.

This allows registrars to handle asynchronous events reliably.

👉 A list of all receivable poll messages is found in [Poll-Message-Reference-Guide](Poll-Message-Reference-Guide.md)

Overview of possible retun codes below:

| Return Code | Description                                    |
|-------------|------------------------------------------------|
| 1000        | A messages was successfully dequeued using ack |
| 1301        | Command completed successfully; ack to dequeue |
| 2303        | In case a requested message no longer exist    |

<a id="poll-req-request"></a>

#### poll req request

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <poll op="req"/>
    <clTRID>ABC-123</clTRID>
  </command>
</epp>
```

<a id="poll-req-response"></a>

#### poll req response

Example of a poll message response:

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1301">
      <msg>Command completed successfully; ack to dequeue</msg>
    </result>
    <msgQ count="1" id="123456">
      <qDate>2025-04-29T10:33:07.0Z</qDate>
      <msg>eksempel.dk has been registered and activated</msg>
    </msgQ>
    <resData>
      <domain:panData
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name paResult="1">eksempel.dk</domain:name>
        <domain:paTRID>
          <clTRID>69f3c970119c0ffb2d379b114c05598a</clTRID>
          <svTRID>B84B3D10-24E3-11F0-8F76-AB60B7075109</svTRID>
        </domain:paTRID>
        <domain:paDate>2025-04-29T10:33:07.0Z</domain:paDate>
      </domain:panData>
    </resData>
    <extension>
      <dkhm:risk_assessment
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">BLUE
      </dkhm:risk_assessment>
    </extension>
    <trID>
      <clTRID>52e962f7468027912d0a0efe6c85b668</clTRID>
      <svTRID>33E95336-963A-8493-E065-000000000202</svTRID>
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
    <poll msgID="1234567" op="ack"/>
  </command>
</epp>
```

<a id="poll-ack-response"></a>

#### poll ack response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <msgQ count="1" id="123456"></msgQ>
    <trID>
      <clTRID>89c0aa60e35a08050092c1640c91d467</clTRID>
      <svTRID>5E7E6A1A-6560-11F0-981A-E18FCC4A6577</svTRID>
    </trID>
  </response>
</epp>
```

<a id="poll-ack-response-for-non-existent-message-or-previously-acknowledged-message"></a>

#### poll ack response for non-existent message (or previously acknowledged message)

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="2303">
      <msg>Object does not exist</msg>
    </result>
    <msgQ count="1" id="123456"></msgQ>
    <trID>
      <clTRID>c45a029937598d8a6e8ccbf0ddae205e</clTRID>
      <svTRID>83321D16-6560-11F0-AEB8-B000796D436E</svTRID>
    </trID>
  </response>
</epp>
```

<a id="balance-and-prepaid-account"></a>

### Balance and Prepaid Account

Under the administrative model for registrar management, transactions operate on a prepaid basis.

This means that EPP commands involving billable operations are evaluated in the context of the registrar’s account, with accounting performed as part of the operation.

The operations currently classified as billable are:

- Domain name application/creation, this is described in detail in the [create domain](#create-domain) section
- Domain name renewal. this is described in detail in the [renew domain](#renew-domain) section
- Restoration from deletion (cancellation), this is described in detail in the [restore domain](#restore-domain) section
- Restoration from suspension due to automatic expiration, also described in detail in the [restore domain](#restore-domain) section

Version 4.0.0 of the service introduces a new error scenario for creation/application requests: a request will be declined if the registrar account has insufficient funds. Manual domain renewals via the [renew domain](#renew-domain) command and automatic renewals are not subject to this restriction. Please refer to the registrar contract for specific business and policy rules, as this document is intended for technical reference only.

All prices and amounts relating to currencies are provided in DKK, converted to the EPP currency type, using decimal point (.) and not decimal comma (,), which is the definition for the Danish locale.

The balance command implementation is based on the extension developed by Verisign, please see: [Verisign: "Balance Mapping for the Extensible Provisioning Protocol (EPP)"][BALANCE],  see also [References](#references)

<a id="balance-request"></a>

#### balance request

```xml
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <command>
     <info>
        <balance:info xmlns:balance="http://www.verisign.com/epp/balance-1.0"/>
     </info>
       <clTRID>4bc8e88d325411be7bed216c214832aa</clTRID>
    </command>
</epp>
```

<a id="balance-response"></a>

#### balance response

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
    <response>
        <result code="1000">
            <msg>Command completed successfully</msg>
        </result>
        <resData>
            <balance:infData xmlns:balance="http://www.verisign.com/epp/balance-1.0">
                <balance:creditLimit>0.00</balance:creditLimit>
                <balance:balance>235.34</balance:balance>
                <balance:availableCredit>0.00</balance:availableCredit>
                <balance:creditThreshold>
                    <balance:fixed>0.00</balance:fixed>
                </balance:creditThreshold>
            </balance:infData>
        </resData>
        <trID>
            <clTRID>4bc8e88d325411be7bed216c214832aa</clTRID>
            <svTRID>EF10C7D4-4D19-11F1-AA94-D22BCE4B584A</svTRID>
        </trID>
    </response>
</epp>
```

<a id="domain"></a>

### Domain

The default behavior of the EPP [create domain](#create-domain) command as described in [RFC:5731], will attach the client-ID (`clID`) of the authenticated party to the object created.

If the clID (client ID) is specified with a registrar handle, this indicates that the domain is currently under registrar management.  

If the value is DKHM1-DK, the domain is registrant-managed.

For more information on the [disclose of the Client ID](#disclosure-of-client-id).

Changing the registrar requires a [transfer domain](#transfer-domain) operation.

The designated registrant (contact) has to be under the same management model as the domain name, or the application will be rejected.

The creation of contacts (registrants) is covered under [create contact](#create-contact).

<a id="create-domain"></a>

#### create domain

This command follows the EPP protocol as defined in [RFC:5730]. However, Punktum dk requires the use of a **mandatory** extension, [`dkhm:orderconfirmationToken`](#dkhmorderconfirmationtoken), during domain creation. This extension is specific to Punktum dk and is not part of the standard EPP specifications.

⚠️ Please note that the domain create command only supports creation of `registrar-managed` domains. 

This is due to an ongoing transition towards a fully registrar-managed model. During this transition phase, it is not possible to create `registrant-managed` domains via EPP. 

All domain creation requests are queued for further processing and will initially be assigned a status of pending (1001).

👉 Please note:

- AuthInfo section is not used for transport of end-user passwords, see also section in: [Implementation Limitations](#implementation-limitations) on [AuthInfo](#authinfo). 

Domains offered from a waiting list can be registered using the create domain command. It requires the authorization token issued by Punktum dk to the designated registrant. The token has to be transported via the AuthInfo field. 

The AuthInfo token used for registration of domain names offered from a waiting list are a 8 digit hexadecimal case insensitive string. The token is offered to the designated registrant out of band and is valid for 14 days. 

A well-formed request for domain creation will always result in:

```text
1001, “Command completed successfully; action pending”
```

The response includes the standard svTRID, which serves as a unique tracking number across provisioning channels. 

The create domain command has been extended with a required field [`dkhm:orderconfirmationToken`](#dkhmorderconfirmationtoken) that must contain a timestamp token confirming the registrant's acceptance of Punktum dk’s terms and conditions in agreement with the registrar. 

```xml
<extension>
  <dkhm:orderconfirmationToken
    xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">1753696971
  </dkhm:orderconfirmationToken>
</extension>
```

For more information, please refer to [`dkhm:orderconfirmationToken`](#dkhmorderconfirmationtoken).

The registrant’s validation status is conveyed in the create domain response via the extension [`dkhm:registrant_validated`](#dkhmregistrant_validated), indicating whether the registrant is validated.

- `1`, indicating the registrant **is** validated 
- `0`, indicating the registrant **is not** validated

The state is communicated in this response in order to provide information on the further flow and process of the [create domain](#create-domain) request.

As part of the process the final response to a [create domain](#create-domain) is communicated via the message queue. In this response the Punktum dk A/S risk assessment is included.

Please refer to the [`dkhm:risk_assessment`](#dkhmrisk_assessment) section for further details.

Upon approval of the application, meaning the pending operation is processed, the domain name will still reflect: pendingCreate. The pendingCreate is not removed until the back-end system serving the EPP service indicates that the operation is completed. The domain name can however be active and it is published in the zone, but some operations are prohibited until finalization of the provisioning towards all systems is completed.

The domain status codes are described in the [Domain Status Codes](#domain-status-codes) addendum..

<a id="domain_application_failure"></a>

##### Domain name Application/Creation Failure

As described in the introduction, the existing commands, which are categorized as billable are not changed. Due to the change to the billing procedure however, the application/create operation is extended with an error scenario, for when the prepaid account does not have sufficient funds. 

The proposed error message is the following, quoted from [RFC:5730].

> 2104    "Billing failure"
>
> This response code MUST be returned when a server attempts
> to execute a billable operation and the command cannot be
> completed due to a client-billing failure.

The create domain command returns EPP response codes indicating either pending status or failure. See the table below for an overview of possible return codes. 

| Return Code | Description                                                              |
|-------------|--------------------------------------------------------------------------|
| 1001        | Command completed successfully; action pending                           |
| 2001        | Invalid period [`period`]. Must be within range                          |
| 2201        | Registrant has previously failed ID-control and cannot become registrant |
| 2201        | Registrar handle is not permitted for registrant role.                   |
| 2004        | `dkhm:orderconfirmationToken`: >`token`< exceeds allowed threshold       |
| 2004        | Too few users for contact type                                           |
| 2004        | Too many users for contact type                                          |
| 2005        | Bad value for `dkhm:orderconfirmationToken`: >`token`<                   |
| 2104        | Insufficient credit. Domain cannot be created.                           |
| 2305        | Specified user handle not permitted when handled by registrar            |
| 2305        | Specified user handle not permitted without handling by registrar        |
| 2306        | Specifying contacts for registrar handled domains is not allowed         |
| 2400        | Internal error. No connection to ECDS service.                           |
| 2500        | Internal error. ECDS operation not found in the configuration            |
| 2500        | Internal error. Can't get prices from ECDS.                              |
| 2500        | Internal error.                                                          |

<a id="create-domain-request"></a>

##### create domain request

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <create>
      <domain:create xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>test123.dk</domain:name>
        <domain:period unit="y">1</domain:period>
        <domain:ns>
          <domain:hostObj>ns1.punktum.dk</domain:hostObj>
          <domain:hostObj>ns2.punktum.dk</domain:hostObj>
        </domain:ns>
        <domain:registrant>DKHM1-DK</domain:registrant>
        <domain:authInfo>
          <domain:pw/>
        </domain:authInfo>
      </domain:create>
    </create>
<extension>
      <dkhm:orderconfirmationToken xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">1778071433</dkhm:orderconfirmationToken>
    </extension>
    <clTRID>095f6d91095189c88977166b61d196b9</clTRID>
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
            <msg>Create domain pending for test123.dk</msg>
        </result>
        <extension>
            <dkhm:trackingNo xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>2026051100008</dkhm:trackingNo>
            <dkhm:registrant_validated xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1</dkhm:registrant_validated>
        </extension>
        <trID>
            <clTRID>095f6d91095189c88977166b61d196b9</clTRID>
            <svTRID>7BD304B2-4D1E-11F1-860A-E2D28493172C-2026051100008</svTRID>
        </trID>
    </response>
</epp>
```

This tracking number (`trackingNo`), listed as an extension and does not replace or interfere with the normal use of the EPP transaction keys, `clTRID` and `svTRID`, but are EPP specific, whereas the tracking number is considered global in Punktum dk. The tracking number is also appended to the `svTRID` in addition to the listing in the extension part. Please see the last digits following the last dash.

```XML
<svTRID>7BD304B2-4D1E-11F1-860A-E2D28493172C-2026051100008</svTRID>
```

```XML
<dkhm:trackingNo xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>2026051100008</dkhm:trackingNo>
```

⚠️ A domain name can only be registered with a one-year period.

Please note that the command create domain supports Punycode notation for specifying IDN domain names, but responses are in the specified UTF-8 character set. 

<a id="poll-and-messages"></a>

##### Poll and Messages

As described above, domain creation is asynchronous. After a create domain request results in a pending state, the outcome must be retrieved using the [poll req request](#poll-req-request) command. 

The outcome can be one of several for [create domain](#create-domain), please see the examples below:

<a id="create-domain-poll-message-for-successful-creation"></a>

###### create domain poll message for successful creation with immediate activation

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1301">
      <msg>Command completed successfully; ack to dequeue</msg>
    </result>
    <msgQ count="1" id="123456">
      <qDate>2025-09-02T13:28:01.0Z</qDate>
      <msg>test123.dk has been registered and activated</msg>
    </msgQ>
    <resData>
      <domain:panData
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name paResult="1">eksempel.dk</domain:name>
        <domain:paTRID>
          <clTRID>095f6d91095189c88977166b61d196b9</clTRID>
          <svTRID>3604B5C2-6B9A-11F0-BE0E-FE6843FC4760-2025072800001</svTRID>
        </domain:paTRID>
        <domain:paDate>2025-09-02T13:28:01.0Z</domain:paDate>
      </domain:panData>
    </resData>
    <extension>
      <dkhm:risk_assessment
        xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>BLUE
      </dkhm:risk_assessment>
    </extension>
    <trID>
      <clTRID>3ea0bbaaa74c30dc610ede409fee1662</clTRID>
      <svTRID>3DD1FB68-0F00-6884-E065-000000000202</svTRID>
    </trID>
  </response>
</epp>
```

<a id="create-domain-poll-message-for-successful-creation-pending-id-check"></a>

###### create domain poll message for successful creation with pending mandatory ID check

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1301">
      <msg>Command completed successfully; ack to dequeue</msg>
    </result>
    <msgQ count="1" id="123456">
      <qDate>2025-09-02T13:34:06.0Z</qDate>
      <msg>eksempel.dk has been registered, but not activated due to pending ID and/or data check</msg>
    </msgQ>
    <resData>
      <domain:panData
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name paResult="1">eksempel.dk</domain:name>
        <domain:paTRID>
          <clTRID>095f6d91095189c88977166b61d196b9</clTRID>
          <svTRID>3604B5C2-6B9A-11F0-BE0E-FE6843FC4760-2025072800001</svTRID>
        </domain:paTRID>
        <domain:paDate>2025-09-02T13:34:06.0Z</domain:paDate>
      </domain:panData>
    </resData>
    <extension>
      <dkhm:risk_assessment
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">RED
      </dkhm:risk_assessment>
    </extension>
    <trID>
      <clTRID>915054c16e950ae3c09d8c424ba3b439</clTRID>
      <svTRID>3DD1FB68-0F07-6884-E065-000000000202</svTRID>
    </trID>
  </response>
</epp>
```

<a id="create-domain-poll-message-for-unsuccessful-creation-existing-domain"></a>

###### create domain poll message for unsuccessful creation, existing domain

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1301">
      <msg>Command completed successfully; ack to dequeue</msg>
    </result>
    <msgQ count="1" id="123456">
      <qDate>2025-09-02T14:16:01.0Z</qDate>
      <msg>The application for punktum.dk has been rejected, as the domain was already taken</msg>
    </msgQ>
    <resData>
      <domain:panData
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name paResult="0">punktum.dk</domain:name>
        <domain:paTRID>
          <clTRID>6a60db6abb6a7b3abfbeef9c696f18e3</clTRID>
          <svTRID>3091E82E-8807-11F0-A31B-C51FBC20656E-2025090200029</svTRID>
        </domain:paTRID>
        <domain:paDate>2025-09-02T14:16:01.0Z</domain:paDate>
      </domain:panData>
    </resData>
    <trID>
      <clTRID>9a51851b7ce888fb853dda49f6539323</clTRID>
      <svTRID>3DD320AF-55AB-ABF7-E065-000000000202</svTRID>
    </trID>
  </response>
</epp>
```

<a id="create-domain-poll-message-for-unsuccessful-creation-cancelled"></a>

###### create domain poll message for unsuccessful creation, application cancelled without a specified reason

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1301">
      <msg>Command completed successfully; ack to dequeue</msg>
    </result>
    <msgQ count="1" id="123456">
      <qDate>2025-09-02T14:19:54.0Z</qDate>
      <msg>The application for eksempel.dk has been cancelled</msg>
    </msgQ>
    <resData>
      <domain:panData
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name paResult="0">eksempel.dk</domain:name>
        <domain:paTRID>
          <clTRID>a234c68c228a5bb0ce0456406bda07b1</clTRID>
          <svTRID>DDD311FC-8807-11F0-8097-AEDFBB5E2208-2025090200030</svTRID>
        </domain:paTRID>
        <domain:paDate>2025-09-02T14:19:54.0Z</domain:paDate>
      </domain:panData>
    </resData>
    <trID>
      <clTRID>730605a9ed209d4089fc2fb8bb4e513c</clTRID>
      <svTRID>3D9348A0-62CD-8EA8-E065-000000000202</svTRID>
    </trID>
  </response>
</epp>
```

<a id="check-domain"></a>

#### check domain

In general this part of the EPP protocol is described in [RFC:5731] and this command adheres to the standard.

Punktum dk supports multiple domain names per <check> command in EPP.

Requests may contain more than one domain name and will be processed accordingly.

See the table below for an overview of possible return codes for check domain responses: 

| Return Code | Description                                   |
|-------------|-----------------------------------------------|
| 1000        | If the check domain command is successful     |
| 2303        | If the specified domain object does not exist |

<a id="check-domain-request"></a>

##### check domain request

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <check>
      <domain:check
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>punktum.dk</domain:name>
      </domain:check>
    </check>
    <clTRID>f542e948b25e5797d908ab549943bf37</clTRID>
  </command>
</epp>
```

<a id="check-domain-response"></a>

##### check domain response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <domain:chkData
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:cd>
          <domain:name avail="0">punktum.dk</domain:name>
          <domain:reason>In Use</domain:reason>
        </domain:cd>
      </domain:chkData>
    </resData>
    <trID>
      <clTRID>f542e948b25e5797d908ab549943bf37</clTRID>
      <svTRID>C30DE4A0-6BAB-11F0-BD88-B1847992122E</svTRID>
    </trID>
  </response>
</epp>
```

The available values for the `reason` field are:

- `In use`, for domain names registered with the Punktum dk registry
- `Enqueued`, the domain name is awaiting application processing; this can last a few seconds to a few days.
- `Offered for pos. on waiting list`, for when the domain name has been offered to a designated registrant from a waiting list position

An example for a waiting list position offering would look as follows:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <domain:chkData
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:cd>
          <domain:name avail="0">waiting-list.dk</domain:name>
          <domain:reason>Offered for pos. on waiting list</domain:reason>
        </domain:cd>
      </domain:chkData>
    </resData>
    <trID>
      <clTRID>4dd63066e8dc43b5bb5071004386e9d6</clTRID>
      <svTRID>46C2B78A-6BAC-11F0-A0E8-E8DFF89BC595</svTRID>
    </trID>
  </response>
</epp>
```

<a id="info-domain"></a>

#### info domain

This part of the EPP protocol is described in [RFC:5731]. This command adheres to the standard. In addition the command has been extended with Punktum dk extensions:

- `dkhm:domainAdvisory`
- `dkhm:registrant_validated`
- `dkhm:authInfo`
- `dkhm:autoRenew`
- `dkhm:vid`

Do note that the response only contains the registrant contact object, if the authenticated user has a relationship via the domain name, which provides access to more information.

The below example could shows the public available information. It could be extended with the following data:

```xml
<domain:contact type="admin">DKHM1-DK</domain:contact>
```

and additionally:

```xml
<domain:contact type="billing">DKHM1-DK</domain:contact>
```

Please note that the billing contact and proxy are displayed only if the authenticated user is authorized to access this information. The registrant role associated with the domain name is publicly available. Authorization is granted based on either a direct role on the domain name or an affiliation through a registrar group. 

The domain:clID field indicates which management model the domain is currently under. 

- For registrant managed domain names: 
`<domain:clID>DKHM1-DK<domain:clID>`, indicating Punktum dk A/S 

- For registrar managed domain names: 
`<domain:clID>REG-123456</domain:clID>`, as seen by users associated with the registrar account 


For DNSSEC data the availability is limited to only displaying if the information is public available. 

See the table below for an overview of possible return codes for info domain:

| Return Code | Description                                   |
|-------------|-----------------------------------------------|
| 1000        | If the info domain command is successful      |
| 2303        | If the specified domain object does not exist |

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
  </command>
</epp>
```

<a id="info-domain-response"></a>

##### info domain response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <domain:infData
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>dk-hostmaster.dk</domain:name>
        <domain:roid>DK_HOSTMASTER_DK-DK</domain:roid>
        <domain:status s="ok"/>
        <domain:registrant>DKHM1-DK</domain:registrant>
        <domain:ns>
          <domain:hostObj>auth01.ns.dk-hostmaster.dk</domain:hostObj>
          <domain:hostObj>auth02.ns.dk-hostmaster.dk</domain:hostObj>
          <domain:hostObj>auth03.ns.dk-hostmaster.dk</domain:hostObj>
        </domain:ns>
        <domain:clID>DKHM1-DK</domain:clID>
        <domain:crDate>1998-01-18T23:00:00.0Z</domain:crDate>
        <domain:upDate>2022-03-15T02:03:39.0Z</domain:upDate>
        <domain:exDate>2027-03-31T21:59:59.0Z</domain:exDate>
      </domain:infData>
    </resData>
    <extension>
      <dkhm:registrant_validated
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">1
      </dkhm:registrant_validated>
      <secDNS:infData
        xmlns:secDNS="urn:ietf:params:xml:ns:secDNS-1.1">
        <secDNS:dsData>
          <secDNS:keyTag>57040</secDNS:keyTag>
          <secDNS:alg>8</secDNS:alg>
          <secDNS:digestType>2</secDNS:digestType>
          <secDNS:digest>02676dc904d9a55b4a60ce2545f863220da34356e4cea979e9c81408e126f2ca</secDNS:digest>
        </secDNS:dsData>
      </secDNS:infData>
      <dkhm:autoRenew
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">true
      </dkhm:autoRenew>
      <dkhm:vid
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">false
      </dkhm:vid>
    </extension>
    <trID>
      <clTRID>dd24599cb23fd6a0693cc85ea37c9b8e</clTRID>
      <svTRID>E391526A-6BAC-11F0-A8F9-84F63C765911</svTRID>
    </trID>
  </response>
</epp>
```

<a id="info-domain-response-with-domain-advisory"></a>

##### info domain response with domain advisory

The [`dkhm:domainAdvisory`](#dkhmdomainadvisory) extension can be included in an info domain response to display advisory information. 

The advisory can currently communicate two advisories:

- `pendingDeletionDate`, indicating that a given domain name is scheduled for deletion
- `offeredOnWaitingList`, indicating that a given domain name has been offered to a designated registrant

If a domain name is marked for pending deletion, this special status is communicated via the [`dkhm:domainAdvisory`](#dkhmdomainadvisory) extension.

```xml
<extension>
  <dkhm:domainAdvisory advisory="pendingDeletionDate" date="2025-07-28T22:00:00.0Z" domain="eksempel.dk"
    xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"/>
</extension>
```

The field `advisory` indicates a pending delete date with the string: `pendingDeletionDate` followed by a field containing the actual date.

Do note the date is only guiding, since the actual operation of deletion is handled by an external process and operational circumstances might vary and are executed under the discretion of the registry.

If a domain name is offered to a position on a waiting list, the advisory `offeredOnWaitingList` is used.

```xml
<extension>
  <dkhm:domainAdvisory advisory="offeredOnWaitingList" domain="eksempel.dk"
    xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"/>
</extension>
```

Do note that the waiting list status is also used in the [check domain](#check-domain) command, using the `reason` field.

As a waiting list entry does not constitute a full domain name registration, the data returned in the response is limited compared to that of a registered domain. 

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <domain:infData
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk</domain:name>
        <domain:roid>EKSEMPEL_DK-DK</domain:roid>
        <domain:clID>DKHM1-DK</domain:clID>
      </domain:infData>
    </resData>
    <extension>
      <dkhm:domainAdvisory advisory="offeredOnWaitingList" domain="eksempel.dk"
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"/>
    </extension>
    <trID>
     <clTRID>5a8afa3ead777c161cefffa861431ad7</clTRID>
      <svTRID>39040BB8-858C-11F0-9D4C-AD5DAB9DAA48</svTRID>
    </trID>
  </response>
</epp>
```

<a id="info-domain-response-with-authinfo-token"></a>

#### info domain response with AuthInfo token

The AuthInfo token can be set using the [setting AuthInfo](#setting-authinfo) command. Once set, it can be retrieved via the info domain command, provided that the authenticated user is authorized to view it. The token is only included in the response for users with the appropriate privileges. 

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <domain:infData
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk</domain:name>
        <domain:roid>EKSEMPEL_DK-DK</domain:roid>
        <domain:status s="ok"/>
        <domain:registrant>DKHM1-DK</domain:registrant>
        <domain:ns>
          <domain:hostObj>auth01.ns.dk-hostmaster.dk</domain:hostObj>
          <domain:hostObj>auth02.ns.dk-hostmaster.dk</domain:hostObj>
          <domain:hostObj>auth03.ns.dk-hostmaster.dk</domain:hostObj>
        </domain:ns>
        <domain:clID>REG-123456</domain:clID>
        <domain:crDate>1999-05-16T22:00:00.0Z</domain:crDate>
        <domain:upDate>2022-06-13T14:20:36.0Z</domain:upDate>
        <domain:exDate>2027-06-30T21:59:59.0Z</domain:exDate>
      </domain:infData>
    </resData>
    <extension>
      <dkhm:registrant_validated
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">1
      </dkhm:registrant_validated>
      <dkhm:authInfo expdate="2025-08-10T13:05:19.0Z" op="transfer"
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">REG-TRANSFER-dd521b38b8419f1ea833fd3b0d75f334
      </dkhm:authInfo>
      <dkhm:autoRenew
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">true
      </dkhm:autoRenew>
      <dkhm:vid
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">false
      </dkhm:vid>
    </extension>
    <trID>
      <clTRID>86c828baafe091dea8fd8dbed4daa42d</clTRID>
      <svTRID>4D632036-6BAF-11F0-8A08-C5BF4D9C1206</svTRID>
    </trID>
  </response>
</epp>
```

<a id="renew-domain"></a>

#### renew domain

This part of the EPP protocol is described in [RFC:5731]. This command adheres to the standard.

Domain name subscriptions can be renewed manually via the EPP service. The feature applies to both: 

- `Registrant managed domain names`, where the registrar is appointed as the billing contact
- `Registrar managed domain names`

This command is permitted only if the EPP user holds the billing contact privilege, and the domain is either assigned with the registrar as billing contact or is registrar managed. 

Manual renewal can be done up to the expiration date of the specific domain name. It does not influence automatic renewal or automatic expiration apart from delaying their effective execution and automatic change to the domain name lifespan.  

Do note that the billing contact changes are restricted once a domain has been invoiced. Since manual renewal requires billing contact status, the renewal window is limited to the period before invoice generation. 

See current prices at the Punktum dk website: [Products and Prices][DKHMPRICES]

👉 Insufficient funds in the registrar account will not prohibit the renew operation. 

Do note that for period specification, only the unit y indicating year is accepted.

☝️ Only a renewal period of `1` year can be specified in the `domain:period` section. 

⚠️ A domain can only be renewed when there are 2 months or less remaining until its expiration date. 

See diagram of EPP process for EPP renew domain: [:eye_speech_bubble:][epp-renew-domain]

| Return Code | Description                                                                                                                                                                                                                                                                                                                          |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1000        | If the renew domain command is successful                                                                                                                                                                                                                                                                                            |
| 2005        | Syntax of the command is not correct                                                                                                                                                                                                                                                                                                 |
| 2105        | If the domain object is not eligible for renewal. The domain name has to be in the state ‘Active’ and the expiration date has to be within window there renew is still allowed. This will also be reflected in status value `serverRenewProhibited`. See also [ICANN description][ICANNserverRenewProhibited] of status  |
| 2201        | If the authenticated user does not hold the privilege to renew the specified domain object. This privilege is given to the billing contact for the domain name (see also the [login command](#login))                                                                                                                                |
| 2303        | If the specified domain object does not exist                                                                                                                                                                                                                                                                                        |
| 2306        | If the specified expiry date is not valid. The provided expiration date has to be equal to the current expiration date or we return `2306`                                                                                                                                                                                           |
| 2306        | If the calculated expiry date is not allowed. The new expiration date has to be lower than the current expiration date + 10 years. The maximum period to which the expiration date can be extended is 10 years and 3 months. The current expiration date is available via the [info domain](#info-domain) command as `domain:exDate` |
| 2400        | In case of an exception                                                                                                                                                                                                                                                                                                              |

This complete process is atomic and might throw an unrecoverable exception: 2400 either due to unforeseen circumstances or a change in the state of the domain name. 

Upon success, the system returns code 1000. No further communication will occur via the EPP service. The associated billable transaction is deducted from the [Prepaid Account](#balance-and-prepaid-account).

The invoked sub-process is illustrated in the following diagram, which outlines Punktum dk’s EPP domain renewal workflow.  [:eye_speech_bubble:][dkh-renew-domain]

The status code `serverRenewProhibited` is set:

- If the status `pendingCreate` is set, see [create domain](#create-domain)
- If the status `pendingDelete` is set
- If the domain name period renewal will exceed the maximum period of 14 months 
- If the domain name is not settled/paid
- If the domain name is suspended due to automatic expiration
- If the domain name is on hold or blocked, meaning it has been suspended by Punktum dk

<a id="renew-domain-request"></a>

##### renew domain request

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <renew>
      <domain:renew xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk.dk</domain:name>
        <domain:curExpDate>2026-07-30</domain:curExpDate>
        <domain:period unit="y">1</domain:period>
      </domain:renew>
    </renew>
  <clTRID>c288de5f0f07cd184eee2fdbcae3ce37</clTRID>
  </command>
</epp>
```

<a id="renew-domain-response"></a>

##### renew domain response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <trID>
      <clTRID>de5a8c34097da642265c9b611c719e79</clTRID>
      <svTRID>CBC1E812-6BB0-11F0-9AF3-DA2D104402D5</svTRID>
    </trID>
  </response>
</epp>
```

<a id="update-domain"></a>

#### update domain

This part of the EPP protocol is based on [RFC:5731], but the implementation does not fully comply with the standard. 

⚠️ The authInfo element is intended for handling AuthInfo tokens and must not be used for transporting end-user passwords. 

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

It supports DNSSEC management as specified in [RFC:5910].

The command will be evaluated as an atomic command, even though it is dispatched to several sub-commands.

Diagram of EPP process for EPP update domain [:eye_speech_bubble:][epp-update-domain]

For the command to be processed, the following data must be provided: 

- a valid domain name
- a sub-command, consisting of either
	- add (`add`)
	- change (`chg`)
	- remove (`rem`)

If the command is valid, the command is separated into one of more of the following sub-commands (by order of precedence): 

1. `change registrant`
1. `remove name server`
1. `remove admin contact`
1. `remove billing contact`
1. `add name server`
1. `add admin contact`
1. `add billing contact`

1. `remove DSRECORDS`
1. `add DSRECORDS`

1. `setting authinfo`
1. `unsetting authinfo`

The commands are then executed sequentially (above order dictates the precedence) as a single transaction. If a single sub-command fails, the transaction is rolled-back and the relevant error code is returned.

👉 The command might be stopped if the sub-commands cannot be executed. For example if one of the sub-commands is a: `change registrant`, none of the other commands can be executed, since role changes will be implicit. 

Do note that the change of billing contact, if inserting a registrar-user, will be silent, meaning no e-mails will be sent to the registrant or existing billing contact or other contacts. 

When the command succeeds either `1000` or `1001` is returned. If one of the operations initiated by the sub-command require additional actions to be taken, `1001` will have precedence over `1000`. If a `1001` is returned the status code pendingUpdate might be set if an additional update domain command is issued. 

Diagram of EPP process for EPP update domain command evaluation [:eye_speech_bubble:][epp-update-domain-evaluate]

Below is a table of possible return codes for [update domain](#update-domain).

| Return Code | Description                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------|
| 1000        | If the update domain command is successful                                                               |
| 1001        | If the update domain command awaits acknowledgment by 3rd. party                                        |
| 2005        | Syntax of the command is not correct                                                                     |
| 2102        | Change of status for host object is not supported                                                        |
| 2201        | If the authenticated user does not hold the privilege to update the specified domain object              |
| 2303        | If the specified domain name does not exist                                                              |
| 2303        | If the specified host name does not exist, for when adding a new name server                             |
| 2303        | If the specified host name does not exist, for when removing a name server                               |
| 2303        | If the specified contact-id  does not exist, for when adding a new billing contact                       |
| 2304        | If the specified host name does not link with the specified domain name, for when removing a name server |
| 2307        | Unimplemented object service, the service does not support change of registrant on a domain              |
| 2308        | The number of name servers are below the required limit                                                  |

The command might be blocked and the status code: serverUpdateProhibited is returned in domain info indicating that an update is not possible. See also [ICANN description][ICANN] of status codes.

<a id="update-domain-request"></a>

##### update domain request

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <domain:update
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk</domain:name>
        <domain:add/>
        <domain:rem/>
        <domain:chg/>
      </domain:update>
    </update>
    <clTRID>a218ecdd3a08202b38d891a4079b32f6</clTRID>
  </command>
</epp>
```

<a id="update-domain-response"></a>

##### update domain response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <trID>
      <clTRID>a218ecdd3a08202b38d891a4079b32f6</clTRID>
      <svTRID>51C5819E-6BB1-11F0-8F14-DB306A69F6DD</svTRID>
    </trID>
  </response>
</epp>
```

<a id="change-registrant"></a>

##### change registrant

The change of registrant operation results in all privileges and rights being transferred to another entity. A registrar only holds the privileges to complete such an operation for domains in their own portfolio. 

This mean the following prerequisites have to be met: 

- The domain name has to be in the portfolio of the registrar requesting the change

- The registrant on the domain name has to be in the portfolio of the registrar requesting the change

- The domain name has to be in a state where it can have the registrant changed

- The contact designated to be appointed as the new registrant are in the portfolio of the registrar requesting the change

- The contact designated to be appointed as the new registrant has to be in a state where it can be assigned the role of registrant

- The registrar will have to collect the accept of terms and conditions for Punktum dk from the contact designated to be appointed as the new registrant

The use of the [`dkhm:orderconfirmationToken`](#dkhmorderconfirmationtoken) extension is always required in order to relay the accept of Terms and Condition for Punktum dk from the new registrant to Punktum dk. Without the extension the command will fail. This usage is identical to domain name application using [create domain](#create-domain).

Here follows an example of a request to change the registrant.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <domain:update
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk</domain:name>
        <domain:add/>
        <domain:rem/>
        <domain:chg>
          <domain:registrant>DKHM1-DK</domain:registrant>
        </domain:chg>
      </domain:update>
    </update>
    <extension>
      <dkhm:orderconfirmationToken
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">1753707224
      </dkhm:orderconfirmationToken>
    </extension>
    <clTRID>3865a5fa134cd896455a909ebb2958d7</clTRID>
  </command>
</epp>
```

If the registrant already has completed ID-control, the change wil now be completed with this response:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <trID>
      <clTRID>3865a5fa134cd896455a909ebb2958d7</clTRID>
      <svTRID>441D3264-6C81-11F0-B179-97A77FBE290A</svTRID>
    </trID>
  </response>
</epp>
```

If Punktum dk require that ID and/or data check must be successfully completed before the change can be completed the response will indicate that:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <response>
        <result code="1001">
	        <msg>Command completed. Pending registrant accept</msg>
        </result>
        <trID>
            <clTRID>3865a5fa134cd896455a909ebb2958c7</clTRID>
            <svTRID>4CDF5D36-AD97-11EF-A65B-622FDC063AF2</svTRID>
        </trID>
    </response>
</epp>
```

Responses returning result code `1001` will be followed by poll messages containing the final operation status, including completion or failure information.

The possible return codes for change registrant are listed below:

| Return Code | Description                                                                                 |
|-------------|---------------------------------------------------------------------------------------------|
| 1000        | If the update domain command is successful                                                  |
| 1001        | If the update domain command is successful, but an action is pending                        |
| 2005        | Syntax of the command is not correct                                                        |
| 2201        | If the authenticated user does not hold the privilege to update the specified domain object |
| 2303        | If the specified domain name does not exist                                                 |

<a id="add-name-server"></a>

##### add name server

⚠️ The addition of a new name server to a domain name or a re-delegation requires that the new name server must offer resolution for the domain name in question.

With this process change, the change of name servers operation using [update domain](#update-domain), also delete all DSRECORDS.

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<update>
			<domain:update
				xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name>punktum.dk</domain:name>
				<domain:add>
					<domain:ns>
						<domain:hostObj>ns01.punktum.dk</domain:hostObj>
						<domain:hostObj>ns02.punktum.dk</domain:hostObj>
					</domain:ns>
				</domain:add>
			</domain:update>
		</update>
	</command>
</epp>
```

Diagram: Update domain - Add name server [:eye_speech_bubble:][epp-update-domain-add-ns]

Possible return codes for add name server is shown below:

| Return Code | Description                                                                                 |
|-------------|---------------------------------------------------------------------------------------------|
| 1000        | If the update domain command is successful                                                  |
| 2005        | Syntax of the command is not correct                                                        |
| 2201        | If the authenticated user does not hold the privilege to update the specified domain object |
| 2303        | If the specified domain name does not exist                                                 |
| 2303        | If the specified host name does not exist, for when adding a new name server                |

<a id="remove-name-server"></a>

##### remove name server

The removal of an existing name server from a domain name requires that at least two other name servers are offering resolution for the domain in question, else the command will fail. 

👉 Since the update domain command can contain several sub-commands, this could be accompanied by an `add name server`, so the policy requirement is met and resolution is kept. 

With this process change, the change of name servers operation using [update domain](#update-domain), also delete all DSRECORDS.

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0">
	<command>
		<update>
			<domain:update
				xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name>punktum.dk</domain:name>
				<domain:rem>
					<domain:ns>
						<domain:hostObj>ns1.punktum.dk</domain:hostObj>
						<domain:hostObj>ns2.punktum.dk</domain:hostObj>
					</domain:ns>
				</domain:rem>
			</domain:update>
		</update>
	</command>
</epp>
```

Diagram Update domain - Remove name server [:eye_speech_bubble:][epp-update-domain-remove-ns]

| Return Code | Description                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------|
| 1000        | If the update domain command is successful                                                               |
| 2005        | Syntax of the command is not correct                                                                     |
| 2201        | If the authenticated user does not hold the privilege to update the specified domain object              |
| 2303        | If the specified domain name does not exist                                                              |
| 2303        | If the specified host name does not exist, for when removing a name server                               |
| 2304        | If the specified host name does not link with the specified domain name, for when removing a name server |
| 2308        | The number of name servers are below the required limit                                                  |

<a id="remove-and-add-name-server"></a>

##### remove and add name server

⚠️ A domain must always have at least two active name servers configured, and all specified name servers must resolve correctly. It is therefore recommended to perform name server changes using a single combined update operation, rather than executing separate add and remove commands.

An example of both remove and add nameserver in one operation can look like this:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <domain:update xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>punktum.dk</domain:name>
        <domain:add>
          <domain:ns>
            <domain:hostObj>ns01.punktum.dk</domain:hostObj>
            <domain:hostObj>ns02.punktum.dk</domain:hostObj>
          </domain:ns>
        </domain:add>
   <domain:rem>
          <domain:ns>
            <domain:hostObj>ns1.dk-hostmaster.dk</domain:hostObj>
            <domain:hostObj>ns2.dk-hostmaster.dk</domain:hostObj>
          </domain:ns>
        </domain:rem>
        <domain:chg/>
      </domain:update>
    </update>
  </command>
</epp>
```

<a id="add-contact"></a>

##### add contact

The addition of contacts to a domain name is subject to the following policies. These rules apply only to registrant-managed domain names, as roles on registrar-managed domain names are implicitly handled by the registrar.

1. If the authenticated user is the admin contact, only the billing contact role can be added. 
2. If the authenticated user is a registrar only billing can be added

If the new contact is not a registrar account, the contact must accept the role before it is assigned. Therefore, the operation is asynchronous.

Adding contacts requires appropriate privileges, which reside with the registrant, subject to the policies described above. 

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <domain:update
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk</domain:name>
        <domain:add>
          <domain:contact type="billing">EKSEMPEL-DK</domain:contact>
        </domain:add>
        <domain:rem/>
        <domain:chg/>
      </domain:update>
    </update>
    <clTRID>75aa0c5c0397cdf4bf6739bc20b00f8c</clTRID>
  </command>
</epp>
```

Diagram: Update domain - Add billing/admin contact [:eye_speech_bubble:][epp-update-domain-add-contact]

Diagram: Update domain - Add billing/admin contact sub-process [:eye_speech_bubble:][dkh-update-domain-add-contact]

Possible return codes for add contact:

| Return Code | Description                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------|
| 1000        | If the update domain command is successful                                                               |
| 2005        | Syntax of the command is not correct                                                                     |
| 2201        | If the authenticated user does not hold the privilege to update the specified domain object              |
| 2303        | If the specified domain name does not exist                                                              |

<a id="remove-contact"></a>

##### remove contact

The removal of a existing contact is possible for both billing and admin contacts.

1. If the authenticated user is the admin, both billing and admin roles can be removed
2. If no new contact is specified, the role defaults to the registrant. No request is sent; the registrant is only informed of the change out-of-band. 

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <domain:update
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk</domain:name>
        <domain:add/>
        <domain:rem>
          <domain:contact type="admin">EKSEMPEL-DK</domain:contact>
        </domain:rem>
        <domain:chg/>
      </domain:update>
    </update>
    <clTRID>aef0533f49d8e87f0728308677737b8e</clTRID>
  </command>
</epp>
```

Diagram: Update domain - Remove billing/admin contact [:eye_speech_bubble:][epp-update-domain-remove-contact]

Diagram: Update domain - Remove billing/admin contact sub-process [:eye_speech_bubble:][dkh-update-domain-remove-contact]

Possible return codes for remove contact: 

| Return Code | Description                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------|
| 1000        | If the update domain command is successful                                                               |
| 2005        | Syntax of the command is not correct                                                                     |
| 2201        | If the authenticated user does not hold the privilege to update the specified domain object              |
| 2303        | If the specified domain name does not exist                                                              |

<a id="remove-dsrecords"></a>

##### Remove DSRECORDS

This command is used to remove one or more DSRECORDS for a domain name.

To use this command, the registrar must have name server manager, admin or registrar management privileges, as only registrars with these roles are authorized to modify DNSSEC information for a domain. 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <domain:update
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk</domain:name>
        <domain:add/>
        <domain:rem/>
        <domain:chg/>
      </domain:update>
    </update>
    <extension>
      <secDNS:update
        xmlns:secDNS="urn:ietf:params:xml:ns:secDNS-1.1">
        <secDNS:rem>
          <secDNS:all>true</secDNS:all>
        </secDNS:rem>
      </secDNS:update>
    </extension>
    <clTRID>b1e1e889dad29c9cffece3485b7779e7</clTRID>
  </command>
</epp>
```

Possible return codes for remove DSRECORDS: 

| Return Code | Description                                                                                 |
|-------------|---------------------------------------------------------------------------------------------|
| 1000        | If the update domain command is successful                                                  |
| 2005        | Syntax of the command is not correct                                                        |
| 2201        | If the authenticated user does not hold the privilege to update the specified domain object |
| 2303        | If the specified domain name does not exist                                                 |
| 2303        | If DSRECORDS do not exist, when removing DSRECORDS                                          |

<a id="add-dsrecords"></a>

##### Add DSRECORDS

This command is used to add one or more DSRECORDS for a domain name. 

To use this command, the registrar must have name server manager, admin or registrar management privileges, as only registrars with these roles are authorized to modify DNSSEC information for a domain. 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <domain:update
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk</domain:name>
        <domain:add/>
        <domain:rem/>
        <domain:chg/>
      </domain:update>
    </update>
    <extension>
      <secDNS:update
        xmlns:secDNS="urn:ietf:params:xml:ns:secDNS-1.1">
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
    <clTRID>67909497e93493916776f7814e7e2431</clTRID>
  </command>
</epp>
```

Possible return codes for add DSRECORDS:

| Return Code | Description                                                                                 |
|-------------|---------------------------------------------------------------------------------------------|
| 1000        | If the update domain command is successful                                                  |
| 2005        | Syntax of the command is not correct                                                        |
| 2201        | If the authenticated user does not hold the privilege to update the specified domain object |
| 2303        | If the specified domain name does not exist                                                 |
| 2303        | If DSRECORDS do not exist, when removing DSRECORDS                                          |

<a id="setting-authinfo"></a>

##### Setting AuthInfo

Setting the AuthInfo is done using the `update domain` command. The AuthInfo token is not set as such, but is generated using a keyword indicating the authorization scope. Keyword for generating AuthInfo is

- `autotransfer`

Autotransfer is used to change the acting registrar of a domain name. 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <domain:update
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk</domain:name>
        <domain:add/>
        <domain:rem/>
        <domain:chg>
          <domain:authInfo>
            <domain:pw>autotransfer</domain:pw>
          </domain:authInfo>
        </domain:chg>
      </domain:update>
    </update>
    <clTRID>69ab449541c9873d2521bd4216134df3</clTRID>
  </command>
</epp>
```

The generation of an AuthInfo token can be accomplished by all users with privileges to do so.

When the authorization has been created it is visible to all users with the privilege to generate it, not just the requester.

The token is accessible in:

- The service portal in the domain detail page
- The registrar portal in the domain detail page
- Via EPP using the [domain info](#domain-info) command

<a id="unsetting-authinfo"></a>

##### Unsetting AuthInfo

Anyone with the appropriate privileges may have an interest in ending the life of an AuthInfo token prematurely.

This can be done using the below command:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <domain:update
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk</domain:name>
        <domain:add/>
        <domain:rem/>
        <domain:chg>
          <domain:authInfo>
            <domain:null>
          </domain:authInfo>
        </domain:chg>
      </domain:update>
    </update>
  <clTRID>4686ecd212c05118bd805ba99abacc78</clTRID>
  </command>
</epp>
```

Do note that this can be accomplished by all users with privileges to accomplish this. It adheres to [RFC:5731], which states:

> A domain:null element can be used within the domain:authInfo element to remove authorization information.

The command simply unsets (removes/clears) an AuthInfo token if it exists.

<a id="authinfo-token-format"></a>

##### AuthInfo Token Format

AuthInfo tokens are keys to delegate authorization for a single one-time operation. AuthInfo tokens are limited to work on a single object and perform a single operation, they expire after a specified time, when used or if they are retracted.

The overall requirements are:

- Unpredictable (is secure to the extent possible and for the given TTL time frame)
- Human pronounceable (can be communicated over telephone call)
- Usable (constrained on length and format)

These requirements represent our internal guidelines for AuthInfo codes, designed to ensure security, usability, and ease of communication. 

The AuthInfo tokens currently support the operation:

- Change of registrar via the [transfer domain](#transfer-domain) command, by using the keyword `TRANSFER`

The **AuthInfo** token is generated upon request by Punktum dk and will adhere to the following proposed format:

`<role>-<operation>-<unique token>`

Role can contain: 

- `REG` (Registrar has created the code) 
- `OWN` (Registrant has created the code)

Operation can contain:

- `TRANSFER` (Transfer of domain to another registrar)

Unique token: 

Example: `REG-TRANSFER-098f6bcd4621d373cade4e832627b4f6`

For registration of domain names offered from a waiting list, the authorization is using AuthInfo, the token here is however simpler and is currently formatted as a 8 character string of case insensitive hexadecimal characters.

<a href="delete-domain"></a>

#### delete domain

The default `delete domain` command behaviour is to deactivate immediately, which complies with [RFC:5731]. Not being able to complete the request will result in a error, also in compliance with [RFC:5731]. Please see below for more information on the business process for deletion.

The current expiration date can be obtained using the `info domain` command and is specified in the `domain:exDate` field. The date conforms with the required format. The [status code](#status-codes), `pendingDelete` delete is set and can be removed either by the execution of the process after the redemption period or a [restore](#restore-domain) operation.

The alternative approach to deletion is to set auto expire, which will cancel the domain name subscription automatically at expiration.

Do note that it is not possible to delete a domain name after the expiration date of a domain name. Domain name deletion is not prohibited on the expiration date, but due to technical constraints it is recommended to set automatic expiration instead, which will have the same result.

The deletion of a domain name results in an expected 30-day suspension, which is regarded as a redemption period where it is possible to restore the suspended domain name using the [restore domain](#restore-domain) command.

The status code `serverDeleteProhibited` is set:

- If the status `pendingCreate` is set, see [create domain](#create-domain)
- If the status `pendingDelete` is set
- If the domain name is on hold or blocked, meaning it has been suspended by Punktum dk
- If the domain name is superordinate to a name server, which has active name service
- If the registrant completed ID-control unsuccessfully

<a id="delete-domain-request"></a>

##### delete domain request

The complete command will look as follows (example lifted from [RFC:5731]):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <delete>
      <domain:delete
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk</domain:name>
      </domain:delete>
    </delete>
    <clTRID>f8cb214a177b3d9cf272bf9855999beb</clTRID>
  </command>
</epp>
```

<a id="delete-domain-response"></a>

##### delete domain response

Domain names are not deleted immediately but are instead flagged for deletion. If the delete command is successful, the domain name will be scheduled for deletion within the timeframe defined by Punktum dk’s business rules. 

The response for a `delete domain` command will be `1001`.

Response example (example lifted from [RFC:5731] and modified):

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1001">
      <msg>Command completed successfully; action pending</msg>
    </result>
    <trID>
      <clTRID>f8cb214a177b3d9cf272bf9855999beb</clTRID>
      <svTRID>644E6BAC-6C8B-11F0-A1BD-9D4129D85F0B</svTRID>
    </trID>
  </response>
</epp>
```

The expiration date will be adjusted accordingly and a status pendingDelete with an advisory date will be applied and made available via [info domain response](#info-domain-response), via the Punktum dk extension [`dkhm:domainAdvisory`](#dkhmdomainadvisory).

Example:

```xml
<extension>
  <dkhm:domainAdvisory advisory="pendingDeletionDate" date="2025-08-27T22:00:00.0Z" domain="eksempel.dk" xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"/>
</extension>
```

Do note that if subordinates exist these may block for a delete and the request will result in an error: `2305`.

<a id="restore-domain"></a>

#### restore domain

As described in [RFC:3915], with a support for grace periods, it is possible to restore a domain name scheduled for deletion, (in the state `pendingDelete`).

Punktum dk will support the ability to restore for two use-cases:

1. Get a domain name back to the state active from a pending deletion specified by an explicit deletion request (delete command) or a automatic expiration
1. Get a domain name back to state active from a pending deletion, caused by missing financial settlement (only for registrant managed domain names)

Domain names might be suspended for other reasons, these will no be recoverable using the described restore facility, this will be indicated using the `serverUpdateProhibited` status.

Restoration has to take place during the redemption period and will not be possible after the domain has been deleted.

The restoration is requested using the update domain command.

##### restore domain request

The request operation is not implemented for the restoration process. You must skip directly to the report operation to complete the restoration. 

his means that the `pendingRestore` state is not supported in the restore process.

The step to actually complete the restoration is the `report` operation, which look as follows:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <domain:update
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk</domain:name>
        <domain:add/>
        <domain:rem/>
        <domain:chg/>
      </domain:update>
    </update>
    <extension>
      <rgp:update
        xmlns:rgp="urn:ietf:params:xml:ns:rgp-1.0">
        <rgp:restore op="report">
          <rgp:report>
            <rgp:preData/>
            <rgp:postData/>
            <rgp:delTime>1900-01-01T00:00:00.0Z</rgp:delTime>
            <rgp:resTime>1900-01-01T00:00:00.0Z</rgp:resTime>
            <rgp:resReason/>
            <rgp:statement/>
          </rgp:report>
        </rgp:restore>
      </rgp:update>
    </extension>
    <clTRID>bcfd3fa74ed98b3f64774d120dc5a7d5</clTRID>
  </command>
</epp>
```

Example is lifted from [RFC:3915] and modified.

The proposal is to use the report part as an acknowledgment. The domain name is restored as-is if possible, so the mandatory fields are included but may remain empty: 

- `rgp:preData`
- `rgp:postData`
- `rgp:resReason`
- `rgp:statement`

The following fields are mandatory and must include both deletion time and restoration time according to [RFC:3915].: 

- `rgp:delTime`
- `rgp:resTime`

<a id="restore-domain-response"></a>

##### restore domain response

A response indicating an unsuccessful restoration attempt, when the domain is not eligible for restoration by the current user, will appear as follows:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="2201">
      <msg>No privilege. You are not authorized to restore domain</msg>
    </result>
    <trID>
      <clTRID>bcfd3fa74ed98b3f64774d120dc5a7d5</clTRID>
      <svTRID>0DB4E2E8-88B1-11F0-AEA1-E15658147090</svTRID>
    </trID>
  </response>
</epp>
```

Example lifted from [RFC:5730] and modified.

A response indicating successful restoration attempt will look as follows:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <trID>
      <clTRID>bcfd3fa74ed98b3f64774d120dc5a7d5</clTRID>
      <svTRID>E7F5D19E-828D-11F0-B455-ABF709D3D6BD</svTRID>
    </trID>
  </response>
</epp>
```

Example is lifted from [RFC:3915] and modified.

<a id="xsd-definition"></a>

##### XSD Definition

The `restore` command is an extension to `update domain`. All is described in [RFC:3915]. The XSD has been included in our EPP XSD repository as `rgs-1.0.xsd` all lifted from [RFC:3915], please see [the repository][DKHMXSD] for details.

<a id="transfer-domain"></a>

#### transfer domain

The transfer command is only available to registrars. The command should be used in the following use cases.

- Transfer from Punktum dk to a registrar 
- Transfer from the current registrar to a future registrar 

The implementation is based on a pull model. Domain transfer operations typically require authorization via an AuthInfo token and the designated domain name. The operation must be initiated by the future registrar, who has obtained the AuthInfo token from the current registrar or the registrant.

Alternatively, for registrant-managed domains, a transfer can be performed without an AuthInfo token if the initiating registrar has a role associated with the domain, such as name server manager, admin contact, or billing contact.

The transfer from Punktum dk to a new registrar implies a change of administrative model from `registrant management` to `registrar management`. Whereas the transfer from registrar to registrar is only a change of administrative party, not the administrative model.

The administration of authorizations is described in detail under the [update-domain](#update-domain) command.

- [Setting AuthInfo](#setting-authinfo)
- [Unsetting AuthInfo](#unsetting-authinfo)

Do also see the general description of the [AuthInfo](#authinfo) implementation.

The transfer process differs from the one outlined in the [RFC:5731], the following status codes should only be the ones observed:

- `clientApproved` - the use of an AuthInfo Token equals client pre-approval
- `serverApproved` - in the transition period (see below section) the registry can approve the transfer without the use of an AuthInfo token

The following should not be observed (ref: `domain:trStatus`), since the process does not implement the same transactional model:

- `clientCancelled`
- `clientRejected`
- `pending`
- `serverCancelled`

Upon transfer, the contact object referring to the registrant role, is being cloned to avoid issues with disappearing data and sponsorship in cross-portfolio operations. Do note that Punktum dk does not implement direct transfer of contact objects as described in the [Implementation Limitations](#implementation-limitations) section.

A contact object (registrant) is cloned without additional relations bound to other objects within the registry or another portfolio, only the key object, the domain name is transferred, together with potential subordinate objects such as name servers. 

The cloning is a best-effort cloning, since the ID-control status cannot be guaranteed to be consistent in the case where a contact object is locked to a register, but has limitations in access to data due to policies in regard to disclosure etc. 

The status code `serverTransferProhibited` is set:

- If the status `pendingCreate` is set, see [create domain](#create-domain)
- If the domain name is not settled/paid
- If the domain name is registrant managed and has VID service
- If the domain name is on hold or blocked, meaning it has been suspended by Punktum dk

<a id="transition-period"></a>

#### Transition Period

 The transition period is a special period following the introduction of the transfer command with version 4.0.0 of the EPP service.

During the transition period, registrars can do a transfer **without** providing the else mandatory `AuthInfo` token with the [transfer domain](#transfer-domain) command. 

The following prerequisites has to be in place for this operation to be possible:

- The designated domain name has to be linked to a name server belonging to the requesting registrar or the requesting registrar should already be the billing or admin contact for the domain name 
- The registrar will have to have collected consent from the registrant of the designated domain name

The moment a transfer from registrant management is completed, it is no longer viable for this transfer, even if the name servers are administered by others than the registrar.

Example of a transfer domain request without AuthInfo token provided:

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <transfer op="request">
      <domain:transfer xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk</domain:name>
      </domain:transfer>
    </transfer>
    <clTRID>ABC-123457</clTRID>
  </command>
</epp>
```

<a id="transfer-domain-request"></a>

##### transfer domain request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <transfer op="request">
      <domain:transfer
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk</domain:name>
        <domain:authInfo>
          <domain:pw>OWN-TRANSFER-a84ecadf4efcc82e38c1799697c6525b</domain:pw>
        </domain:authInfo>
      </domain:transfer>
    </transfer>
    <clTRID>c6c12b2190420ecb8dc98153fb496f57</clTRID>
  </command>
</epp>
```

<a id="transfer-domain-response"></a>

##### transfer domain response

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi=http://www.w3.org/2001/XMLSchema-instance xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <domain:trnData
        xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>eksempel.dk</domain:name>
        <domain:trStatus>clientApproved</domain:trStatus>
        <domain:reID>REG-12345</domain:reID>
        <domain:reDate>2025-09-01T07:38:22.0Z</domain:reDate>
        <domain:acID>OWNER1-DK</domain:acID>
        <domain:acDate>2025-09-01T07:38:27.0Z</domain:acDate>
      </domain:trnData>
    </resData>
    <trID>
      <clTRID>c6c12b2190420ecb8dc98153fb496f57</clTRID>
      <svTRID>A7093CAC-8706-11F0-9C20-FBF0BC8FD8EA</svTRID>
    </trID>
  </response>
</epp>
```

Possible return codes for [transfer domain](#transfer-domain):

| Return Code | Description                                                                                                                        |
|-------------|------------------------------------------------------------------------------------------------------------------------------------|
| 1000        | If the transfer domain command is successful                                                                                       |
| 2005        | Syntax of the command is not correct                                                                                               |
| 2201        | If the authenticated user does not hold the privilege to transfer the specified domain object or the AuthInfo token does not exist |
| 2303        | If the specified domain name does not exist                                                                                        |
| 2304        | If the requesting user does not have the privilege and is not authorized to transfer the domain                                    |
| 2400        | The operation failed                                                                                                               |


<a id="contact"></a>

### Contact

The default behavior of the EPP create contact command, as defined in [RFC:5733], is to set the client ID (CLID) of the authenticated party as the sponsoring client for the contact object, similar to how it is handled for domain creation as described above. 

The contact object will be under the sponsoring party throughout its life cycle, and transfer of contact objects will not be explicitly supported, see [Unimplemented commands](#unimplemented-commands) section.

Deletion will not be supported and will work as it currently is implemented in the Punktum dk EPP service and described in the specification. See the section: [Unimplemented commands](#unimplemented-commands) for details. Contact objects are automatically deleted, under the following policy: 

- The contact object is not in use
- Contact does not hold roles/association with other objects
- The associated financial account has a balance of `0`
- It has been deemed inactive for 14 days

The maintenance, meaning changes and updates to data, will also be limited. Punktum dk locks contact objects for changes if these have been matched to a register for name and address information, this applies to:

- Danish citizens of the type individual, bound to the CPR register
- Danish companies, public organizations and associations of the types, bound to the CVR register

The following contact types are also limited, due to the VAT number validation:

- EU companies, public organizations and associations of the types, bound to the EU Vies register

All other types has to be maintained by the sponsoring client, with the exception of the name attribute.

A contact object consist of the following data.

| Data            | Can be locked      | Note                                                                                                                            |
|-----------------|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| User type       | :white_check_mark: | The is communicated via the extension: `dkhm:userType`                                                                          |
| Name            | :white_check_mark: | The is included in the standard, see: [RFC:5733] (`contact:name`) / (`contact:org`), see: [Address Handling](#address-handling) |
| Address         | :white_check_mark: | The is included in the standard, see: [RFC:5733] (`contact:addr`) / (`contact:street`)                                          |
| Country         | :white_check_mark: | The is included in the standard, see: [RFC:5733] (`contact:addr`) / (`contact:cc`)                                              |
| Attention       |                    | The is included in the standard, see: [RFC:5733] (`contact:name`) / (`contact:org`), see: [Address Handling](#address-handling) |
| Phone           |                    | The is included in the standard, see: [RFC:5733] (`contact:voice`)                                                              |
| Mobile phone    |                    | The is communicated via the extension: `dkhm:mobilephone`                                                                       |
| Email           |                    | The is included in the standard, see: [RFC:5733] (`contact:email`)                                                              |
| Secondary email |                    | The is communicated via the extension: `dkhm:secondaryEmail`                                                                    |
| VAT number      | :white_check_mark: | The is communicated via the extension: `dkhm:CVR`                                                                               |
| P-number        |                    | The is communicated via the extension: `dkhm:pnumber`                                                                           |
| EAN             | :white_check_mark: | The is communicated via the extension: `dkhm:EAN`                                                                               |

The locking of data is towards an authoritative register, like CPR (Central Person Register, Danish individuals) or CVR (Dansk Virksomheds Register, Danish companies, associations and public organizations). 

Locking is applied in conjunction with

- Successful ID-control
- For Danish companies, associations and public organizations if a CVR number is matched towards the CVR register
- For European companies, associations and public organizations if a VAT number is matched towards the VIES service
- For contact object appointed as registrants and waiting list position owners, the name is locked in order to prevent change of ownership of these

<a id="create-contact"></a>

#### create contact

This part of the EPP protocol is described in [RFC:5733].

This command has been extended with the following mandatory field:

- [`dkhm:userType`](#dkhmusertype), which has to be one of:
	- `company`, indicating a company
	- `public_organization`, indicating a public organization
	- `association`, indicating an association
	- `individual`, indicating an individual

The user type will result in context-specific interpretation of the following fields:

- `EAN` - this number is only supported for public organizations and companies in Denmark, user types: company, public_organization and association. The field is optional, but is required for electronic invoicing (OIOUBL). 
- `CVR` - (VAT number) this is only supported for user types: company, public_organization and association. The number is required for handling VAT correctly, The rules for indication of the field is specified in the table below. 
- `P-number` - (production unit number) this is only supported for user types: company, public_organization and association. The number is used for handling validation correctly and it relates to the CVR (Vat number field) the field is optional. 

These fields is validated on the server side. 

The `contact-id` field is auto-generated and assigned by Punktum dk. EPP do however open for providing a contact-id in the context of the create contact command, this is not supported by Punktum dk at this point, see also: [Implementation Limitations](#implementation-limitations).

<a id="cvr--vat-number-indication"></a>

##### CVR / Vat Number Indication

|                                                                                       | Mandatory | Note                                         |
|---------------------------------------------------------------------------------------|-----------|----------------------------------------------|
| `company`/`public_organization`/`association` with address in Denmark and EU/EØS      | Yes       | Has to be specified                          |
| `company`/`public_organization`/`association` with address EU/EØS                     | No        | Can be specified if VAT handling is required |
| `company`/`public_organization`/`association` with address outside Denmark and EU/EØS | No        | Can be specified                             |
| `individual` with address in Denmark and EU/EØS                                       | No        | Not supported                                |
| `individual` with address outside Denmark and EU/EØS                                  | No        | Not supported                                |

<a id="forced-and-smart-contact-creation"></a>

##### Forced and Smart Contact Creation

Punktum dk supports two modes for contact creation using the <contact:id> element: **smart creation** and **forced creation**.  

**Smart creation (auto)**

Smart creation attempts to reuse an existing contact if an exact match is found based on the provided data. If a matching contact exists, the existing user ID (handle) is returned. If no match is found, a new contact is created. 

This is done by setting:

```xml
<contact:id>auto</contact:id>
```

When using smart creation, the system will respond with the existing or newly created user ID. This allows the client to reuse the same contact when creating domains or other objects linked to the contact.

The matching is performed on the following fields, and requires an exact match: 

- <dkhm:userType>  
- <dkhm:CVR>  
- <contact:name>  
- <contact:street>  
- <contact:email>  
- <contact:pc>  
- <contact:cc>  
- <contact:voice>

If all relevant fields match exactly an existing contact, the corresponding user ID is returned in the [create contact response](#create-contact-response), instead of creating a new one.

**Forced creation (force)**

Forced creation always results in a new contact being created, regardless of whether a matching contact already exists. 

This is done by setting:

```xml
<contact:id>force</contact:id> 
```

When using forced creation, Punktum dk will generate a new unique user ID (handle) for the contact.  

⚠️ **Important notes**

- The keywords auto and force must be provided in lowercase.
- Smart creation depends on exact matching of all relevant fields; even minor differences will result in a new contact being created.
- Contacts created via either method may be subject to ID control. In such cases, contact data can be updated during verification, which may affect future matching behavior. 

Diagram for contact creation [:eye_speech_bubble:][epp_create_contact]

<a id="address-handling"></a>

##### Address Handling

When creating a contact via EPP, it is possible to provide postal information in both a local and an international format. Due to how contact data is handled in Punktum dk’s systems, only one format is stored. 

For contacts with Denmark as the country, the local format is used and the international format is ignored. For contacts with any other country, the international format is used and the local format is ignored. Please see the table below. 

| Denmark                      | Other country                    |
|------------------------------|----------------------------------|
| **Local representation**     | Local representation             |
| International representation | **International representation** |

The diagram illustrates the general logic used to determine which address data is applied.

Diagram of address resolution for contact creation [:eye_speech_bubble:][epp-address-resolution]

It is important to note that if the international representation is specified, but data are provided in local representation or only local representation is provided for an international address, communication to the specified address might prove unreliable.

The handling of name and organization is also a special case. Where the following mapping is made based on the user type.

`<contact:name>` is **mandatory** for usertype = `individual` and optional for usertype = `company`, `public_organization`, and `association`.

`<contact:org>` is **mandatory** for usertype = `company`, `public_organization`, and `association`, and **must not** be provided for usertype = `individual`.

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

The data is collected as required by Danish legislation.

👉 Please note:

- `authInfo` section is ignored, but cannot be omitted, since it is specified as mandatory by the EPP protocol in [RFC:5733].
- Contact creation is silent, and the contact is not notified unless the contact is being associated with other objects.

<a id="create-contact-request"></a>

##### create contact request

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <create>
      <contact:create xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
        <contact:id>auto</contact:id>
        <contact:postalInfo type="loc">
          <contact:name>Punktum dk A/S</contact:name>
          <contact:addr>
            <contact:street>Ørestads Boulevard 108, 11.</contact:street>
            <contact:city>København S</contact:city>
			<contact:sp/>
            <contact:pc>2300</contact:pc>
			<contact:cc>DK</contact:cc>
          </contact:addr>
        </contact:postalInfo>
        <contact:voice>+45.33646000</contact:voice>
        <contact:email>punktum@punktum.dk</contact:email>
        <contact:authInfo>
          <contact:pw/>
        </contact:authInfo>
      </contact:create>
    </create>
    <extension>
     <dkhm:userType xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">company</dkhm:userType>
<dkhm:CVR xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">24210375</dkhm:CVR>
      <dkhm:management xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">registrant</dkhm:management>      
      <dkhm:mobilephone
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">+45.33646001</dkhm:mobilephone>
<dkhm:contact_verification xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">
        <dkhm:verified_id>false</dkhm:verified_id>
        <dkhm:verified_email>false</dkhm:verified_email>
      </dkhm:contact_verification>
    </extension>
   <clTRID>ABC123456789</clTRID>
  </command>
</epp>
```

<a id="create-contact-response"></a>

##### create contact response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
    <response>
        <result code="1000">
            <msg>Contact created.</msg>
        </result>
        <resData>
            <contact:creData xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
                <contact:id>DHA1263-DK</contact:id>
                <contact:crDate>2026-05-13T08:01:35.0Z</contact:crDate>
            </contact:creData>
        </resData>
        <trID>
            <clTRID>ABC123456789</clTRID>
            <svTRID>F69FEE1A-4EA1-11F1-9222-F003002C7243</svTRID>
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
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <check>
      <contact:check
        xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
        <contact:id>DKHM1-DK</contact:id>
      </contact:check>
    </check>
    <clTRID>29306d449b5f51deece335ae2ad9da55</clTRID>
  </command>
</epp>
```

<a id="check-contact-response"></a>

##### check contact response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <contact:chkData
        xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
        <contact:cd>
          <contact:id avail="0">DKHM1-DK</contact:id>
          <contact:reason>In use</contact:reason>
        </contact:cd>
      </contact:chkData>
    </resData>
    <trID>
      <clTRID>29306d449b5f51deece335ae2ad9da55</clTRID>
      <svTRID>D5E460EE-857E-11F0-A5D8-F9ADE1E926C2</svTRID>
    </trID>
  </response>
</epp>
```

<a id="info-contact"></a>

#### info contact

This part of the EPP protocol is described in [RFC:5733]. This command has been extended with information indicating whether the queried contact has been validated in accordance with Punktum dk’s requirements and policies.

- See the extension: [`dkhm:contact_validated`](#dkhmcontact_validated) extension used in the response.
- See the extension [`dkhm:contact_verification`](#dkhmcontactverification) extension used in the response for the validation status. 

Please note that the email address `contact:email` is masked, and the value `anonymous@punktum.dk` is always returned for this field unless the authenticated user has an authorized relationship through the domain name or a registrar group association, granting access to additional information.

A secondary email address [`dkhm:secondaryEmail`](#dkhmsecondaryemail) is shown only when usertype is set to company, organization, or public organization if the authenticated user does not have a relationship with the queried contact.

👉 The command has also been extended with information (if available) from the following extensions:

- [`dkhm:userType`](#dkhmusertype)
- [`dkhm:EAN`](#dkhmean)
- [`dkhm:CVR`](#dkhmcvr)
- [`dkhm:pnumber`](#dkhmpnumber)
- [`dkhm:mobilephone`](#dkhmmobilephone)
- [`dkhm:secondaryEmail`](#dkhmsecondaryemail)
- [`dkhm:sole_proprietorship`](#dkhmsoleproprietorship)
- [`dkhm:contact_verification`](#dkhmcontactverification)

The info contact command response is only available for the registrant contact object, unless the authenticated user has a relationship via the domain name or a registrar group association, which provides access to more information or additional contact objects.

<a id="info-contact-request"></a>

##### info contact request

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <info>
      <contact:info xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
        <contact:id>DKHM1-DK</contact:id>
      </contact:info>
    </info>
    <clTRID>f66aa8d9ad158d527047da69175c4846</clTRID>
  </command>
</epp>
```

<a id="info-contact-response"></a>

##### info contact response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <contact:infData
        xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
        <contact:id>DKHM1-DK</contact:id>
        <contact:roid>DKHM1-DK</contact:roid>
        <contact:status s="linked"/>
        <contact:status s="serverDeleteProhibited"/>
        <contact:status s="serverTransferProhibited"/>
        <contact:postalInfo type="loc">
          <contact:name>Punktum dk A/S</contact:name>
          <contact:addr>
            <contact:street>Ørestads Boulevard 108, 11.</contact:street>
            <contact:city>København S</contact:city>
            <contact:pc>2300</contact:pc>
            <contact:cc>DK</contact:cc>
          </contact:addr>
        </contact:postalInfo>
        <contact:email>anonymous@punktum.dk</contact:email>
        <contact:clID>DKHM1-DK</contact:clID>
        <contact:crID>DKHM1-DK</contact:crID>
        <contact:crDate>1899-12-31T23:00:00.0Z</contact:crDate>
        <contact:upID>DKHM1-DK</contact:upID>
        <contact:upDate>2024-12-10T07:25:30.0Z</contact:upDate>
      </contact:infData>
    </resData>
    <extension>
      <dkhm:contact_validated
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">1
      </dkhm:contact_validated>
      <dkhm:CVR
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">24210375
      </dkhm:CVR>
      <dkhm:userType
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">company
      </dkhm:userType>
      <dkhm:sole_proprietorship
        xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
      </dkhm:sole_proprietorship>
      <dkhm:contact_verification
        xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
        <dkhm:responsible>registry</dkhm:responsible>
        <dkhm:verified_id  status="completed" >true</dkhm:verified_id>
        <dkhm:verified_email  status="completed" >true</dkhm:verified_email>
        <dkhm:confirm_email expdate="2025-11-28T22:59:59.0Z" responsible="registry" status="pending">new_email@for_this_contact.nu</dkhm:confirm_email>
        <dkhm:confirm_secondary_email expdate="2025-11-28T22:59:59.0Z" responsible="registry" status="pending">new_secondary_email@for_this_contact.nu</dkhm:confirm_secondary_email>
      </dkhm:contact_verification>
    </extension>
    <trID>
      <clTRID>6e7ccb5d042befa3b8299ac74a9edfba</clTRID>
      <svTRID>1DA8B15A-857F-11F0-BFF6-E76F33F1AFCB</svTRID>
    </trID>
  </response>
</epp>
```

<a id="update-contact"></a>

#### update contact

This part of the EPP protocol is described in [RFC:5733]. This command adheres to the standard. In addition to the standard the command allows for manipulation of the extensions associated with contact objects, meaning that it is possible to update the following fields:

- [`dkhm:EAN`](#dkhmean)
- [`dkhm:pnumber`](#dkhmpnumber)
- [`dkhm:mobilephone`](#dkhmmobilephone)
- [`dkhm:secondaryEmail`](#dkhmsecondaryemail)
- [`dkhm:sole_proprietorship`](#dkhmsoleproprietorship)
- [`dkhm:contact_verification`](#dkhmcontactverification), if `responsible` is set to `registrar`

These are of course all controlled by relevant privileges.

- Name / organization
- Address
- Country
- Phone (voice)
- Email
- Secondary email
- Mobile phone

Diagram of EPP update contact [:eye_speech_bubble:][epp-update-contact]

<a id="update-contact-request"></a>

##### update contact request

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <contact:update
        xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
        <contact:id>PDA1234-DK</contact:id>
        <contact:chg>
          <contact:postalInfo type="int">
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
          <contact:authInfo>
            <contact:pw></contact:pw>
          </contact:authInfo>
        </contact:chg>
      </contact:update>
    </update>
    <extension>
      <dkhm:mobilephone
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">+1.7034444445
      </dkhm:mobilephone>
      <dkhm:secondaryEmail
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">email@eksempel.dk
      </dkhm:secondaryEmail>
      <dkhm:sole_proprietorship
        xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>true
      </dkhm:sole_proprietorship>
      <dkhm:contact_verification
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">
        <dkhm:verified_id>true</dkhm:verified_id>
      </dkhm:contact_verification>
    </extension>
    <clTRID>d9381652a4ee6001a5600c1f247be81d</clTRID>
  </command>
</epp>
```

⚠️ Do note that the `authInfo` part is ignored, but cannot be omitted, since it is specified as mandatory by the EPP protocol in [RFC:5733].

<a id="update-contact-response"></a>

##### update contact response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <trID>
      <clTRID>d9381652a4ee6001a5600c1f247be81d</clTRID>
      <svTRID>452ED3A6-8581-11F0-A3A0-F27BCBB5E036</svTRID>
    </trID>
  </response>
</epp>
```

<a id="delete-contact"></a>

#### delete contact

**This command is not supported.**

This command will always return: `2101`, indicating unimplemented command.

The deletion of contact objects is handled automatically by Punktum dk. The following status flags will be set:

- `serverDeleteProhibited`

The contact is deleted when it is no longer associated with any objects, has no outstanding balance linked to the user ID, and is then scheduled for automatic deletion.

<a id="transfer-contact"></a>

#### transfer contact

**This command is not supported.**

When a [transfer domain](#transfer-domain) operation is performed, the contact object assigned to the registrar role is cloned. The cloned contact object is then assigned to the new portfolio, while the original contact object remains unchanged in the original portfolio.

<a id="host"></a>

### Host

The default behavior of the EPP `create host` command as described in [RFC:5732], will attach the client-ID (`CLID`) of the authenticated party to the created host object.

Responsibility and privileges for maintenance (update host) of the host object is assigned to the name server administrator as described in the [create host](#create-host) section. 

If the name server responsible is allocated to the registrar account, this can be handled via RP and EPP. 

<a id="create-host"></a>

#### create host

This part of the EPP protocol is described in [RFC:5732]. This command adheres to the standard. The command can be extended to specify another name server administrator than the authenticated user.

:point_right: Please note that IP addresses might be required for domain names ending in '.dk', please refer to the [glue record policy][DKHMDNSSPECGLUE].

:warning: By default, the authenticated registrar account is assigned as the designated name server administrator if the [`dkhm:requestedNsAdmin`](#dkhmrequestednsadmin) extension is excluded. 

Diagram of EPP create host [:eye_speech_bubble:][epp_create_host]

The command can be used in two scenarios:

1. The command is used as described in the RFC and the authenticated user is appointed as administrator for the name server created
2. The command is extended with a contact object pointing to an existing user, which is requested to take the role as name server administrator for the host object requested created

Below is a table of possible return codes for create host:

| Return Code | Description                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------|
| 1000        | If the create host command is successful                                                                 |
| 1001        | If the create host command awaits acknowledgment by the contact-id specified in `dkhm:requestedNsAdmin` |
| 2003        | If required IP address is not specified                                                                  |
| 2004        | If the specified IP addresses are non-public addresses                                                   |
| 2005        | Syntax of the command is not correct                                                                     |
| 2201        | If the authenticated user does not hold the privilege to update the specified host object                |
| 2302        | If the specified host object already exist                                                               |
| 2303        | If the contact-id pointed to in `dkhm:requestedNsAdmin` points to a non-existing contact object          |
| 2303        | If the domain name for the host is not registered                                                        |
| 2306        | If the specified name server administrator is a registrar account                                        |


Diagram of Punktum dk create host [:eye_speech_bubble:][dkh_create_host]

<a id="create-host-request"></a>

##### create host request

Request to create a host object, using both IPv4 and IPv6 addresses:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
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
    <clTRID>c0ea573a1bb87d48eaa4bd575788988c</clTRID>
  </command>
</epp>
```

<a id="create-host-response"></a>

##### create host response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <host:creData
        xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:name>ns1.eksempel.dk</host:name>
        <host:crDate>2025-08-30T09:27:32.0Z</host:crDate>
      </host:creData>
    </resData>
    <trID>
      <clTRID>c0ea573a1bb87d48eaa4bd575788988c</clTRID>
      <svTRID>8ED4A380-8583-11F0-A4BA-DC40448FBEE8</svTRID>
    </trID>
  </response>
</epp>
```

<a id="create-host-request-with-request-to-new-administrator"></a>

##### create host request with request to new administrator

Request to create a host object, requesting a different administrator of the host object.

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
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
      <dkhm:requestedNsAdmin
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">ADMIN1-DK
      </dkhm:requestedNsAdmin>
    </extension>
    <clTRID>c9944a06e4c3fbf02b7d14c399e494f1</clTRID>
  </command>
</epp>
```

<a id="create-host-response-from-request-to-new-administrator"></a>

##### create host response from request to new administrator

Response to the above request. The response indicates that the operation is pending, as the provided contact must accept the request.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1001">
      <msg>Command completed. Pending approval by nameserver contact</msg>
    </result>
    <trID>
      <clTRID>c9944a06e4c3fbf02b7d14c399e494f1</clTRID>
      <svTRID>7FC248C6-8582-11F0-877E-AADF8F433D60</svTRID>
    </trID>
  </response>
</epp>
```

<a id="check-host"></a>

### check host

This part of the EPP protocol is described in [RFC:5732]. This command adheres to the standard.

<a id="check-host-request"></a>

#### check host request

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <check>
      <host:check
        xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:name>ns1.dk-hostmaster.dk</host:name>
      </host:check>
    </check>
    <clTRID>ae5eae278543f2553bed07483eb2718c</clTRID>
  </command>
</epp>
```

<a id="check-host-response"></a>

#### check host response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <host:chkData
        xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:cd>
          <host:name avail="0">ns1.dk-hostmaster.dk</host:name>
          <host:reason>In use</host:reason>
        </host:cd>
      </host:chkData>
    </resData>
    <trID>
      <clTRID>ae5eae278543f2553bed07483eb2718c</clTRID>
      <svTRID>4AF163E8-8587-11F0-9861-B8ED2B97A61D</svTRID>
    </trID>
  </response>
</epp>
```

<a id="info-host"></a>

### info host

This part of the EPP protocol is described in [RFC:5732]. This command adheres to the standard.

Please note that according to the RFC [section 3.1.2][RFC:5732-3.1.2], the `ClID` points to the sponsoring client.

This field supports the two administrative models as follows:

- For registrar managed host object, the `CLID` points to the registrar, see also [Disclosure of Client ID](#disclosure-of-client-id)
- For registrant managed host objects, the `CLID` points to `DKHM1-DK`

<a id="info-host-request"></a>

#### info host request

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <info>
      <host:info
        xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:name>ns1.dk-hostmaster.dk</host:name>
      </host:info>
    </info>
    <clTRID>af0662654b8c5494cd62161242a27765</clTRID>
  </command>
</epp>
```

<a id="info-host-response"></a>

#### info host response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <host:infData
        xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:name>ns1.dk-hostmaster.dk</host:name>
        <host:roid>NS1_DK_HOSTMASTER_DK-DK</host:roid>
        <host:status s="ok"/>
        <host:clID>DKHM1-DK</host:clID>
        <host:crID>DKHM1-DK</host:crID>
        <host:crDate>2004-11-03T08:10:46.0Z</host:crDate>
        <host:upID>DKHM1-DK</host:upID>
        <host:upDate>2006-08-10T09:34:58.0Z</host:upDate>
      </host:infData>
    </resData>
    <trID>
      <clTRID>af0662654b8c5494cd62161242a27765</clTRID>
      <svTRID>89A5D1FA-8587-11F0-B139-BA92495989E1</svTRID>
    </trID>
  </response>
</epp>
```

<a id="update-host"></a>

#### update host

This part of the EPP protocol is described in [RFC:5732]. This command adheres to the standard but is extended to support name server administrator changes.

<a id="process"></a>

##### process

This is the overall process, the process is divided into sub-processes, please see the processes below for details.

Diagram of EPP update host [:eye_speech_bubble:][epp_update_host]

<a id="change-hostname-sub-process"></a>

##### Change hostname sub-process

The process of changing a host name is unsupported by Punktum dk and will always result in an error code: `2102`.

<a id="add-ip-address-sub-process"></a>

##### Add IP Address sub-process

Addition of IP addressed supports the additional of IPv4 and IPv6 addresses. These might be required as part of our [glue record policy][DKHMDNSSPECGLUE]. If additional status elements are added to this command it will fail.

| Return Code | Description                                            |
|-------------|--------------------------------------------------------|
| 1000        | If the update host command is successful               |
| 2004        | If the specified IP addresses are non-public addresses |
| 2005        | Syntax of the command is not correct                   |
| 2102        | Change of status for host object is not supported      |

Diagram of EPP update host add IP [:eye_speech_bubble:][epp_update_host_add_ip]

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <host:update xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:name>ns1.dk-hostmaster.dk</host:name>
        <host:add>
          <host:addr ip="v4">193.163.102.123</host:addr>
        </host:add>
      </host:update>
    </update>
    <clTRID>b65eced02e550d55117c5feb9ef1b82e</clTRID>
  </command>
</epp>
```

<a id="remove-ip-address-sub-process"></a>

##### Remove IP Address sub-process

Removal of IP addresses supports the removal of IPv4 and IPv6 addresses. These may be required to be removed in accordance with our [glue record policy][DKHMDNSSPECGLUE]. If additional status elements are added to this command, it will fail.

| Return Code | Description                                             |
|-------------|---------------------------------------------------------|
| 1000        | If the update host command is successful                |
| 2005        | Syntax of the command is not correct                    |
| 2102        | The command contains status elements                    |
| 2304        | The number of IP addresses are below the required limit |

Diagram of EPP update host remove IP [:eye_speech_bubble:][epp_update_host_remove_ip]

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <host:update xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:name>ns1.dk-hostmaster.dk</host:name>
        <host:rem>
          <host:addr ip="v4">80.80.80.80</host:addr>
        </host:rem>
      </host:update>
    </update>
    <clTRID>b65eced02e550d55117c5feb9ef1b82e</clTRID>
  </command>
</epp>
```

<a id="change-admin-sub-process"></a>

##### Change admin sub-process

Diagram of EPP update host change admin [:eye_speech_bubble:][epp_update_host_change_admin]

The command is extended with a contact object pointing to an existing user, which is requested to takeover the role as name server administrator for the host object.

The update of a host object can only be requested by the administrator of the given host. 

Below is a table of possible return codes for update host change admin:

| Return Code | Description                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------|
| 1000        | If the update host command is successful                                                                 |
| 1001        | If the update host command awaits acknowledgment by the contact-id specified in `dkhm:requestedNsAdmin` |
| 2004        | If the specified IP addresses are non-public addresses                                                   |
| 2005        | Syntax of the command is not correct                                                                     |
| 2102        | The command contains status elements                                                                     |
| 2201        | If the authenticated user does not hold the privilege to update the specified host object                |
| 2303        | If the specified host object does not exist                                                              |
| 2303        | If the contact-id pointed to in `dkhm:requestedNsAdmin` points to a non-existing contact object          |
| 2304        | The number of IP addresses are below the required limit                                                  |

As described in Implementation Limitations, the service does not support setting of status via update host.

Diagram of Punktum dk update host [:eye_speech_bubble:][dkh_update_host]

<a id="update-host-request-with-request-to-new-administrator"></a>

##### update host request with request to new administrator

Request to update a host object, requesting a different administrator of the host object, hence requiring offline evaluation.

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <update>
      <host:update
        xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:name>ns1.eksempel.dk</host:name>
      </host:update>
    </update>
    <extension>
      <dkhm:requestedNsAdmin
        xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">ADMIN2-DK
      </dkhm:requestedNsAdmin>
    </extension>
    <clTRID>30f43d8302d48c8bf79d9fe27776fe7a</clTRID>
  </command>
</epp>
```

<a id="update-host-response-with-request-to-new-administrator"></a>

##### update host response with request to new administrator

Response to the above request. The response indicates a successful accept of the request, but requires offline evaluation by the designated administrator of the host object, so the response indicates that the operation is pending.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1001">
      <msg>Command completed successfully; action pending</msg>
    </result>
    <trID>
      <clTRID>30f43d8302d48c8bf79d9fe27776fe7a</clTRID>
      <svTRID>7DAFF078-8588-11F0-BC92-B524AAD91788</svTRID>
    </trID>
  </response>
</epp>
```

<a id="delete-host"></a>

#### delete host

This part of the EPP protocol is described in [RFC:5732]. This command adheres to the standard.

Diagram of EPP delete host [:eye_speech_bubble:][epp_delete_host]

👉 The deletion of a host object can only be requested by the administrator.

Below is a table of possible return codes for delete host request:

| Return Code | Description                                                                               |
|-------------|-------------------------------------------------------------------------------------------|
| 1000        | If the delete host command is successful                                                  |
| 2201        | If the authenticated user does not hold the privilege to delete the specified host object |
| 2303        | If the specified host object does not exist                                               |
| 2305        | If the specified host object links to domain name objects                                 |

<a id="delete-host-request"></a>

##### delete host request

```XML
<?xml version="1.0" encoding="UTF-8"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <command>
    <delete>
      <host:delete
        xmlns:host="urn:ietf:params:xml:ns:host-1.0">
        <host:name>ns1.dk-hostmaster.dk</host:name>
      </host:delete>
    </delete>
    <clTRID>4a6afbeb2610c9ce7f93559420f07ddd</clTRID>
  </command>
</epp>
```

<a id="delete-host-response"></a>

##### delete host response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
  xmlns="urn:ietf:params:xml:ns:epp-1.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <trID>
      <clTRID>4a6afbeb2610c9ce7f93559420f07ddd</clTRID>
      <svTRID>AF75EA84-858A-11F0-9B60-F415D44FDF66</svTRID>
    </trID>
  </response>
</epp>
```

<a id="data-collection-policy"></a>

## Data Collection Policy

This chapter describes the data collection policy announced via the greeting available using the hello command. 

Please refer to the greeting response included in [hello and greeting](#hello-and-greeting) section.

<a id="access"></a>

### Access

The EPP service provides access to identified data relating to all available entities (personal and organizational) under the terms and conditions that anonymity will be applied as specified by the entities in question, and in accordance with [General Terms and Conditions][DKHMTAC] and legislation.

<a id="purpose-statement"></a>

### Purpose Statement

The collected data will be used solely for provisioning and administrative purposes. As specified under access above, and in the recipient statement below, some data are required to be publicly available and therefore some data will be accessible to the public under the circumstances specified in the referred sections. 

Address and contact information is collected in accordance with Danish legislation, except in cases where private individuals have name and address protection. 

<a id="recipient-statement"></a>

### Recipient Statement

Recipients of data are specified as other and unrelated. As specified in the purpose statement section and under access, identified data is made publicly available, therefore Punktum dk will not be able to control how the publicly available information is used.

<a id="retention-statement"></a>

### Retention Statement

Data will be retained with Punktum dk as required by Danish legislation.

<a id="references"></a>

## References

List of references used in this document in alphabetical order.

1. [Punktum dk: "Terms and conditions for the right of use to a .dk domain name"][DKHMTAC]
1. [Punktum dk: "Cooperation with our registrars"][CONCEPT]
1. [Punktum dk: EPP - general information][DKHMEPP]
1. [Punktum dk: ID check at Punktum dk][DKHMIDENT]
1. [Punktum dk: How waiting list works][DKHMWAITLIST]
1. [Punktum dk: Name Service Specification][DKHMDNSSPEC]
1. [Punktum dk: RESTful WHOIS Service Specification][DKHMWHOISRESTSPEC]
1. [Punktum dk: WHOIS Service Specification][DKHMWHOISSPEC]
1. [Punktum dk: Registrar Portal Service Specification][DKHMRPSPEC]
1. [Punktum dk: Sandbox Environment Specification][DKHMSANDBOX]
1. [Punktum dk: EPP XSD File Repository][DKHMXSD]
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

A list of resources for Punktum dk EPP service support is located below.

<a id="xsd-xml-schemas"></a>

### XSD/XML Schemas

This is a list in alphabetical order of the schemas currently used in the DKHM EPP Service described in this document. Please note that the XSD implementation preserves the original namespace and does not make alterations to this apart from adding the already described XML elements.

- `balance-1.0.xsd`
- `contact-1.0.xsd`
- `dkhm-4.5.xsd`
- `dkhm-domain-4.4`
- `domain-1.0.xsd`
- `epp-1.0.xsd`
- `eppcom-1.0.xsd`
- `host-1.0.xsd`
- `rgp-1.0.xsd`
- `secDNS-1.1.xsd`

The files are all available for [download][DKHMXSD]. Details on version history is available in the [EPP XSD Repository][DKHMXSD]

<a id="issue-reporting"></a>

### Issue Reporting

For issue reporting related to this specification, the EPP implementation or test, sandbox or production environments, please contact us via the regular support channels.

<a id="additional-information"></a>

### Additional Information

More generic information on EPP is available at the [Punktum dk website][DKHMEPP].

<a id="appendices"></a>

## Appendices

<a id="status-codes"></a>

### Status Codes

<a id="domain-status-codes"></a>

#### Domain Status Codes

This list of EPP domain status codes is based on information from [RFC:5731] and the ICANN status code interpretation: ["EPP Status Codes | What Do They Mean, and Why Should I Know?"][ICANN].

The description is the status, use and interpretation by Punktum dk.

As a general business rule, Punktum dk does not support the `client*` statuses, see also: [Unsupported Domain Status Codes](#unsupported-domain-status-codes) in the [Implementation Limitations](#implementation-limitations) section.

| Status Code                | Description                                                                                                                                                                                                             |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `addPeriod`                | _unsupported_ the status is not described in [RFC:5731] only in [ICANN resource][ICANN], see: [Unsupported Domain Status Codes](#unsupported-domain-status-codes)                                                       |
| `autoRenewPeriod`          | _unsupported_ the status is not described in [RFC:5731] only in [ICANN resource][ICANN], see: [Unsupported Domain Status Codes](#unsupported-domain-status-codes)                                                       |
| `clientDeleteProhibited`   | _unsupported_, see: [Unsupported Domain Status Codes](#unsupported-domain-status-codes)                                                                                                                                 |
| `clientHold`               | _unsupported_, see: [Unsupported Domain Status Codes](#unsupported-domain-status-codes)                                                                                                                                 |
| `clientRenewProhibited`    | _unsupported_, see: [Unsupported Domain Status Codes](#unsupported-domain-status-codes)                                                                                                                                 |
| `clientTransferProhibited` | _unsupported_, see: [Unsupported Domain Status Codes](#unsupported-domain-status-codes)                                                                                                                                 |
| `clientUpdateProhibited`   | _unsupported_, see: [Unsupported Domain Status Codes](#unsupported-domain-status-codes)                                                                                                                                 |
| `inactive`                 | _unsupported_ domain names in the Punktum dk registry **must** have associated name servers, , see: [Unsupported Domain Status Codes](#unsupported-domain-status-codes)                                              |
| `ok`                       | Exclusive for all other status codes                                                                                                                                                                                    |
| `pendingCreate`            | Indication that a the given domain is enqueued for possible creation, see [create domain](#create-domain) or is awaiting allocation with Punktum dk                                                                   |
| `pendingDelete`            | Deletion is pending, see [delete domain](#delete-domain). An advisory date is applicable via the extension [`dkhm:domainAdvisory`](dkhmdeldate)                                                                                |
| `pendingRenew`             | _unsupported_ as renewal is instantaneous, see: [Unsupported Domain Status Codes](#unsupported-domain-status-codes)                                                                                                     |
| `pendingRestore`           | _unsupported_ as restoration is instantaneous, see: [Unsupported Domain Status Codes](#unsupported-domain-status-codes)                                                                                                 |
| `pendingTransfer`          | _unsupported_ as transfer is instantaneous, see: [Unsupported Domain Status Codes](#unsupported-domain-status-codes)                                                                                                    |
| `pendingUpdate`            | The domain has active asynchronous requests, see [update domain](#update-domain)                                                                                                                                        |
| `redemptionPeriod`         | This status is applied when a domain name has `pendingDelete` and the delete operation can be redeemed using [restore domain](#restore-domain)                                                                          |
| `renewPeriod`              | _unsupported_ the status is not described in [RFC:5731] only in [ICANN resource][ICANN], see: [Unsupported Domain Status Codes](#unsupported-domain-status-codes)                                                       |
| `serverDeleteProhibited`   | Indicates whether the registrant or registrar can delete the domain                                                                                                                                                     |
| `serverHold`               | Given domain name is not active, it can hold a number of different _internal_ states rendering it on hold                                                                                                               |
| `serverRenewProhibited`    | Indicates a transient status where the billing or registrar contact is not able to renew the domain                                                                                                                     |
| `serverTransferProhibited` | Indicates status where the registrant or registrar contact is not able to transfer the domain                                                                                                                           |
| `serverUpdateProhibited`   | Indicates whether the registrant or registrar for a given domain can have ownership transferred, can appoint new proxy/admin contact, can appoint new billing contact, change name servers and can associate DS Records |
| `transferPeriod`           | _unsupported_ the status is not described in [RFC:5731] only in [ICANN resource][ICANN]                                                                                                                                 |

<a id="contact-status-codes"></a>

#### Contact Status Codes

This list of EPP contact status codes is based on information from [RFC:5733].

The description is the status, use and interpretation by Punktum dk.

As a general business rule, Punktum dk does not support the `client*` statuses, see also: [Unsupported Contact Status Codes](#unsupported-contact-status-codes) in the [Implementation Limitations](#implementation-limitations) section.

| Status Code                | Description                                                                                                                                                                                                  |
|----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `clientDeleteProhibited`   | _unsupported_, see: [Unsupported Contact Status Codes](#unsupported-contact-status-codes)                                                                                                                    |
| `clientTransferProhibited` | _unsupported_, see: [Unsupported Contact Status Codes](#unsupported-contact-status-codes)                                                                                                                    |
| `clientUpdateProhibited`   | _unsupported_, see: [Unsupported Contact Status Codes](#unsupported-contact-status-codes)                                                                                                                    |
| `linked`                   | Object is linked to other objects                                                                                                                                                                            |
| `ok`                       | Exclusive for all other status codes, except `linked`                                                                                                                                                        |
| `pendingCreate`            | _unsupported_ as creation is instantaneous, see: [Unsupported Contact Status Codes](#unsupported-contact-status-codes)                                                                                       |
| `pendingDelete`            | _unsupported_, see: [Unsupported Contact Status Codes](#unsupported-contact-status-codes)                                                                                                                    |
| `pendingTransfer`          | _unsupported_ as transfer is instantaneous, see: [Unsupported Contact Status Codes](#unsupported-domain-status-codes)                                                                                        |
| `pendingUpdate`            | _unsupported_ at this time                                                                                                                                                                                   |
| `serverDeleteProhibited`   | Always set deletions are an automated process and the delete command is not supported, see [Unimplemented Command: Delete Contact](#unimplemented-command-delete-contact)                                    |
| `serverTransferProhibited` | Always set as users cannot be transferred, see: [Unsupported Contact Status Codes](#unsupported-contact-status-codes) and [Unimplemented Command: Transfer Contact](#unimplemented-command-transfer-contact) |
| `serverUpdateProhibited`   | _unsupported_ at this time                                                                                                                                                                                   |

<a id="host-status-codes"></a>

#### Host Status Codes

This list of EPP host status codes is based on information from [RFC:5732].

The description is the status, use and interpretation by Punktum dk.

As a general business rule, Punktum dk does not support the `client*` statuses, see also: [Unsupported Host Status Codes](#unsupported-host-status-codes) in the [Implementation Limitations](#implementation-limitations) section.

| Status Code                | Description                                                                                                                             |
|----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| `clientDeleteProhibited`   | _unsupported_, see: [Unsupported Host Status Codes](#unsupported-host-status-codes)                                                     |
| `clientUpdateProhibited`   | _unsupported_, see: [Unsupported Host Status Codes](#unsupported-host-status-codes)                                                     |
| `linked`                   | Object is linked to other objects                                                                                                       |
| `ok`                       | No pending or prohibited operations. Exclusive for all other status codes, except `linked`                                              |
| `pendingCreate`            | Awaiting accept from registrant if required and awaiting accept from appointed name server administrator if required                    |
| `pendingDelete`            | Host object has been marked for deletion via deletion of superordinate domain name                                                      |
| `pendingTransfer`          | _unsupported_ as transfer is instantaneous, see: [Unsupported Host Status Codes](#unsupported-host-status-codes) and [RFC:5732]         |
| `pendingUpdate`            | Awaiting accept from appointed name server administrator if required                                                                    |
| `serverDeleteProhibited`   | If the host is linked is it not eligible for deletion                                                                                   |
| `serverTransferProhibited` | _unsupported_ as transfer for hosts is not defined, see: [Unsupported Host Status Codes](#unsupported-host-status-codes) and [RFC:5732] |
| `serverUpdateProhibited`   | If the host is marked for deletion (see `pendingDelete` this status will be set                                                         |

<a id="privilege-matrix-registrant-managed-objects"></a>

### Privilege Matrix for Registrant Managed Objects

| Command                                 | Sub-command                             |       Registrar        |   Domain name admin    |  Domain name billing   |      Name server admin      |
|-----------------------------------------|-----------------------------------------|:----------------------:|:----------------------:|:----------------------:|:---------------------------:|
| [login](#login)                         |                                         |   :white_check_mark:   | :white_check_mark: \*1 | :white_check_mark: \*1 |   :white_check_mark: \*1    |
| [create domain](#create-domain)         |                                         |   :white_check_mark:   |                        |                        |                             |
| [update domain](#update-domain)         |                                         |                        | :white_check_mark: \*2 |                        |   :white_check_mark: \*2    |
|                                         | add billing contact                     | :white_check_mark: \*8 | :white_check_mark: \*3 |                        |                             |
|                                         | remove billing contact                  | :white_check_mark: \*4 | :white_check_mark: \*4 | :white_check_mark: \*4 |                             |
|                                         | add admin contact                       |                        | :white_check_mark: \*5 |                        |                             |
|                                         | remove admin contact                    |                        | :white_check_mark: \*4 |                        |                             |
|                                         | change registrant                       |                        | :white_check_mark: \*6 |                        |                             |
|                                         | add name server                         |                        | :white_check_mark: \*6 |                        | :white_check_mark: \*6/\*10 |
|                                         | remove name server                      |                        | :white_check_mark: \*6 |                        | :white_check_mark: \*6/\*10 |
|                                         | add DSRECORDS                           |                        |   :white_check_mark:   |                        |   :white_check_mark: \*10   |
|                                         | remove DSRECORDS                        |                        |   :white_check_mark:   |                        |   :white_check_mark: \*10   |
|                                         | set AuthInfo for change of name servers |                        |   :white_check_mark:   |                        |     :white_check_mark:      |
|                                         | unset AuthInfo change of name servers   |                        |   :white_check_mark:   |                        |     :white_check_mark:      |
|                                         | set AuthInfo for transfer               |                        |   :white_check_mark:   |                        |                             |
|                                         | unset AuthInfo for transfer             |                        |   :white_check_mark:   |                        |                             |
| [renew domain](#renew-domain)           |                                         |                        |                        |   :white_check_mark:   |                             |
| [delete domain](#delete-domain)         |                                         |                        | :white_check_mark: \*6 |                        |                             |
| [restore domain](#restore-domain)       |                                         |                        |   :white_check_mark:   |                        |                             |
| [info domain](#info-domain)             |                                         | :white_check_mark: \*9 | :white_check_mark: \*9 | :white_check_mark: \*9 |   :white_check_mark: \*9    |
| [check domain](#check-domain)           |                                         |   :white_check_mark:   |   :white_check_mark:   |   :white_check_mark:   |     :white_check_mark:      |
| [transfer domain](#transfer-domain)     |                                         |                        |                        |                        |                             |
| [withdraw](#withdraw)                   |                                         |   :white_check_mark:   |                        |                        |                             |
| [create contact](#create-contact)       |                                         |   :white_check_mark:   |   :white_check_mark:   |   :white_check_mark:   |     :white_check_mark:      |
| [update contact](#update-contact)       |                                         | :white_check_mark: \*7 |                        |                        |   :white_check_mark: \*7    |
| [info contact](#info-contact)           |                                         | :white_check_mark: \*9 | :white_check_mark: \*9 | :white_check_mark: \*9 |   :white_check_mark: \*9    |
| [check contact](#check-contact)         |                                         |   :white_check_mark:   |   :white_check_mark:   |   :white_check_mark:   |     :white_check_mark:      |
| [create host](#create-host)             |                                         |   :white_check_mark:   |                        |                        |     :white_check_mark:      |
| [update host](#update-host)             |                                         |                        |                        |                        |     :white_check_mark:      |
| [delete host](#delete-host)             |                                         |                        |                        |                        |     :white_check_mark:      |
| [info host](#info-host)                 |                                         |   :white_check_mark:   |                        |                        |     :white_check_mark:      |
| [check host](#check-host)               |                                         |   :white_check_mark:   |                        |                        |     :white_check_mark:      |
| [poll](#poll-and-message-queue)         |                                         |   :white_check_mark:   |   :white_check_mark:   |   :white_check_mark:   |     :white_check_mark:      |
| [balance](#balance-and-prepaid-account) |                                         |   :white_check_mark:   |                        |   :white_check_mark:   |     :white_check_mark:      |

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

| Command                                 | Sub-command                             |       Registrar        |   Name server admin    |
|-----------------------------------------|-----------------------------------------|:----------------------:|:----------------------:|
| [login](#login)                         |                                         |   :white_check_mark:   |   :white_check_mark:   |
| [create domain](#create-domain)         |                                         |   :white_check_mark:   |                        |
| [update domain](#update-domain)         |                                         | :white_check_mark: \*1 | :white_check_mark: \*1 |
|                                         | add billing contact                     |                        |                        |
|                                         | remove billing contact                  |                        |                        |
|                                         | add admin contact                       |                        |                        |
|                                         | remove admin contact                    |                        |                        |
|                                         | change registrant                       |   :white_check_mark:   |                        |
|                                         | add name server                         |   :white_check_mark:   |   :white_check_mark:   |
|                                         | remove name server                      | :white_check_mark: \*2 | :white_check_mark: \*2 |
|                                         | add DSRECORDS                           |   :white_check_mark:   |   :white_check_mark:   |
|                                         | remove DSRECORDS                        |   :white_check_mark:   |   :white_check_mark:   |
|                                         | set AuthInfo for change of name servers |   :white_check_mark:   |   :white_check_mark:   |
|                                         | unset AuthInfo change of name servers   |   :white_check_mark:   |   :white_check_mark:   |
|                                         | set AuthInfo for transfer               |   :white_check_mark:   |                        |
|                                         | unset AuthInfo for transfer             |   :white_check_mark:   |                        |
| [renew domain](#renew-domain)           |                                         |   :white_check_mark:   |                        |
| [delete domain](#delete-domain)         |                                         |   :white_check_mark:   |                        |
| [restore domain](#restore-domain)       |                                         |   :white_check_mark:   |                        |
| [info domain](#info-domain)             |                                         | :white_check_mark: \*3 | :white_check_mark: \*3 |
| [check domain](#check-domain)           |                                         |   :white_check_mark:   |   :white_check_mark:   |
| [transfer domain](#transfer-domain)     |                                         | :white_check_mark: \*4 |                        |
| [withdraw](#withdraw)                   |                                         |   :white_check_mark:   |                        |
| [create contact](#create-contact)       |                                         |   :white_check_mark:   |   :white_check_mark:   |
| [update contact](#update-contact)       |                                         | :white_check_mark: \*5 | :white_check_mark: \*6 |
| [info contact](#info-contact)           |                                         | :white_check_mark: \*3 | :white_check_mark: \*3 |
| [check contact](#check-contact)         |                                         |   :white_check_mark:   |   :white_check_mark:   |
| [create host](#create-host)             |                                         |   :white_check_mark:   |                        |
| [update host](#update-host)             |                                         | :white_check_mark: \*7 |   :white_check_mark:   |
| [delete host](#delete-host)             |                                         | :white_check_mark: \*7 |   :white_check_mark:   |
| [info host](#info-host)                 |                                         |   :white_check_mark:   |   :white_check_mark:   |
| [check host](#check-host)               |                                         |   :white_check_mark:   |   :white_check_mark:   |
| [poll](#poll-and-message-queue)         |                                         |   :white_check_mark:   |   :white_check_mark:   |
| [balance](#balance-and-prepaid-account) |                                         |   :white_check_mark:   |   :white_check_mark:   |

- \*1 see sub-commands
- \*2 changes status of existing DSRECORDS
- \*3 can only see contact information for authorized objects, access to registrant is authorized as public other roles require authorization via relation
- \*4 requires authorization
- \*5 only data not locked by business rules and under external registry administration such as CPR, CVR and VIES registers
- \*6 only own profile
- \*7 only subordinate host names

<a id="feature-and-meta-role-matrix"></a>

### Feature and Meta-role Matrix

| Command                                 | Sub-command                             | Meta-role                 |
|-----------------------------------------|-----------------------------------------|:--------------------------|
| [login](#login)                         |                                         | All                       |
| [create domain](#create-domain)         |                                         | Registrar                 |
| [update domain](#update-domain)         |                                         | Proxy                     |
|                                         | add billing contact                     | Proxy                     |
|                                         | remove billing contact                  | Proxy                     |
|                                         | add admin contact                       | Proxy                     |
|                                         | remove admin contact                    | Proxy                     |
|                                         | change registrant                       | Proxy                     |
|                                         | add name server                         | Name Server Administrator |
|                                         | remove name server                      | Name Server Administrator |
|                                         | add DSRECORDS                           | Name Server Administrator |
|                                         | remove DSRECORDS                        | Name Server Administrator |
|                                         | set AuthInfo for change of name servers | Name Server Administrator |
|                                         | unset AuthInfo change of name servers   | Name Server Administrator |
|                                         | set AuthInfo for transfer               | Proxy                     |
|                                         | unset AuthInfo for transfer             | Proxy                     |
| [renew domain](#renew-domain)           |                                         | Payer                     |
| [delete domain](#delete-domain)         |                                         | Proxy                     |
| [restore domain](#restore-domain)       |                                         | Proxy                     |
| [info domain](#info-domain)             |                                         | All                       |
| [check domain](#check-domain)           |                                         | All                       |
| [transfer domain](#transfer-domain)     |                                         | Proxy                     |
| [withdraw](#withdraw)                   |                                         | Proxy                     |
| [create contact](#create-contact)       |                                         | All                       |
| [update contact](#update-contact)       |                                         | Proxy                     |
| [info contact](#info-contact)           |                                         | All                       |
| [check contact](#check-contact)         |                                         | All                       |
| [create host](#create-host)             |                                         | Name Server Administrator |
| [update host](#update-host)             |                                         | Name Server Administrator |
| [delete host](#delete-host)             |                                         | Name Server Administrator |
| [info host](#info-host)                 |                                         | All                       |
| [check host](#check-host)               |                                         | All                       |
| [poll](#poll-and-message-queue)         |                                         | All                       |
| [balance](#balance-and-prepaid-account) |                                         | Payer                     |

<a id="compatibility-matrix"></a>

### Compatibility Matrix

This is a high level overview of the EPP commands offered by the Punktum dk EPP service, please see the specific commands for details.

The version numbers used in the matrix are major numbers only, e.g. 1 for 1.X.X.

| EPP Command                             | Available since version | Exceptions and notes                                                                              |
|-----------------------------------------|-------------------------|---------------------------------------------------------------------------------------------------|
| [Log in](#login)                        | 1                       | Only password authentication is supported                                                         |
| [Log out](#logout)                      | 1                       |                                                                                                   |
| [Create domain](#create-domain)         | 1                       | Asynchronous \*1                                                                                  |
| [Check Domain](#check-domain)           | 1                       |                                                                                                   |
| [Info Domain](#info-domain)             | 1 / 3                   | Billing contact not disclosed. Proxy contact not disclosed since version 3                        |
| [Update Domain](#update-domain)         | 2                       | Asynchronous \*2                                                                                  |
| [Renew Domain](#renew-domain)           | 2                       |                                                                                                   |
| [Transfer Domain](#transfer-domain)     | 4                       |                                                                                                   |
| [Withdraw](#withdraw)                   | 4                       |                                                                                                   |
| [Delete Domain](#delete-domain)         | 4                       |                                                                                                   |
| [Restore Domain](#restore-domain)       | 4                       |                                                                                                   |
| [Create Contact](#create-contact)       | 1                       | Supplying handle/user-id is not supported                                                         |
| [Check Contact](#check-contact)         | 1 / 3                   | Only registrants disclosed, other contacts requires relation to authenticated user                |
| [Info Contact](#info-contact)           | 1 / 3                   | Only registrants disclosed, other contacts requires relation to authenticated user                |
| [Update Contact](#update-contact)       | 2                       | Asynchronous \*3                                                                                  |
| [Transfer Contact](#transfer-contact)   | N/A                     | Contact objects cannot be transferred only domain names. Contact objects are cloned upon transfer |
| [Delete Contact](#delete-contact)       | N/A                     | Deletion of contacts is an automated process                                                      |
| [Create Host](#create-host)             | 2                       | Asynchronous \*4                                                                                  |
| [Check Host](#check-host)               | 1                       |                                                                                                   |
| [Info Host](#info-host)                 | 1                       |                                                                                                   |
| [Update Host](#update-host)             | 2                       | Asynchronous \*5                                                                                  |
| [Delete Host](#delete-host)             | 2                       |                                                                                                   |
| [Poll](#poll-and-message-queue)         | 1                       |                                                                                                   |
| [Balance](#balance-and-prepaid-account) | 4                       |                                                                                                   |

- \*1 Requires order confirmation by the registrant. VID product not supported, PO numbers not supported
- \*2 Change of name server is asynchronous, requires approval by the registrant
- \*3 Updating email is asynchronous and is regarded as non-atomic due to the email validation process
- \*4 Requires accept of the registrant of the domain name if the domain is under the .dk TLD and requires that the requesting user accepts the responsibility as name server administrator
- \*5 Requires that the requested administrator accepts the responsibility as name server administrator

[DKHMLOGO]: https://punktum.dk/sites/default/files/logo/dk_logo_symbol_1.png
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
[epp_create_domain]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_create_domain_v1.0.png
[epp_create_contact]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_create_contact_v1.0.png
[RFC:3339]: https://tools.ietf.org/html/rfc3339
[RFC:3735]: https://tools.ietf.org/html/rfc3735
[RFC:3915]: https://tools.ietf.org/html/rfc3915
[RFC:5730]: https://tools.ietf.org/html/rfc5730
[RFC:5731]: https://tools.ietf.org/html/rfc5731
[RFC:5731-3.2.4]: https://datatracker.ietf.org/doc/html/rfc5731#section-3.2.4
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
[CONCEPT]: https://punktum.dk/en/articles/cooperation-with-our-registrars
[DKHMWHOISSPEC]: https://github.com/DK-Hostmaster/whois-service-specification
[DKHMRPSPEC]: https://github.com/DK-Hostmaster/rp-service-specification
[DKHMWHOISRESTSPEC]: https://github.com/DK-Hostmaster/whois-rest-service-specification
[BALANCE]: https://www.verisign.com/assets/epp-sdk/verisign_epp-extension_balance_v01.html
[DKHMWAITLIST]: https://punktum.dk/en/articles/how-waiting-list-works
[DKHMIDENT]: https://punktum.dk/en/articles/id-check-at-punktum-dk
[DKHMEPP]: https://punktum.dk/en/articles/epp
[DKHMSANDBOX]: https://github.com/DK-Hostmaster/sandbox-environment-specification
[DKHMTAC]: https://punktum.dk/en/articles/terms-and-conditions-for-the-right-of-use-to-a-dk-domain-name
[DKHMMAIL]: https://punktum.dk/en/articles/operational-status
[DKHMPRICES]: https://punktum.dk/en/articles/prices-and-fees
[DKHMVID]: https://punktum.dk/en/articles/vid-service
[DKHM]: https://www.punktum.dk/
