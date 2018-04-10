DK Hostmaster EPP Service Specification

2017-12-19
Revision: 2.2

# Table of Contents

<!-- MarkdownTOC bracket=round levels="1,2,3" -->

- [Introduction](#introduction)
	- [About this Document](#about-this-document)
	- [License](#license)
	- [Document History](#document-history)
- [The .dk Registry in Brief](#the-dk-registry-in-brief)
- [EPP in Brief](#epp-in-brief)
- [EPP Service](#epp-service)
	- [SSL/TLS Support](#ssltls-support)
	- [Available Environments](#available-environments)
		- [production](#production)
		- [sandbox](#sandbox)
- [Implementation Requirements](#implementation-requirements)
	- [Client Transaction ID \(`clTRID`\)](#client-transaction-id-cltrid)
	- [IP Whitelisting](#ip-whitelisting)
- [Implementation Extensions](#implementation-extensions)
	- [`dkhm:userType`](#dkhmusertype)
	- [`dkhm:EAN`](#dkhmean)
	- [`dkhm:CVR`](#dkhmcvr)
	- [`dkhm:pnumber`](#dkhmpnumber)
	- [`dkhm:trackingNo`](#dkhmtrackingno)
	- [`dkhm:domainAdvisory`](#dkhmdomainadvisory)
	- [`dkhm:orderconfirmationToken`](#dkhmorderconfirmationtoken)
	- [`dkhm:domain_confirmed`](#dkhmdomain_confirmed)
	- [`dkhm:contact_validated`](#dkhmcontact_validated)
	- [`dkhm:registrant_validated`](#dkhmregistrant_validated)
	- [`dkhm:mobilephone`](#dkhmmobilephone)
	- [`dkhm:secondaryEmail`](#dkhmsecondaryemail)
	- [`dkhm:requestedNsAdmin`](#dkhmrequestednsadmin)
- [Implementation Limitations](#implementation-limitations)
	- [Commands](#commands)
	- [Unimplemented commands](#unimplemented-commands)
	- [Additional limitations](#additional-limitations)
	- [DNSSEC](#dnssec)
	- [Contact Creation](#contact-creation)
	- [Host Update](#host-update)
	- [Domain Update](#domain-update)
	- [Information Disclosure](#information-disclosure)
	- [Encoding and IDN domains](#encoding-and-idn-domains)
- [Supported Object Transform and Query Commands](#supported-object-transform-and-query-commands)
	- [hello and greeting](#hello-and-greeting)
	- [login](#login)
		- [login request](#login-request)
		- [login response](#login-response)
	- [logout](#logout)
		- [logout request](#logout-request)
		- [logout response](#logout-response)
	- [poll and message queue](#poll-and-message-queue)
		- [poll req request](#poll-req-request)
		- [poll req response](#poll-req-response)
		- [poll ack request](#poll-ack-request)
		- [poll ack response](#poll-ack-response)
		- [poll ack response for non-existant message \(or previously acknowledged message\)](#poll-ack-response-for-non-existant-message-or-previously-acknowledged-message)
	- [create domain](#create-domain)
		- [create domain request](#create-domain-request)
		- [create domain response](#create-domain-response)
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
		- [add nameserver](#add-nameserver)
		- [remove nameserver](#remove-nameserver)
		- [add contact](#add-contact)
		- [remove contact](#remove-contact)
	- [check host](#check-host)
		- [check host request](#check-host-request)
		- [check host response](#check-host-response)
	- [info host](#info-host)
		- [info host request](#info-host-request)
		- [info host response](#info-host-response)
	- [create host](#create-host)
		- [create host request](#create-host-request)
		- [create host response](#create-host-response)
		- [create host request with request to new administrator](#create-host-request-with-request-to-new-administrator)
		- [create host response from request to new administrator](#create-host-response-from-request-to-new-administrator)
		- [Delayed create host response, from request to new administrator](#delayed-create-host-response-from-request-to-new-administrator)
		- [create host request, with request to registrant of host domain name](#create-host-request-with-request-to-registrant-of-host-domain-name)
		- [create host response, from request to registrant of domain name](#create-host-response-from-request-to-registrant-of-domain-name)
		- [Delayed create host response, from request to registrant of domain name](#delayed-create-host-response-from-request-to-registrant-of-domain-name)
	- [update host](#update-host)
		- [Proces](#proces)
		- [Change hostname sub-proces](#change-hostname-sub-proces)
		- [Add IP sub-proces](#add-ip-sub-proces)
		- [Remove IP sub-proces](#remove-ip-sub-proces)
		- [Change admin sub-proces](#change-admin-sub-proces)
		- [update host request with request to new administrator](#update-host-request-with-request-to-new-administrator)
		- [update host response with request to new administrator](#update-host-response-with-request-to-new-administrator)
		- [Delayed update host response from request to new administrator](#delayed-update-host-response-from-request-to-new-administrator)
	- [delete host](#delete-host)
		- [delete host request](#delete-host-request)
		- [delete host response](#delete-host-response)
	- [create contact](#create-contact)
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
- [Data Collection Policy](#data-collection-policy)
	- [Access](#access)
	- [Purpose Statement](#purpose-statement)
	- [Recipient Statement](#recipient-statement)
	- [Retention Statement](#retention-statement)
- [References](#references)
- [Resources](#resources)
	- [XML Schemas](#xml-schemas)
		- [XSD Version History](#xsd-version-history)
	- [Mailing list](#mailing-list)
	- [Issue Reporting](#issue-reporting)
	- [Demo/Test Client](#demotest-client)
	- [Additional Information](#additional-information)
	- [Pre-activation Service](#pre-activation-service)
- [Appendices](#appendices)
	- [Greeting](#greeting)
	- [Status Codes](#status-codes)
		- [Domain](#domain)
	- [Privilege Matrix](#privilege-matrix)
	- [Compatibility Matrix](#compatibility-matrix)

<!-- /MarkdownTOC -->


<a id="introduction"></a>
# Introduction

This document describes and specifies the implementation offered by DK Hostmaster for interaction with the central registry for the ccTLD dk using the Extensible Provisioning Protocol (EPP). It is primarily aimed at a technical audience, and the reader is required to have prior knowledge of DNS registration and EPP.

<a id="about-this-document"></a>
## About this Document

This specification describes version 2.X.X of the DK Hostmaster EPP Implementation. Future releases will be reflected in updates to this specification, please see the document history section below.

The document describes the current DK Hostmaster EPP implementation, for more general documentation on the EPP protocol, EPP client development or configuration, please refer to the RFCs and additional resources in the [References](#references) and [Resources](#resources) chapters below.

Any future extensions and possible additions and changes to the implementation are not within the scope of this document and will not be discussed or mentioned throughout this document.

This document is owned and maintained by DK Hostmaster A/S and must not be distributed without this information.

All examples provided in the document are fabricated or changed from real data to demonstrate commands etc. any resemblence to actual data are coincidental.


<a id="license"></a>
## License

This document is copyright by DK Hostmaster A/S and is licensed under the MIT License, please see the separate LICENSE file for details.


<a id="document-history"></a>
## Document History

* HEAD 2018-04-03
  * Added information on format of Orderconfirmation Token

* 2.1 2017-06-08
  * Removed information on waiting list handling, since this is being revisited

* 2.0 2016-10-24
  * Describes EPP service 2.X.X 
  * Added renew domain description
  * Added update domain description
  * Added create/update/delete host descriptions
  * Added update contact description
  * Added XSD 2.0 description

* 1.10 2016-06-08
  * Added information on IP whitelisting 

* 1.9 2016-01-30
  * Information on new waiting list handling
  * Information on new DNSSEC key handling

* 1.8 2015-09-03
  * Minor corrections
  * More information on extensions for possible registration of the DK Hostmaster extensions with IANA in relation to [RFC:7451][RFC:7451]
  * Added [RFC:7451][RFC:7451] compliant descriptions in subdirectory: `rfc7451/`

* 1.7 2015-05-12
  * This revision of the specification is describing EPP service release 1.3.X
  * This release also updates the [XSD][XSD Files] specification to revision 1.4, introducing the extension pnumber for transport of production unit numbers for validation of danish companies as part of the create contact command

* 1.6 2015-01-06
  * This revision of the specification is describing EPP service release 1.2.X
  * This release also updates the [XSD][XSD Files] specification to revision 1.3
  * The document has with this revision been ported from a proprietary format to markdown and is being hosted on github for easier maintenance and distribution, this has resultet in a lot of minor corrections and clarifications.
  * Extended the section about this document, due to the migration to Github, so copyright is now explicitly mentioned
  * info contact command extended with validation information
  * create domain command extended with validation information for registrant
  * create domain command extended with information on confirmation status for domain

* 1.5 2014-06-18
  * This revision of the specification is describing EPP service release 1.1.X
  * The test environment is no longer active
  * Examples updated to latest [XSD][XSD Files] revision (1.2)
  * Pre-activation token (`orderconfirmationToken`) can be transported via extension for create domain command
  * Multiple examples of requests and responses added

* 1.4 2013-11-19
  * Corrected links in resources
  * Empasized the use of the `auto` keyword for contact creation, this has also been listed in the implementation limitations section
  * Added information on the restrictive use of `clTRID` in new section entitled: Implementation Requirements

* 1.3 2013-10-29
  * This revision of the specification is describing EPP service release 1.0.9
  * Added information on use of `clTRID` in context of create domain command
  * Added more information on the domain check command, which has been extended with EPP service release 1.0.9.
  * This release also updates the [XSD][XSD Files] specification to revision 1.1

* 1.2 2013-08-07
  * This revision of the specification is describing EPP service release 1.0.8
  * Added note on domain check

* 1.1 2013-05-31
  * Added paragraph on passwords in section on the login command
  * Added mention of standard port 700
  * Corrected some of the XML examples, which had not been updated to reflect the correct use of [XSDs][XSD Files]
  * Added important note on contact creation

* 1.0 2013-02-25
  * Initial revision
  * Describes EPP service 1.X.X
  * Introduces [XSD][XSD Files] specification revision 1.0


<a id="the-dk-registry-in-brief"></a>
# The .dk Registry in Brief

DK Hostmaster is the registry for the ccTLD for Denmark (dk). The current model used in Denmark is based on a sole registry, with DK Hostmaster maintaining the central DNS registry.

The legislation and registry model utilised in Denmark imposes some limitations compared to the EPP protocol in general, since the primary intent of the EPP protocol is focused on a model based on shared-registry rather than a sole-registry model like the one used in Denmark.

These limitations are described in detail below in the chapter entitled Implementation Limitations, and these are explained further in the command descriptions where the single commands deviate from the EPP standard specification. In addition to limitations and deviations found in the above, a few others have been implemented to support DNS registration under Danish legislation, these are described in detail under the individual commands, where relevant.

Our EPP extensions are registered with the [IANA EPP Extension Repository][IANA EPP Extension Repository].


<a id="epp-in-brief"></a>
# EPP in Brief

EPP is an XML-based protocol aimed at provisioning data between registries. The protocol is intended for machine-to-machine communication in a client-server setup. Please see the References chapter for more information on specifications and references for EPP.

Please note that the service does not support XML entity expansion on the server side, due to security implications related to this feature.


<a id="epp-service"></a>
# EPP Service

The DK Hostmaster’s EPP Service is based on an SOA architecture. EPP implementation is regarded as a service offered to external parties requiring provisioning actions towards DK Hostmaster.

The EPP service requires the use of and possible development of EPP client software. This is beyond the scope of this specification as the API and other assets for assisting in this are the primary object of this document.

In addition to the assets, DK Hostmaster aims to assist users and developers of EPP client software with integration towards DK Hostmaster and therefore provide facilities to ease this integration. This is primarily centered around a sandbox environment and related documentation.

The service is implemented under the following principles:

1 Adhere to the standard to the extent possible or use non-intrusive extensions to support the requirements or finally use mandatory extensions to adhere to service requirements
1 Use _in-band_ communication, meaning requests made via  EPP will be responded to via EPP unless the end-user have specified differently
1 Use standard error code to the extent possible, communicating state more clearly and unambigiously


<a id="ssltls-support"></a>
## SSL/TLS Support

The EPP service supports the following protocols for transport security:

- TLSv1.2 


<a id="available-environments"></a>
## Available Environments

DK Hostmaster offers the following environments:


<a id="production"></a>
### production

  * epp.dk-hostmaster.dk runs the EPP service 2.X.X

  * This environment is the production environment
  * info and check requests made to this environment will reflect live production data
  * create requests made to this environment will be carried out provided that they comply with business rules and general terms
  * Approved domains will be processed for possible activation and propagation into the zone
  * Contacts (users) will be created and will be available in other systems like the self-service system etc.
  * Hosts (name servers) will be processed for possible activation
  * The Change Password operation is available in this environment
  * Please note that this operation will change the password and this change will be reflected in other systems
  * This is environment is using [IP Whitelisting](#ip-whitelisting)
  * This environment is only available to registrars
  * Both environments respond on port 700


<a id="sandbox"></a>
### sandbox

  * This environment runs EPP service version 2.X.X

  * This environment is intended for client development towards the DK Hostmaster EPP service
  * info and check requests made to this environment will reflect sandbox data. For host objects, some static content synched in by DK Hostmaster, in addition to sandbox data
  * create requests made to this environment will be serialised in the sandbox environment, provided that syntax and data are valid
  * Domains will be enqueued, but will not be processed further nor be available for activation and propagation into the zone
  * Contacts (users) can be created, but will not be available in other systems like the self-service system etc.

  * The Change Password operation will only change the password on the sandbox environment
  * The sandbox environment is available at: epp-sandbox.dk-hostmaster.dk port 700
  * This environment is available to both registrars and nameserver administrators

Please note that when you first start to use the EPP sandbox environment, the access credentials are matching your production credentials. If these do not work as expected (e.g. error `2200`). please contact: tech@dk-hostmaster.dk to get the credentials synhcronized.


<a id="implementation-requirements"></a>
# Implementation Requirements

This section outlines the overall requirements in regard to implementing an EPP client to work with the DK Hostmaster EPP service.


<a id="client-transaction-id-cltrid"></a>
## Client Transaction ID (`clTRID`)

In order to ensure transactional integrity and due to the asynchronous nature of some of the EPP commands, we rely on the client transaction id to be unique. This is unique as per client id. The assists in ensuring that a delayed response can be easily identified by simple means.

The `clTRID` is recommended to be unique for all transactions and is required to be unique for the create domain command. This might change in the future.


<a id="ip-whitelisting"></a>
## IP Whitelisting

Since 2016-02-29 DK Hostmaster has enforced IP whitelisting of IPs for access to the EPP service. Additions and removals of IP addresses is currently a manual proces handled by DK Hostmaster. 

Please submit change requests including registrar handle information to:

* tech@dk-hostmaster.dk


<a id="implementation-extensions"></a>
# Implementation Extensions

The EPP service implemented by DK Hostmaster holds several extensions, these are documented where appropriate for the specific commands etc. This section serves to give an overview of the extensions as a whole. 

Please refer to the [dkhm-2.0][XSD Files] for implementation details.

Here follows a listed, the extensions are described separately and in detail below.

* `dkhm:userType`
* `dkhm:EAN`
* `dkhm:CVR`
* `dkhm:pnumber`
* `dkhm:mobilephone`
* `dkhm:secondaryEmail`
* `dkhm:trackingNo`
* `dkhm:domainAdvisory`
* `dkhm:orderconfirmationToken`
* `dkhm:domain_confirmed`
* `dkhm:contact_validated`
* `dkhm:registrant_validated`
* `dkhm:requestedNsAdmin`


<a id="dkhmusertype"></a>
## `dkhm:userType`

The `userType` extension is used to categorize a contact type, since the requirements for data differs between the different usertypes, we need to be able to differenciate between: company, individual, public organisation and association. More information is available under the create contact command.

Related extensions are `dkhm:EAN`, `dkhm:CVR` and `dkhm:pnumber`.


<a id="dkhmean"></a>
## `dkhm:EAN`

The EAN extension, holds the EAN number associated with public organisations in Denmark. The field is mandatory for this type of contact objects and is required for electronic invoicing, more information is available under the create contact command.


<a id="dkhmcvr"></a>
## `dkhm:CVR`

The CVR extension is for holding VAT registration numbers. The number is used for validation and VAT accounting. More information is available under the create contact command.


<a id="dkhmpnumber"></a>
## `dkhm:pnumber`

The pnumber extension is for holding production-unit numbers, used for validation for danish companies, with more physical addressed related to one VAT number. More information is available under the create contact command.


<a id="dkhmtrackingno"></a>
## `dkhm:trackingNo`

A unique tracking number for a domain registration for uniformity with the mail form. EPP it not the only channel of domain registration and in order to handle registrations via multiple channel, a unique tracking-id is assigned to every request. More information is available under the create domain command.


<a id="dkhmdomainadvisory"></a>
## `dkhm:domainAdvisory`

Any special circumstances in relation to a domain name, can be communicated using this special field. Please see the specific commands for examples.


<a id="dkhmorderconfirmationtoken"></a>
## `dkhm:orderconfirmationToken`

This is a special field for supporting the business flow where the agreement for a domain name is accepted by the registrant with the registrar. More information is available under the create domain command.


<a id="dkhmdomain_confirmed"></a>
## `dkhm:domain_confirmed`

Domain names registered with DK Hostmaster, has to be confirmed by the registrant, this is can either be done using pre-activation, see the `orderconfirmationToken` above or other systems with DK Hostmaster, the domain confirmation state is available via the create domain command using this extension.

See also `orderconfirmationToken`.


<a id="dkhmcontact_validated"></a>
## `dkhm:contact_validated`

Contact objects related to the role of registrant has to be validated, this field is used to indicate the status of a validation object via the info contact command.


<a id="dkhmregistrant_validated"></a>
## `dkhm:registrant_validated`

As described above, contact objects related to the role of registrant has to be validated, this field is used to indicate the status of a validation object via the create domain command.

See also `contact_validated`.


<a id="dkhmmobilephone"></a>
## `dkhm:mobilephone`

Contact objects can have a mobilephone number in addition to `voice` and `fax`. The extension was introduced in the DK Hostmaster XSD file set 1.6.


<a id="dkhmsecondaryemail"></a>
## `dkhm:secondaryEmail`

Contact objects can have a secondary email address in addition to `email`. The extension was introduced in the DK Hostmaster XSD file set 1.6.


<a id="dkhmrequestednsadmin"></a>
## `dkhm:requestedNsAdmin`

The extension is used for update and create host, where it is possible to request another nameserver administrator than the authenticated user. The extension was introduced in the DK Hostmaster XSD file set 1.5.


<a id="implementation-limitations"></a>
# Implementation Limitations

As mentioned previously the EPP service comes with some limitations. Please see the [Compatibility Matrix](compatibility-matrix) in the appendices.


<a id="commands"></a>
## Commands

The current implementation implements the following list of commands:

* hello
* login, including change password
* logout
* poll, including acknowledgement of messages
* info (contact/domain/host)
* check (contact/domain/host)
* create (contact/domain/host)
* renew (domain)
* update (contact/domain/host)
* delete (host)

All commands are described in detail below.


<a id="unimplemented-commands"></a>
## Unimplemented commands

The following commands have not been implemented in the service described in this specification:

* delete (contact/domain)
* transfer (contact/domain)

In general the service is not localized and all EPP related errors and messages are provided in English.


<a id="additional-limitations"></a>
## Additional limitations

The service does not support the following features of the EPP protocol:

* Authorization
* Transport of `authInfo`, the section is ignored is not recommended for transport of end-user passwords

Comparing the EPP implementation to the existing channel for domain registration using the form via SMTP, the following fields are not supported.

* VID (VIP domain name)
* Billing contact's purchase order (PO) number


<a id="dnssec"></a>
## DNSSEC

I accordance with [RFC 5910][RFC5910]. We support DS only and not DNSKEY. In addition the maximum signature lifetime (`secDNS:maxSigLife`) is disregarded. See [section 3.3](http://tools.ietf.org/html/rfc5910#section-3.3) in the referenced RFC.

DK Hostmaster specifies rules ownership of DNSSEC keys. If you provide DNSSEC keys a part of registration, the keys are associated with the registrant as owner. If you want to specify another owner, please specify the `tech` or `keyholder` role (see: Role Mapping under: create domain command).

Not all algorithms are supported, please refer to the [DK Hostmaster Name Service specification][dkhm-name-service-specification] for a complete list of supported algorithms.


<a id="contact-creation"></a>
## Contact Creation

This command does not support the feature of providing a predefined userid. The userid has to be specified as `auto` and the userid is assigned by DK Hostmaster. See also information on the create contact command.


<a id="host-update"></a>
## Host Update

This command does not support the setting and removal of status using the XML element: `host:status`. The status is assigned by DK Hostmaster. See also information on the update host command.


<a id="domain-update"></a>
## Domain Update

This command does not support the change of the registrant and the setting and removal of status using the XML element: `domain:status`. The status is assigned by DK Hostmaster. See also information on the update domain command.


<a id="information-disclosure"></a>
## Information Disclosure

Please note that some information is not disclosed when using Object Query Commands. See the specific commands for more information.


<a id="encoding-and-idn-domains"></a>
## Encoding and IDN domains

The danish registry supports IDN domain names and the EPP commands support punycode notation for this in requests. We do however not support punycode notation in responses at this time.


<a id="supported-object-transform-and-query-commands"></a>
# Supported Object Transform and Query Commands

The following describes the currently supported EPP commands. As mentioned previously, some of the commands have been extended beyond the basic capabilities of EPP. These minor extensions are described separately under each command and are included in the [XSD files][XSD Files] listed in the Resources chapter.

Commands that have not been extended are not described in much detail, please refer to the general EPP documentation from IETF (see: the RFCs listed in References).


<a id="hello-and-greeting"></a>
## hello and greeting

This part of the EPP protocol is described in [RFC 5730][RFC5730]. This command adheres to the standard. For a more detailed explanation of the data collection policy announced via the greeting, please see the Data Collection Policy chapter.

As announced in the greeting, the following objects are available:

* Host
* Domain
* Contact

With regard to extensions, the following are available:

* [secDNS-1.1][XSD Files]
* [dkhm-2.0][XSD Files]

Please see the greeting response included in the [appendices](greeting) for illustration of the actual announcement.


<a id="login"></a>
## login

This part of the EPP protocol is described in [RFC 5730][RFC5730]. This command adheres to the standard.

The login uses the general AAA functionality in DK Hostmaster. This mean that in addition to the validation of username and password specified as part of the login request, an attempt is made to authorise the authenticated user for access to the actual EPP service and subsequent operations.

Authorisation is currently only available to specified user roles, therefore the username provided must point to an entity with the role of registrar or nameserver administrator with the DK Hostmaster registry. See also Available Environments above.

DK Hostmaster supports the change of passwords via EPP. Please refer to the chapter Available Environments for any special circumstances.

Password should adhere to the following requirements:

EPP supports  a password with at least 6 and max 16, where DK Hostmaster supports 8 - 64 characters. The password must include at least three of these four character types:

* Lower-case letters
* Upper-case letters
* Numbers
* Special Characters

The following characters are legal special characters in passwords:

```
% ` ' ( ) * + - , . / : ; < > = ! _ & ~ { } | ^ ? $ # @ " [ ]
```

Currently, the only language supported is English. So the language parameter is ignored and all responses are provided in English.


<a id="login-request"></a>
### login request

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
        <objURI>domainurn:ietf:params:xml:ns:domain-1.0urn:ietf:params:xml:ns:domain-1.0 domain-1.0.xsd</objURI>
        <objURI>hosturn:ietf:params:xml:ns:host-1.0urn:ietf:params:xml:ns:host-1.0 host-1.0.xsd</objURI>
        <objURI>contacturn:ietf:params:xml:ns:contact-1.0urn:ietf:params:xml:ns:contact-1.0 contact-1.0.xsd</objURI>
      </svcs>
    </login>
    <clTRID>d52eaf8995d2b679fe9dc53ee5bc3ad9</clTRID>
  </command>
</epp>
```


<a id="login-response"></a>
### login response

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
## logout

This part of the EPP protocol is described in [RFC 5730][RFC5730]. This command adheres to the standard.

There are no special additions or alterations to the specification or use of this command.


<a id="logout-request"></a>
### logout request

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
### logout response

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
## poll and message queue

This part of the EPP protocol is described in [RFC 5730][RFC5730]. This command adheres to the standard.

There are no special additions or alterations to the specification or use of this command.

For clarification `2303` is returned in case a provided message-id (`msgID`) point to a non-existing message.


<a id="poll-req-request"></a>
### poll req request

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
### poll req response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>    
    <result code="1301">
      <msg>Command completed successfully; ack to dequeue</msg>    </result>    
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
### poll ack request

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
### poll ack response

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


<a id="poll-ack-response-for-non-existant-message-or-previously-acknowledged-message"></a>
### poll ack response for non-existant message (or previously acknowledged message)

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

<a id="create-domain"></a>
## create domain

This part of the EPP protocol is described in [RFC 5730][RFC5730]. This command adheres to the standard. DK Hostmaster, however, is based on an asynchronous domain creation workflow. All domain requests are enqueued for further processing and their creation will be in a state of pending.

Please note:

- `authInfo` section is ignored is not recommended for transport of end-user passwords

A well-formed request for domain creation will then always result in:

```
1001, “Commmand completed successfully; action pending”
```

The extension in response will provide a unique tracking number, which can be used to identify the creation request across provisioning channels offered by DK Hostmaster. The result of the further processing will be relayed back via EPP, see poll and message queue above.

So the customised response for a domain creation request looks as below.

The create domain command has been extended with a field (`orderconfirmationToken`.) making it possible to assign a token indicating that the registrant has agreed to the terms and coniditions for DK Hostmaster with the registrar.

```XML
<dkhm:orderconfirmationToken xmlns:dkhm=“urn:dkhm:params:xml:ns:dkhm-2.1”>
	1522744544
</dkhm:orderconfirmationToken>
```

The token is a timestamp in EPOCH format, indicating when the agreement was accepted.

The `token` is handled the following way:

- If absent DK Hostmaster will require the agreement for the terms and conditions be accepted with DK Hostmaster, this process is handled by DK Hostmaster

- If present. The token will be validated by DK Hostmaster
  - if not valid the request with result in an error and the request will be dismissed
  - if valid the request will be accepted and processed

The timestamp is regarded as valid for 4 days, so the validation accepts timestamps within the following interval:

- Up to 4 days old
- Up to 5 minutes into the future

Do note that the validation of the timestamp is based on the UTC timezone.

The requirement for the registrant to be valid is communicated via the response, using the extension:
`dkhm:registrant_validated`. Please see the command info contact for more information. The state is communicated in this response in order to provide information on the further flow and process of the create domain request.

The status codes applying to domain are described in the addendum: Status Codes: Domain.


<a id="create-domain-request"></a>
### create domain request

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
      <dkhm:orderconfirmationToken xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-1.2">testtoken</dkhm:orderconfirmationToken>
    </extension>
    <clTRID>92724843f12a3e958588679551aa988d</clTRID>
  </command>
</epp>
```


<a id="create-domain-response"></a>
### create domain response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1001">
      <msg>Create domain pending for domain1.dk</msg>
    </result>
    <msgQ count="1" id="1"/>
    <extension>
      <dkhm:trackingNo xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-1.3">2013010100030</dkhm:trackingNo>
      <dkhm:domain_confirmed xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-1.3">0</dkhm:domain_confirmed>
      <dkhm:registrant_validated xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-1.3">1</dkhm:registrant_validated>
    </extension>
    <trID>
      <clTRID>47a4178679f26909ebcfcfd8572f315c</clTRID>
      <svTRID>EDF4F436-9CC9-11E4-AC57-51CB2AC2711D-2013010100030</svTRID>
    </trID>
  </response>
</epp>
```

This tracking number (`trackingNo`), listed as an extension and does not replace or interfere with the normal use of EPP’s transaction keys, `clTRID` and `svTRID`, but are EPP specific, whereas the tracking number is considered global in DK Hostmaster. The tracking number is also appended to the `svTRID` in addition to the listing in the extension part. Please see the last digits following the last dash.

```XML
<svTRID>9917BE58-3D53-11E2-A5BD-C532BF0DC46A-1234</svTRID>
```

An important note is that the `clTRID` is mandatory for this command. Since we use the `clTRID` to report back via the message polling functionality, when the domain creation request changes state.

The default value for domain value, if not specified, is one year.


<a id="role-mapping"></a>
### Role Mapping

As for the user entities some mappings are made so all relevant roles are specified.

| EPP | DKHM | Fallback | Note |
| ------------- | ----------- | ----------- | ----------- |
| admin | administrator (fuldmægtig) | registrant | optional, will use fallback |
| billing | billing (betaler) | registrant | optional, will use fallback |
| tech | keyholder (nøgleansvarlig) | | optional, will be ignored if keyholder is specified |
| registrant | registrant | mandatory | |
| registrar | registrar | mandatory | |

Please note that the command supports punycode notation for specifying IDN domain names, but responses are in the specified UTF-8 character set.

![Diagram of role resolution for EPP create domain][epp-role-resolution]


<a id="check-domain"></a>
## check domain


<a id="check-domain-request"></a>
### check domain request

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
### check domain response

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

In general this part of the EPP protocol is described in [RFC 5731][RFC5731] and his command adheres to the standard.


<a id="info-domain"></a>
## info domain

This part of the EPP protocol is described in [RFC 5731][RFC5731]. This command adheres to the standard.

Please see the addendum on domain status codes.


<a id="info-domain-request"></a>
### info domain request

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
### info domain response

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>    
    <result code="1000">
      <msg>Info result</msg>
    </result>    
    <msgQ count="1" id="4">
    </msgQ>    
    <resData>
      <domain:infData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>dk-hostmaster.dk</domain:name>
        <domain:roid>DK_HOSTMASTER_DK-DK</domain:roid>
        <domain:status s="serverDeleteProhibited"/>
        <domain:status s="serverUpdateProhibited"/>
        <domain:status s="serverRenewProhibited"/>
        <domain:status s="serverTransferProhibited"/>
        <domain:registrant>DKHM1-DK</domain:registrant>
        <domain:contact type="admin">DKHM1-DK</domain:contact>
        <domain:ns>
          <domain:hostObj>auth01.ns.dk-hostmaster.dk</domain:hostObj>
          <domain:hostObj>auth02.ns.dk-hostmaster.dk</domain:hostObj>
          <domain:hostObj>p.nic.dk</domain:hostObj>
        </domain:ns>
        <domain:host>ns10.dk-hostmaster.dk</domain:host>
        <domain:host>gr1.dk-hostmaster.dk</domain:host>
        <domain:host>gr2.dk-hostmaster.dk</domain:host>
        <domain:host>ns.dk-hostmaster.dk</domain:host>
        <domain:host>auth01.ns.dk-hostmaster.dk</domain:host>
        <domain:host>ns1.dk-hostmaster.dk</domain:host>
        <domain:host>papkasse.dk-hostmaster.dk</domain:host>
        <domain:host>papkassehuset.dk-hostmaster.dk</domain:host>
        <domain:host>parat1.dk-hostmaster.dk</domain:host>
        <domain:host>parat2.dk-hostmaster.dk</domain:host>
        <domain:host>smukkehansi.dk-hostmaster.dk</domain:host>
        <domain:host>smukkehansi15.dk-hostmaster.dk</domain:host>
        <domain:host>ns4.dk-hostmaster.dk</domain:host>
        <domain:host>hostcount.dk-hostmaster.dk</domain:host>
        <domain:host>venteliste1.dk-hostmaster.dk</domain:host>
        <domain:host>venteliste2.dk-hostmaster.dk</domain:host>
        <domain:host>dnegle.dk-hostmaster.dk</domain:host>
        <domain:host>blocked1.ns.dk-hostmaster.dk</domain:host>
        <domain:host>blocked2.ns.dk-hostmaster.dk</domain:host>
        <domain:host>auth02.ns.dk-hostmaster.dk</domain:host>
        <domain:host>ns2.dk-hostmaster.dk</domain:host>
        <domain:host>ns.25.dnegle.dk-hostmaster.dk</domain:host>
        <domain:host>ns3.dk-hostmaster.dk</domain:host>
        <domain:host>ææ.dk-hostmaster.dk</domain:host>
        <domain:host>æøö.dk-hostmaster.dk</domain:host>
        <domain:host>øæå.dk-hostmaster.dk</domain:host>
        <domain:clID>DKHM1-DK</domain:clID>
        <domain:crID>DK_WHOIS</domain:crID>
        <domain:crDate>1998-01-19T00:00:00.0Z</domain:crDate>
        <domain:exDate>2020-03-31T00:00:00.0Z</domain:exDate>
      </domain:infData>
    </resData>    
    <trID>
      <clTRID>71e77199292ea1a5fd5e7918f2da7cc0</clTRID>
      <svTRID>30DB64F6-8F8F-11E6-A066-DCC11F9D93B1</svTRID>    
    </trID></response>
</epp>
```

The example is obsolete and will be replaced with post implementation of the domain renew command (see below).


<a id="renew-domain"></a>
## renew domain

This part of the EPP protocol is described in [RFC 5731][RFC5731]. This command adheres to the standard.

![Diagram of EPP proces for EPP renew domain][epp-renew-domain]

| Return Code  | Description |
| ------------ | ------------ |
| 2005 | Syntax of the command is not correct |
| 2303 | If the specified domain object does not exist |
| 2201 | If the authenticated user does not hold the privilege to renew the specified domain object. This privilege is given to the billing contact for the domain name (see also the [login command](#login)) |
| 2306 | If the specified expiry date is not valid. The provided expiration date has to be equal to the current expiration date or we return `2306` |
| 2306 | If the calculated expiry date is not allowed. The new expiration date has to be lower than the current expiration date + 5 years. The maximum period to which the expiration date can be extended is 5 years and 3 months. The current expiration date is available via the [info domain](#info-domain) command as `domain:exDate` |
| 2105 | If the domain object is not eligible for renewal. The domain name has to be in the state ‘Active’. This will also be reflected in status value `serverRenewProhibited`. See also [ICANN description](https://www.icann.org/resources/pages/epp-status-codes-2014-06-16-en/#serverRenewProhibited) of status |
| 2400 | In case of an exception |
| 1000 | If the renew domain command is successful |

This complete proces is atomic and might throw an unrecoverable exception: `2400` either due to unforeseen circumstances or a change in the state of the domain name.

On success we emit the return code `1000`. No further communication is made via the EPP service. An invoice is generated and is distributed out of band for EPP as shown in the sub-proces and an additional *message* is sent out of band for EPP to the billing contact and the registrant

The sub-proces called, can be depicted as follows:

![Diagram of DKH sub-proces for EPP renew domain][dkh-renew-domain]


<a id="renew-domain-request"></a>
### renew domain request

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
### renew domain response

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
## update domain

This part of the EPP protocol is described in [RFC 5731][RFC5731]. This command does not adhere to the standard

- `authInfo` section is ignored is not recommended for transport of end-user passwords
- `contact` object in the  `ns` section is ignored

This command covers a lot of functionality, it can complete operations such as:

- change registrant for domain
- add nameserver to domain
- remove nameserver from domain
- add admin contact
- remove admin contact
- add billing contact
- remove billing contact

In addition it supports DNSSEC management capabilities as specified in [RFC 5910][RFC5910]

The command will be evaluated as an atomic command, even though it is dispatched to several sub-commands.

![Diagram of EPP proces for EPP update domain][epp-update-domain]

The requirements for the command to commence with processing it that the following data are available:

- a valid domain name
- a sub-command, consisting of either
  - add (`add`)
  - change (`chg`)
  - remove (`rem`)

If the request is not parsable the service responds with a `2005`.

If the command is parsable, the command is separated into one of more of the following sub-commands (by order of precedence):

1. change registrant
1. remove nameserver
1. remove admin contact
1. remove billing contact
1. add nameserver
1. add admin contact
1. add billing contact

The commands are then executed sequentially (order is dictates the precedence) as a single transaction. If a single sub-command fails, the transaction is rolled-back and the relevant error code is returned (`2XXX`).

The command might be stopped if the sub-commands cannot be executed. For example if one of the sub-commands is a: change registrant, none of the other commands can be executed, since role changes will be implicit. 

When the command succeeds either `1000` or `1001` is returned the latter if one of the operations initiated by the sub-command require additional actions to be taken, `1001` will have precedence over `1000`. If a `1001` is returned the status code `pendingUpdate` might be set if an additional **update domain** command is issued.

| Return Code  | Description |
| ------------ | ------------ |
| 1000 | If the update domain command is successful |
| 1001 | If the update domain command awaits acknowledgement by 3rd. party |
| 2005 | Syntax of the command is not correct |
| 2102 | Change of status for host object is not supported |
| 2201 | If the authenticated user does not hold the privilege to update the specified domain object |
| 2303 | If the specified domain name does not exist |
| 2303 | If the specified host name does not exist, for when adding a new nameserver |
| 2303 | If the specified host name does not exist, for when removing a nameserver |
| 2303 | If the specified userid  does not exist, for when adding a new billing contact |
| 2304 | If the specified host name does not link with the specified domain name, for when removing a nameserver |
| 2307 | Unimplemented object service, the service does not support change of registrant on a domain |
| 2308 | The number of name servers are below the required limit |

Please see the below sections for details on the different sub-commands.

The command might be blocked and the status code: `serverUpdateProhibited` is returned indicating that an update is not possible. The status code `clientUpdateProhibited` will be returned if the issued update request cannot be fullfilled due to a domain lock with the registry. See also [ICANN description](https://www.icann.org/resources/pages/epp-status-codes-2014-06-16-en/) of status codes.


<a id="update-domain-request"></a>
### update domain request

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

TODO: The above example is error prone, it will be replaced with a correct example.


<a id="update-domain-response"></a>
### update domain response

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
### change registrant

The change of registrant is a *special* operation, it results in all privileges and rights being transferred to another entity. A registrar does not hold the privileges to complete such a request, so the object service is unimplemented at this time.

![Update domain - Change registrant][epp-update-domain-change-registrant]

| Return Code  | Description |
| ------------ | ------------ |
| 2307 | Unimplemented object service, the service does not support change of registrant on a domain |


<a id="add-nameserver"></a>
### add nameserver

The addition of a new nameserver to a domain name or a re-delegation requires that the new nameserver must offer resolution for the domain name in question.

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

![Update domain - Add nameserver][epp-update-domain-add-ns]

| Return Code  | Description |
| ------------ | ------------ |
| 1000 | If the update domain command is successful |
| 2005 | Syntax of the command is not correct |
| 2201 | If the authenticated user does not hold the privilege to update the specified domain object |
| 2303 | If the specified domain name does not exist |
| 2303 | If the specified host name does not exist, for when adding a new nameserver |


<a id="remove-nameserver"></a>
### remove nameserver

The removal of a existing nameserver from a domain name requires that at least two other name servers are offering resolution for the domain in question, else the command will fail.

Since the update domain command can contain several sub-commands, this could be accompanied by an *add nameserver* (see above), so the policy requirement is met and resolution is kept.

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

![Update domain - Remove nameserver][epp-update-domain-remove-ns]

| Return Code  | Description |
| ------------ | ------------ |
| 1000 | If the update domain command is successful |
| 2005 | Syntax of the command is not correct |
| 2201 | If the authenticated user does not hold the privilege to update the specified domain object |
| 2303 | If the specified domain name does not exist |
| 2303 | If the specified host name does not exist, for when removing a nameserver |
| 2304 | If the specified host name does not link with the specified domain name, for when removing a nameserver |
| 2308 | The number of name servers are below the required limit |


<a id="add-contact"></a>
### add contact

The addition of a new contact has to adhere to some policies.

1. If the contact is the admin, only the billing role can be added
2. If the authenticated user is a registrar only billing can be added
3. The new contact is requested to accept the role, so the operation is asynchronous

Additing new users require special privileges, currently only with the registrant, apart from the policy listed above.

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

![Update domain - Add billing/admin contact sub-proces][dkh-update-domain-add-contact]


<a id="remove-contact"></a>
### remove contact

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

![Update domain - Remove billing/admin contact sub-proces][dkh-update-domain-remove-contact]


<a id="check-host"></a>
## check host

This part of the EPP protocol is described in [RFC 5732][RFC5732]. This command adheres to the standard.


<a id="check-host-request"></a>
### check host request

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
### check host response

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
## info host

This part of the EPP protocol is described in [RFC 5732][RFC5732]. This command adheres to the standard.

Please note that according to the RFC [section 3.1.2][RFC5732-3.1.2], the `CLID` points to the sponsoring client. DK Hostmaster interprets this as the tehnical contact for the nameserver pointing to the host object in question.


<a id="info-host-request"></a>
### info host request

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
### info host response

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


<a id="create-host"></a>
## create host

This part of the EPP protocol is described in [RFC 5732][RFC5732]. This command adheres to the standard. The command can be extended to specify another nameserver administrator than the authenticated user.

Please note that IP addresses are required for domain names ending in '.dk', please refer to the [glue record policy](https://github.com/DK-Hostmaster/dkhm-name-service-specification#glue-records).

![Diagram of EPP create host][epp_create_host]

The command can be used in two scenarios:

1. The command is used as described in the RFC and the authenticated user is appointed as administrator for the nameserver created
2. The command is extended with a contact object pointing to an existing user, which is requested to take the role as nameserver administrator for the host object requested created

| Return Code  | Description |
| ------------ | ------------ |
| 1000 | If the create host command is successful |
| 1001 | If the create host command awaits acknowledgement by the contact-id specified in `dkhm:requestedNsAdmin` |
| 2003 | If required IP address is not specified |
| 2004 | If the specified IP addresses are non-public addresses |
| 2005 | Syntax of the command is not correct |
| 2201 | If the authenticated user does not hold the privilege to update the specified host object |
| 2302 | If the specified host object already exist |
| 2303 | If the contact-id pointed to in `dkhm:requestedNsAdmin` points to a non-existing contact object |
| 2303 | If the domain name for the host is not registered |
| 2306 | If the specified nameserver administrator is a registrar account |

As for update domain `1001` holds higher precendence than `1000`, so if any of the sub-commands require additional review and are _pending_, the return code will be `1001`.

![Diagram of DKH create host][dkh_create_host]


<a id="create-host-request"></a>
### create host request

Request to create a host object, using both IPv4 and IPv6 adresses and the authenticated user is the registrant of the specified domain name and requested adminstrator of the host object.

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
### create host response

Response to the above request. The reponse indicates a succesful creation, since the operation could be completed successfully without requiring offline evaluation.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
  <response>
    <result code="1000">
      <msg>Command completed successfully</msg>
    </result>
    <resData>
      <host:creData
       xmlnhost="urn:ietf:paramxml:nhost-1.0">
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
### create host request with request to new administrator

Request to create a host object, requesting a different adminstrator of the host object, hence requiring offline evaluation.

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
      <dkhm:requestedNsAdmin xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-2.0">ADMIN2-DK</dkhm:requestedNsAdmin>
    </extension>
    <clTRID>ABC-12345</clTRID>
  </command>
</epp>
```


<a id="create-host-response-from-request-to-new-administrator"></a>
### create host response from request to new administrator

Response to the above request. The response indicates a succesful accept of the requiest, but requires offline evaluation by the designated administrator of the host object, so the response indicates that the operation is pending.

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
### Delayed create host response, from request to new administrator

If the creation of the host has resulting in a delayed operation, pending the designated nameserver administrator, the below example shows what a poll message for the final state of the operation would look like.

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
### create host request, with request to registrant of host domain name

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
### create host response, from request to registrant of domain name

Response to the above request. The reponse indicates a succesful accept of the requiest, but requires offline evaluation by the registrant of the specified domain namem, so the response indicates that the operation is pending.

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
### Delayed create host response, from request to registrant of domain name

If the creation of the host has resulting in a delayed operation, pending the designated nameserver administrator, the below example shows what a poll message for the final state of the operation would look like.

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


<a id="update-host"></a>
## update host

This part of the EPP protocol is described in [RFC 5732][RFC5732]. This command adheres to the standard, but is extended to service one special usage scenario. 


<a id="proces"></a>
### Proces

This is the overall proces, the proces is divided into sub-processes, please see the processes below for details.

![Diagram of EPP update host][epp_update_host]


<a id="change-hostname-sub-proces"></a>
### Change hostname sub-proces

The proces of changing a host name us unsupported by DK Hostmaster and will always result in an error code: `2102`.

![Diagram of EPP update host change hostname][epp_update_host_change_hostname]

| Return Code  | Description |
| ------------ | ------------ |
| 2102 | Change of hostname is not supported |


<a id="add-ip-sub-proces"></a>
### Add IP sub-proces

Addition of IP addressed supports the additional of IPv4 and IPv6 adresses. These are required as part of our glue record policy. If additional status elements are added to this command it will fail.

| Return Code  | Description |
| ------------ | ------------ |
| 1000 | If the update host command is successful |
| 2004 | If the specified IP addresses are non-public addresses  |
| 2005 | Syntax of the command is not correct |
| 2102 | Change of status for host object is not supported |

![Diagram of EPP update host add IP][epp_update_host_add_ip]


<a id="remove-ip-sub-proces"></a>
### Remove IP sub-proces

Addition of IP addressed supports the additional of IPv4 and IPv6 adresses. These are required as part of our glue record policy. If additional status elements are added to this command it will fail.

| Return Code  | Description |
| ------------ | ------------ |
| 1000 | If the update host command is successful |
| 2005 | Syntax of the command is not correct |
| 2102 | The command contains status elements |
| 2304 | The number of IP addresses are below the required limit |

![Diagram of EPP update host remove IP][epp_update_host_remove_ip]


<a id="change-admin-sub-proces"></a>
### Change admin sub-proces

![Diagram of EPP update host change admin][epp_update_host_change_admin]

The command can be used in two scenarios:

1. The command is used as described in the RFC and IP adresses can be administered
2. The command is extended with a contact object pointing to an existing user, which is requested to takeover the role as nameserver administrator for the host object requested updated

The update of a host object can only be requested by the adminstrator of the given host.

| Return Code  | Description |
| ------------ | ------------ |
| 1000 | If the update host command is successful |
| 1001 | If the update host command awaits acknowledgement by the contact-id specified in `dkhm:requestedNsAdmin` |
| 2004 | If the specified IP addresses are non-public addresses  |
| 2005 | Syntax of the command is not correct |
| 2102 | The command contains status elements |
| 2201 | If the authenticated user does not hold the privilege to update the specified host object |
| 2303 | If the specified host object does not exist |
| 2303 | If the contact-id pointed to in `dkhm:requestedNsAdmin` points to a non-existing contact object |
| 2304 | The number of IP addresses are below the required limit |

As for update host `1001` holds higher precendence than `1000`, so if any of the sub-commands require additional review and are _pending_, the return code will be `1001`.

As described in Implementation Limitations, the service does not support setting of status via update host.

![Diagram of DKH update host][dkh_update_host]


<a id="update-host-request-with-request-to-new-administrator"></a>
### update host request with request to new administrator

Request to update a host object, requesting a different adminstrator of the host object, hence requiring offline evaluation.

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
      <dkhm:requestedNsAdmin xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-2.0">DKHM1-DK</dkhm:requestedNsAdmin>
    </extension>
    <clTRID>7a4ac69d335ae661e29fc2c262c5800e</clTRID>
  </command>
</epp>
```


<a id="update-host-response-with-request-to-new-administrator"></a>
### update host response with request to new administrator

Response to the above request. The response indicates a succesful accept of the requiest, but requires offline evaluation by the designated administrator of the host object, so the response indicates that the operation is pending.

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
### Delayed update host response from request to new administrator

If the creation of the host has resulting in a delayed operation, pending the designated nameserver administrator, the below example shows what a poll message for the final state of the operation looks like.

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
## delete host

This part of the EPP protocol is described in [RFC 5732][RFC5732]. This command adheres to the standard.

![Diagram of EPP delete host][epp_delete_host]

The deletion of a host object can only be requested by the adminstrator.

| Return Code  | Description |
| ------------ | ------------ |
| 1000 | If the delete host command is successful |
| 2201 | If the authenticated user does not hold the privilege to delete the specified host object |
| 2303 | If the specified host object does not exist |
| 2305 | If the specified host object links to domain name objects |


<a id="delete-host-request"></a>
### delete host request

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
### delete host response

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


<a id="create-contact"></a>
## create contact

This part of the EPP protocol is described in [RFC 5733][RFC5733].

This command has been extended with the following fields:

* `dkhm:usertype`, which has to be one of:
  * `company` - indicating a company
  * `public_organization` - indicating a public organisation
  * `association` - indicating an association
  * `individual` - indicating an individual

The user type will result in context-specific interpretation of the following fields:

* EAN - this number is only supported for user types: `company`, `public_organization` and `association`. It is only mandatory for `public_organization` and optional for `company` and `association`. [EAN][EAN description] is used by the public sector in Denmark for electronic invoicing, private companies can also be assigned EAN, but this it not so widespread at this time. EAN is required by law for public sector organisations, so this field has to be completed and it has to validate for this type.
* CVR - (VAT number) this is only supported for user types: `company`, `public_organization` and `association`. The number is required for handling VAT correctly, mandatory for user types `company` and `public_organization` and optional for the user type `association`.
* pnumber - (production unit number) this is only supported for user types: `company`, `public_organization` and `association`. The number is used for handling validation correctly and the field is optional.

The `contact-id` field is auto-generated and assigned by DK Hostmaster. EPP do however open for providing a contact-id in the context of the create contact command, this is not supported by DK Hostmaster at this point.

This field is validated on the server site, it is however recommended to perform a check contact on the requested contact-id prior to the create domain request if a userid is already known from a contact create or previous domain creation.

It is required that the client side can request that the contact-id is auto-generated and assigned by DK Hostmaster by providing the keyword `auto`, which will result in an available and validated contact-id for the specified contact object.

```XML
<contact:id>auto</contact:id>
```

Please note that the `auto` keyword is in lower-case.

Contact creation under EPP opens for the ability to represent postal information in both local and international representations. Due to the representation in DK Hostmasters system for handling contacts the following rules are applied to postal information.

For Denmark the local representation is chosen and the international representation is discarded. For other countries the international representation is chosen and the local representation is discarded. Please see the table below.

| Denmark | Other country |
| ----------- | ----------- |
| **Local representation** | Local representation |
| International representation | **International representation** |

This is a diagram depicting the general algorithm used for resolving the address data. The algorithm presupposes that at least one address is present.

![Diagram of address resolution for contact creation][epp-address-resolution]

It is important to note that if the international representation is specified, but data are provided in local representation or only local representation is provided for an international address, communication to the specified address might prove unreliable.

The handling of name and organisation is also a special case. Where the following mapping is made based on the user type.

<table>
<tr>
	<th></th><th colspan="2">Name and Organisation Provided</th><th>Only name provided</th>
<tr>
	<th>User type</th><th>Name (<i>mandatory</i>)</th><th>Organisation (<i>optional</i>)</th><th>Name (<i>mandatory</i>)</th>
</tr>
<tr>
	<td>C (Company)</td><td>attention</td><td>name</td><td>name</td>
</tr>
<tr>
	<td>P (Public organisation)</td><td>attention</td><td>name</td><td>name</td>
</tr>
<tr>
	<td>A (Association)</td><td>attention</td><td>name</td><td>name</td>
</tr>
<tr>
	<td>I (Individual)</td><td>name</td><td>-</td><td>name</td>
</tr>
</table>

Please note that a registrant cannot have a attention field specified, so you should use name solely for creation of contacts intended to be used as registrants for the types: company, public organisation and association

The data is collected as required by danish legislation. See also the data collection policy section below.

Please note:

- `authInfo` section is ignored is not recommended for transport of end-user passwords
- User-creation is silent and the designated user is not notified about the the creation, unless this is a part of the proces of associating the user with other objects


<a id="create-contact-request"></a>
### create contact request

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
      <dkhm:userType xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-1.2">company</dkhm:userType>
      <dkhm:CVR xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-1.2">1234567891231</dkhm:CVR>
    </extension>
    <clTRID>8cced469f2bfdbb0dcad16b875d87c99</clTRID>
  </command>
</epp>
```


<a id="create-contact-response"></a>
### create contact response

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
## check contact

This part of the EPP protocol is described in [RFC 5733][RFC5733]. This command adheres to the standard.


<a id="check-contact-request"></a>
### check contact request

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
### check contact response

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
## info contact

This part of the EPP protocol is described in [RFC 5733][RFC5733]. This command has been extended with information on whether the contact in queried has been validated according to requirements and policies with DK Hostmaster.

See the extension: `dkhm:contact_validated` in the response.

Please note that the email address (`contact:email`) is masked and the value: `anonymous@dk-hostmaster.dk` is always return for this field.


<a id="info-contact-request"></a>
### info contact request

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
### info contact response

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
      <dkhm:contact_validated xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-1.3">1</dkhm:contact_validated>
    </extension>
    <trID>
      <clTRID>76edfef5b78cdaefe8fb426eb8d74b75</clTRID>
      <svTRID>C8C5E496-9CC8-11E4-9F91-D0BF2AC2711D</svTRID>
    </trID>
  </response>
</epp>
```


<a id="update-contact"></a>
## update contact

This part of the EPP protocol is described in [RFC 5733][RFC5733]. This command adheres to the standard. In addition to the standard the command allows for manipulation of the extensions associated with contact objects, meaning that it is possible to update the following fields:

- [`dkhm:userType`](#dkhmusertype)
- [`dkhm:EAN`](#dkhmean)
- [`dkhm:CVR`](#dkhmcvr)
- [`dkhm:pnumber`](#dkhmpnumber)
- [`dkhm:mobilephone`](#dkhmmobilephone)
- [`dkhm:secondaryEmail`](#dkhmsecondaryEmail)

These of course all controlled by relevant privileges.

- Name / Organisation
- Address
- Country
- Phone (voice)
- Fax
- Email
- Secondary email
- Mobilephone

![Diagram of EPP update contact][epp-update-contact]

Please note:

- `authInfo` section is ignored is not recommended for transport of end-user passwords


<a id="update-contact-request"></a>
### update contact request

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
        <dkhm:secondaryEmail xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-1.5">email@eksempel.dk</dkhm:secondaryEmail>
        <dkhm:mobilephone xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-1.5">+1.7034444445</dkhm:mobilephone>
    </extension>
    <clTRID>ABC-12345</clTRID>
  </command>
</epp>
```


<a id="update-contact-response"></a>
### update contact response

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
## delete contact

**This command is not supported.**

This command will always return: `2101`, indicating unimplemented command.

The deletion of contact objects is handled automatically by DK Hostmaster. The following status flags will be set:

- clientDeleteProhibited
- serverDeleteProhibited

The later will only be lifted when the contact object is not linked to any other objects and automatic deletion is scheduled.


<a id="delete-contact-request"></a>
### delete contact request

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
### delete contact response

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


<a id="data-collection-policy"></a>
# Data Collection Policy

This chapter describes the data collection policy announced via the greeting available using the hello command.

Please refer to the [greeting response example](#greeting) included in the [Appendices](#Appendices).


<a id="access"></a>
## Access

The EPP service provides access to identified data relating to all available entities (personal and organisational) under the terms and conditions that anonymity will be applied as specified by the entities in question, and in accordance with [General Terms and Conditions][General Terms and Conditions] and legislation.


<a id="purpose-statement"></a>
## Purpose Statement

The collected data will be used solely for provisioning and administrative purposes. As specified under access above, and in the recipient statement below, some data are required to be publicly available and therefore some data will be accessible to the public under the circumstances specified in the referred sections.

Address data and contact information is collected as required by danish legislation.


<a id="recipient-statement"></a>
## Recipient Statement

Recipients of data are specified as other and unrelated. As specified in the purpose statement section and under access, identified data is made publicly available, therefore DK Hostmaster will not be able to control how the publicly available information is used.


<a id="retention-statement"></a>
## Retention Statement

Data will be retained with DK Hostmaster as required by Danish legislation.


<a id="references"></a>
# References

Here is a list of documents and references used in this document

* [DK Hostmasters General Terms and Conditions][General Terms and Conditions]
* [RFC 3735: Guidelines for Extending Extensible Provisioning Protocol][RFC3735]
* [RFC 5730: EPP Basic Protocol][RFC5730]
* [RFC 5731: EPP Domain Name Mapping][RFC5731]
* [RFC 5732: EPP Host Mapping][RFC5732]
* [RFC 5733: EPP Contact Mapping][RFC5733]
* [RFC 5910: Domain Name System (DNS) Security Extensions for the Extensible Provisioning Protocol][RFC5910]
* [DK Hostmaster: Current domain registration form][Current domain registration form]
* [DK Hostmaster: Documentation on the current domain registration form][Documentation on the current domain registration form]
* [DK Hostmaster: Pre-activation Service Specification][Pre-activation Service Specification]


* [ICANN: EPP status codes](https://www.icann.org/resources/pages/epp-status-codes-2014-06-16-en/)

* [DK Hostmaster: Name Service Specification][dkhm-name-service-specification]


<a id="resources"></a>
# Resources

A list of resources for DK Hostmaster EPP support is located below.


<a id="xml-schemas"></a>
## XML Schemas

This is a list of the schemas currently used in the DKHM EPP Service described in this document. Please note that the XSD implementation preserves the original namespace and does not make alterations to this apart from adding the already described XML elements.

* epp-1.0.xsd
* eppcom-1.0.xsd
* contact-1.0.xsd
* domain-1.0.xsd
* host-1.0.xsd
* dkhm-2.0.xsd
* secDNS-1.1.xsd

The files are all available for [download][XSD files].


<a id="xsd-version-history"></a>
### XSD Version History

* 2.0
  * EPP Service version 2.0.X, 2.1.X and 2.2.X
  * Introduction of `dkhm:requestedNsAdmin` for update host and create host
  * Introduction of `dkhm:mobilephone` on update contact
  * Introduction of `dkhm:secondaryEmail` on update contact

* 1.4
  * EPP Service version 1.3.X
  * Introduction of `dkhm:pnumber` for production unit number information for create contact

* 1.3
  * EPP Service version 1.2.X
  * Introduction of `dkhm:domain_confirmed` for information for create domain
  * Introduction of `dkhm:contact_validated` for information for info contact
  * Introduction of `dkhm:registrant_validated` for information for create domain

* 1.2
  * EPP Service version 1.1.X
  * Introduction of `dkhm:orderConfirmation` for create domain and support of [Pre-activation Service](#pre-activation-service)

* 1.1
  * EPP Service version 1.0.9
  * Introduction of `dkhm:domainAdvisory` for support of blocked status for create domain for blocked domain names

* 1.0
  * EPP Service version 1.0.0
  * Released 2014-02-25


<a id="mailing-list"></a>
## Mailing list

DK Hostmaster operates a mailing list for discussion and inquiries  about the DK Hostmaster EPP implementation. To subscribe to this list, write to the address below and follow the instructions. Please note that the list is for technical discussion only, any issues beyond the technical scope will not be responded to, please send these to the contact issue reporting address below and they will be passed on to the appropriate entities within DK Hostmaster.

* tech-discuss+subscribe@liste.dk-hostmaster.dk


<a id="issue-reporting"></a>
## Issue Reporting

For issue reporting related to this specification, the EPP implementation or test, sandbox or production environments, please contact us.  You are of course welcome to post these to the mailing list mentioned above, otherwise use the address specified below:

* info@dk-hostmaster.dk


<a id="demotest-client"></a>
## Demo/Test Client

We have developed a demo/test client, which is freely available and open sourced under a MIT license.

The client is available at:

- https://github.com/DK-Hostmaster/epp-demo-client-mojolicious


<a id="additional-information"></a>
## Additional Information

More information is available at the DK Hostmaster website:

* https://www.dk-hostmaster.dk/en/epp


<a id="pre-activation-service"></a>
## Pre-activation Service

More information and documentation on the pre-activation service is available at the DK Hostmaster website:

* https://www.dk-hostmaster.dk/en/pre-act


<a id="appendices"></a>
# Appendices


<a id="greeting"></a>
## Greeting

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
                <extURI>urn:dkhm:params:xml:ns:dkhm-2.0</extURI>
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
## Status Codes


<a id="domain"></a>
### Domain

| Status Code | Description  |
| ------------ | ------------ |
| `addPeriod` | *unsupported* |
| `autoRenewPeriod` | *unspported* |
| `inactive` | *unspported at this time* |
| `ok` | exclusive for all other status codes |
| `pendingCreate` | indication that a the given domain is enqueue for possible creation |
| `pendingDelete` | deletion is pending, an advisory date is applicable |
| `pendingRenew` | *unsupported* |
| `pendingRestore` | *unsupported* |
| `pendingTransfer` | *unsupported* |
| `pendingUpdate` | the domain has active asynchronous requests |
| `redemptionPeriod` | *unsupported* |
| `renewPeriod` | *unsupported* |
| `serverDeleteProhibited` | indicates whether the registrant can delete the domain |
| `serverHold` | a given domain is not active, it can hold a number of different states rendering it not-active |
| `serverRenewProhibited` | indicates whether the billing contact can renew the domain |
| `serverTransferProhibited` | *unsupported* |
| `serverUpdateProhibited` | indicates whether the registrant for a given domain can have ownership transferred, can appoint new proxy/admin contact, can appoint new billing contact, change nameservers and can associate DS Records |
| `transferPeriod` | *unsupported* |
| `clientDeleteProhibited` | *unsupported* |
| `clientHold` | *unsupported* |
| `clientRenewProhibited` | *unsupported* |
| `clientTransferProhibited` | *unsupported* |
| `clientUpdateProhibited` | *unsupported* |


<a id="privilege-matrix"></a>
## Privilege Matrix

| Command | Sub-command | Registrar | Domain admin | Domain billing | Nameserver admin |
| --- | --- |:---:|:---:|:---:|:---:|
| login | | :white_check_mark: | :white_check_mark: \*1 | :white_check_mark: \*1 | :white_check_mark: |
| create domain | | :white_check_mark: | | | |
| update domain | | | :white_check_mark: \*2 | | :white_check_mark: \*2 |
| | add billing | :white_check_mark: \*8 | :white_check_mark: \*3 | | |
| | remove billing | :white_check_mark: \*4 | :white_check_mark: \*4 | :white_check_mark: \*4 | |
| | add admin | | :white_check_mark: \*5 | | |
| | remove admin | | :white_check_mark: \*4 | | |
| | change registrant | | :white_check_mark: \*6 | | |
| | add nameserver | | :white_check_mark: \*6 | | :white_check_mark: \*6 |
| | remove nameserver | | :white_check_mark: \*6 | | :white_check_mark: \*6 |
| renew domain | | | | :white_check_mark: | |
| delete domain | | | :white_check_mark: \*6 | | |
| info domain | | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| check domain | | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| create contact | | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| update contact | | :white_check_mark: \*7 | | | :white_check_mark: \*7 |
| delete contact | | | | | |
| info contact | | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| check contact | | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| create host | | :white_check_mark: | | | :white_check_mark: |
| update host | | | | | :white_check_mark: |
| delete host | | | | | :white_check_mark: |
| info host | | :white_check_mark: | | | :white_check_mark: |
| check host | | :white_check_mark: | | | :white_check_mark: |

- \*1 as registrar
- \*2 see sub-commands
- \*3 request to new billing contact
- \*4 defaults to registrant
- \*5 request to to registrant and new admin contact
- \*6 request to registrant
- \*7 only own profile
- \*8 can only assign self


<a id="compatibility-matrix"></a>
## Compatibility Matrix

| EPP Command  | Available since version | Exceptions and notes |
| ------------ | ------------ | ------------ |
| Log in | 1 | |
| Change password | 1 | |
| Log out | 1 | |
| Check Domain | 1 | |
| Create Domain | 1 | Asynchronous, requires orderconfirmation by the registrant. VID product not supported, PO numbers not supported |
| Info Domain | 1 | Billing contact not disclosed, EPP status codes not supported completely |
| Update Domain | 2 | Change of nameserver is asynchronous, requires approval by the registrant. Change of registrant is not supported |
| Renew Domain | 2 | Requires that the requesting user is a registrar and billing contact for the domain. The domain name must nout have any financial outstanding |
| Transfer Domain | N/A | |
| Delete Domain | N/A | |
| Check Contact | 1 | |
| Create Contact | 1 | Supplied handle/user-id is not supported |
| Info Contact | 1 | |
| Update Contact | 2 | Updating email is asynchronous, but is regarded as non-atomic due to the email validation proces |
| Transfer Contact | N/A | |
| Delete Contact | N/A | |
| Check Host | 1 | |
| Create Host | 2 | Asynchronous, requires accept of the registrant of the domain name if the domain is under the .dk TLD and requires that the requesting user accepts the responsibility as nameserver administrator |
| Info Host | 1 | |
| Update Host | 2 |  Asynchronous, requires that the requested administrator accepts the responsibility as nameserver administrator |
| Delete Host | 2 | |
| Poll | 1 | |

[General Terms and Conditions]: https://www.dk-hostmaster.dk/en/general-conditions

[General Terms and Conditions 3_3]: https://www.dk-hostmaster.dk/en/general-conditions#3.3

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

[epp-update-domain]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_domain_v1.1.png

[epp-update-domain-evaluate]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_domain_evaluate_command_v1.0.png

[epp-update-domain-add-contact]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_domain_add_contact_v1.0.png

[dkh-update-domain-add-contact]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/dkh_update_domain_add_contact_v1.0.png

[epp-update-domain-remove-contact]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_domain_remove_contact_v1.1.png

[dkh-update-domain-remove-contact]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/dkh_update_domain_remove_contact_v1.0.png

[epp-update-domain-add-ns]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_domain_add_ns_v1.0.png

[epp-update-domain-remove-ns]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_domain_remove_ns_v1.1.png

[epp-update-domain-change-registrant]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp_update_domain_change_registrant_v1.2.png

[XSD files]: https://github.com/DK-Hostmaster/epp-xsd-files

[RFC3735]: http://tools.ietf.org/html/rfc3735

[RFC5730]: http://tools.ietf.org/html/rfc5730

[RFC5731]: http://tools.ietf.org/html/rfc5731

[RFC5732]: http://tools.ietf.org/html/rfc5732

[RFC5732-3.1.2]: http://tools.ietf.org/html/rfc5732#section-3.1.2

[RFC5733]: http://tools.ietf.org/html/rfc5733

[RFC5910]: http://tools.ietf.org/html/rfc5910

[RFC:7451]: https://tools.ietf.org/html/rfc7451

[IANA EPP Extension Repository]: http://www.iana.org/assignments/epp-extensions/epp-extensions.xhtml

[Pre-activation Service Specification]: https://github.com/DK-Hostmaster/preactivation-service-specification

[EAN description]: https://en.wikipedia.org/wiki/International_Article_Number_(EAN)

[Current domain registration form]: https://raw.githubusercontent.com/DK-Hostmaster/mailform-service-specification/master/5.00/5.00en.txt

[Documentation on the current domain registration form]: https://www.dk-hostmaster.dk/en/mailform-registration

[dkhm-name-service-specification]: https://github.com/DK-Hostmaster/dkhm-name-service-specification
