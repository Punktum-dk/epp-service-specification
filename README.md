DK Hostmaster EPP Service Specification

2015-05-12
Revision: 1.7

# Table of Contents

<!-- MarkdownTOC -->

- Introduction
  - About this Document
  - Document History
- The .dk Registry in Brief
- EPP in Brief
- EPP Service
  - SSL Certificate
  - Available Environments
- Implementation Requirements
  - Client Transaction ID (`clTRID`)
- Implementation Extensions
  - `dkhm:userType`
  - `dkhm:EAN`
  - `dkhm:CVR`
  - `dkhm:pnumber`
  - `dkhm:trackingNo`
  - `dkhm:domainAdvisory`
  - `orderconfirmationToken`
  - `domain_confirmed`
  - `contact_validated`
  - `registrant_validated`
- Implementation Limitations
  - Commands
  - Unimplemented commands
  - Authorization
  - DNSSEC
  - Contact Creation
  - Information Disclosure
  - Encoding and IDN domains
- Supported Object Transform and Query Commands
  - hello and greeting
  - login
  - logout
  - poll and message queue
  - create domain
  - check domain
  - info domain
  - check host
  - info host
  - create contact
  - check contact
  - info contact
- Data Collection Policy
  - Access
  - Purpose Statement
  - Recipient Statement
  - Retention Statement
- References
- Resources
  - XML Schemas
  - Mailing list
  - Issue Reporting
  - Additional Information
  - Pre-activation Service
- Appendices
  - Greeting

<!-- /MarkdownTOC -->


# Introduction

This document describes and specifies the implementation offered by DK Hostmaster for interaction with the central registry for the ccTLD dk using the Extensible Provisioning Protocol (EPP). It is primarily aimed at a technical audience, and the reader is required to have prior knowledge of DNS registration and EPP.

## About this Document

This specification describes version 1 of the DK Hostmaster EPP Implementation. Future releases will be reflected in updates to this specification, please see the document history section below.

The document describes the current DK Hostmaster EPP implementation, for more general documentation on the EPP protocol, EPP client development or configuration, please refer to the RFCs and additional resources in the [References](#references) and [Resources](#resources) chapters below.

Any future extensions and possible additions and changes to the implementation are not within the scope of this document and will not be discussed or mentioned throughout this document.

This document is owned and maintained by DK Hostmaster A/S and must not be distributed without this information.

All examples provided in the document are fabricated or changed from real data to demonstrate commands etc. any resemblence to actual data are coincidental.

Printable version can be obtained via [this link](https://gitprint.com/DK-Hostmaster/epp-service-specification/blob/master/README.md), using the gitprint service.

## Document History

* 1.0 2013-02-25
  * Initial revision
  * Introduces [XSD][XSD Files] specification revision 1.0

* 1.1 2013-05-31
  * Added paragraph on passwords in section on the login command
  * Added mention of standard port 700
  * Corrected some of the XML examples, which had not been updated to reflect the correct use of [XSDs][XSD Files]
  * Added important note on contact creation

* 1.2 2013-08-07
  * This revision of the specification is describing EPP service release 1.0.8
  * Added note on domain check

* 1.3 2013-10-29
  * This revision of the specification is describing EPP service release 1.0.9
  * Added information on use of `clTRID` in context of create domain command
  * Added more information on the domain check command, which has been extended with EPP service release 1.0.9. 
  * This release also updates the [XSD][XSD Files] specification to revision 1.1

* 1.4 2013-11-19
  * Corrected links in resources
  * Empasized the use of the `auto` keyword for contact creation, this has also been listed in the implementation limitations section
  * Added information on the restrictive use of `clTRID` in new section entitled: Implementation Requirements |

* 1.5 2014-06-18
  * This revision of the specification is describing EPP service release 1.1.X
  * The test environment is no longer active
  * Examples updated to latest [XSD][XSD Files] revision (1.2)
  * Pre-activation token (`orderconfirmationToken`) can be transported via extension for create domain command
  * Multiple examples of requests and responses added

* 1.6 2015-01-06
  * This revision of the specification is describing EPP service release 1.2.X
  * This release also updates the [XSD][XSD Files] specification to revision 1.3
  * The document has with this revision been ported from a proprietary format to markdown and is being hosted on github for easier maintenance and distribution, this has resultet in a lot of minor corrections and clarifications.
  * Extended the section about this document, due to the migration to Github, so copyright is now explicitly mentioned
  * info contact command extended with validation information
  * create domain command extended with validation information for registrant
  * create domain command extended with information on confirmation status for domain

* 1.7 2015-05-12
  * This revision of the specification is describing EPP service release 1.3.X
  * This release also updates the [XSD][XSD Files] specification to revision 1.4, introducing the extension pnumber for transport of production unit numbers for validation of danish companies as part of the create contact command

# The .dk Registry in Brief

DK Hostmaster is the registry for the ccTLD for Denmark (dk). The current model used in Denmark is based on a sole registry, with DK Hostmaster maintaining the central DNS registry.

The legislation and registry model utilised in Denmark imposes some limitations compared to the EPP protocol in general, since the primary intent of the EPP protocol is focused on a model based on shared-registry rather than a sole-registry model like the one used in Denmark.

These limitations are described in detail below in the chapter entitled Implementation Limitations, and these are explained further in the command descriptions where the single commands deviate from the EPP standard specification. In addition to limitations and deviations found in the above, a few others have been implemented to support DNS registration under Danish legislation, these are described in detail under the individual commands, where relevant.

# EPP in Brief

EPP is an XML-based protocol aimed at provisioning data between registries. The protocol is intended for machine-to-machine communication in a client-server setup. Please see the References chapter for more information on specifications and references for EPP.

Please note that the service does not support XML entity expansion on the server side, due to security implications related to this feature.

# EPP Service

The DK Hostmaster’s EPP Service is based on an SOA architecture. EPP implementation is regarded as a service offered to external parties requiring provisioning actions towards DK Hostmaster.

The EPP service requires the use of and possible development of EPP client software. This is beyond the scope of this specification as the API and other assets for assisting in this are the primary object of this document.

In addition to the assets, DK Hostmaster aims to assist users and developers of EPP client software with integration towards DK Hostmaster and therefore provide facilities to ease this integration. This is primarily centered around a sandbox environment and related documentation.

In addition, DK Hostmaster provides  a test environment for evaluation of future releases of the service, both for evaluation of new features, but also for opening up for EPP users to assist and guide DK Hostmaster in the EPP service implementation work.

## SSL Certificate

To validate the connection to our EPP service you need to use a [SSL certificate][SSL certificate].

Use of the certificate is recommended and it should be use for all available environments. 

## Available Environments

DK Hostmaster offers the following environments:

* production
  * This environment will be the production environment.
  * info and check requests made to this environment will reflect live production data.
  * create requests made to this environment will be carried out provided that  they comply with business rules and general terms.
  * Approved domains will be processed for possible activation and propagation into the zone
  * Contacts (users) will be created and will be available in other systems like the self-service system etc.
  * Hosts (name servers) will be processed for possible activation.
  * The Change Password operation will be available in this environment. Please note that this operation will change the password and this change will be reflected in other systems.
  * The production environment is available at: epp.dk-hostmaster.dk port 700

* sandbox
  * This environment is intended for client development towards the DK Hostmaster EPP service.
  * info and check requests made to this environment will reflect sandbox data. For host objects, static content synched in by DK Hostmaster.
  * create requests made to this environment will be serialised in the sandbox environment, provided that syntax and data are valid.
  * Domains will be enqueued, but will not be processed further nor be available for activation and propagation into the zone
  * Contacts (users) will be created, but will not be available in other systems like the self-service system etc.
  * The Change Password operation will only change the password on the sandbox environment. 
  * The sandbox environment is available at: epp-sandbox.dk-hostmaster.dk port 700

# Implementation Requirements

This section outlines the overall requirements in regard to implementing an EPP client to work with the DK Hostmaster EPP service.

## Client Transaction ID (`clTRID`)

In order to ensure transactional integrity and due to the asynchronous nature of some of the EPP commands, we rely on the client transaction id to be unique. This is unique as per client id. The assists in ensuring that a delayed response can be easily identified by simple means.

The `clTRID` is recommended to be unique for all transactions and is required to be unique for the create domain command. This might change in the future.

# Implementation Extensions

The EPP service implemented by DK Hostmaster holds several extensions, these are documented where appropriate for the specific commands etc. This section serves to give an overview of the extensions as a whole.

Here follows a listed, the extensions are described separately and in detail below.

* dkhm:userType
* dkhm:EAN
* dkhm:CVR
* dkhm:pnumber
* dkhm:trackingNo
* dkhm:domainAdvisory
* orderconfirmationToken
* domain_confirmed
* contact_validated
* registrant_validated

## `dkhm:userType`

The userType extension is used to categorize a contact type, since the requirements for required data differs between the different types, more information is available under the create contact command.

Related extensions are `dkhm::EAN`, `dkhm:CVR` and `dkhm:pnumber`.

## `dkhm:EAN`

The EAN extension, hold the EAN number associated with public organisations in Denmark. The field is mandatory for this
type of contact objects and is required for electronic invoicing, more information is available under the create contact command.

## `dkhm:CVR`

The CVR extension is for holding VAT registration numbers. The number is used for validation and VAT accounting.

## `dkhm:pnumber`

The pnumber extension is for holding production-unit numbers, used for validation for danish companies, with more physical addressed related to one VAT number.

## `dkhm:trackingNo`

A unique tracking number for a domain registration for uniformity with the mail form.

## `dkhm:domainAdvisory`

Domain names registered with DK Hostmaster can hold a status blocked. This is used for communicating this special status for the check domain command.

## `orderconfirmationToken`

This is a special field for supporting a business flow where a domain can be pre-activated using the DK Hostmaster Pre-activation service.

## `domain_confirmed`

Domain names registered with DK Hostmaster, has to be confirmed by the registrant, this is can either be done using pre-activation, see the `orderconfirmationToken` above or other systems with DK Hostmaster, the domain confirmation state is available via the info domain command using this extension.

See also `orderconfirmationToken`

## `contact_validated`

Contact objects related to the role of registrant has to be validated, this field is used to indicate the status of a validation object via the info contact command.

## `registrant_validated`

As described above, contact objects related to the role of registrant has to be validated, this field is used to indicate the status of a validation object via the info domain command.

# Implementation Limitations

As mentioned previously the EPP service comes with some limitations.

## Commands

The current implementation is limited to the following list of commands:

* hello
* login, including change password
* logout
* poll, including acknowledgement of messages
* info (contact/domain/host)
* check (contact/domain/host)
* create (contact/domain)

All commands will be described in detail below.

## Unimplemented commands

The following commands have not been implemented in the service described in this specification:

* update (contact/domain/host)
* delete (contact/domain/host)
* transfer (contact/domain)
* renew
* create host

The above commands was pulled out of scope, because the overall and primary goal of version 1, is to implement a standardised replacement for the existing [SMTP based form][Current domain registration form].

In general the service is not localized and all EPP related errors and messages are provided in English. 

## Authorization

More specifically, the service does not support the following features of the EPP protocol:

* Authorization

Comparing the EPP implementation to the existing channel for domain registration using the form via SMTP, the following fields are not supported.

* VID (VIP domain name)
* Billing contact's purchase order (PO) number

## DNSSEC

I accordance with [RFC 5910][RFC5910]. We support DS only and not DNSKEY. In addition the maximum signature lifetime (`secDNS:maxSigLife`) is disregarded. See [section 3.3](http://tools.ietf.org/html/rfc5910#section-3.3) in the referenced RFC.

## Contact Creation

This command does not support the feature of providing own userid. The userid has to be specified as `auto` and the userid is assigned by DK Hostmaster. See also information on the create contact command.

## Information Disclosure

Please note that some information is not disclosed when using Object Query Commands. See the specific commands for more information.

## Encoding and IDN domains

The danish registry supports IDN domain names and the EPP commands support punycode notation for this in requests. We do however not support punycode notation in responses at this time.

# Supported Object Transform and Query Commands

The following describes the currently supported EPP commands. As mentioned previously, some of the commands have been extended beyond the basic capabilities of EPP. These minor extensions are described separately under each command and are included in the [XSD files][XSD Files] listed in the Resources chapter.

Commands that have not been extended are not described in much detail, please refer to the general EPP documentation from IETF (see: the RFCs listed in References).

## hello and greeting

This part of the EPP protocol is described in [RFC 5730][RFC5730]. This command adheres to the standard.
For a more detailed explanation of the data collection policy announced via the greeting, please see the Data Collection Policy chapter.

As announced in the greeting, the following objects are available:

* Host
* Domain
* Contact

With regard to extensions, the following are available:

* [secDNS-1.1][XSD Files]
* [dkhm-1.3][XSD Files]

Please see the greeting response included in the [appendices](greeting) for illustration of the actual announcement.

## login

This part of the EPP protocol is described in [RFC 5730][RFC5730]. This command adheres to the standard.

The login uses the general AAA functionality in DK Hostmaster. This mean that in addition to the validation of username and password specified as part of the login request, an attempt is made to authorise the authenticated user for access to the actual EPP service and subsequent operations.

Authorisation is currently only available to active registrars, therefore the username provided must point to an entity with the role of registrar with the DK Hostmaster registry. 

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

### login request:

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

### login reponse:

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

## logout

This part of the EPP protocol is described in [RFC 5730][RFC5730]. This command adheres to the standard.

There are no special additions or alterations to the specification or use of this command.

### logout request:

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <command>
    <logout />
    <clTRID>9450488c8280671c051f273285d7bec7</clTRID>
  </command>
</epp>
```

### logout response:

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

## poll and message queue

This part of the EPP protocol is described in [RFC 5730][RFC5730]. This command adheres to the standard.

There are no special additions or alterations to the specification or use of this command.

For clarification 2303 is returned in case a provided message-id (`msgID`) point to a non-existing message.

## create domain

This part of the EPP protocol is described in [RFC 5730][RFC5730]. This command adheres to the standard. DK Hostmaster, however, is based on an asynchronous domain creation workflow. All domain requests are enqueued for further processing and their creation will be in a state of pending.

A well-formed request for domain creation will then always result in:

```
1001, “Commmand completed successfully; action pending”
```

The extension in response will provide a unique tracking number, which can be used to identify the creation request across provisioning channels offered by DK Hostmaster. The result of the further processing will be relayed back via EPP, see poll and message queue above.

So the customised response for a domain creation request looks as below.

The create domain command has also been extended so it is possible to assign a pre-activation token to the request using the `orderconfirmationToken`.

```XML
<dkhm:orderconfirmationToken xmlns:dkhm=“urn:dkhm:params:xml:ns:dkhm-1.2”>
	testtoken
</dkhm:orderconfirmationToken>
```

The token is only validated from an XML perspective by the EPP service if not present is is simply ignored and registration carries on as it was not there. It’s validity is not validated until later in the process and possible interaction with the registrant is required.

The `orderconfirmationToken` can be obtained via the [Pre-activation Service](#pre-activation-service), please see references and resources below.

In addition a create domain contains information on whether the domain has been confirmed, this is communicated via the extension: `dkhm:domain_confirmed`. This indicated is based on whether the provided `orderconfirmationToken` is valid.

The requirement for the registrant to be valid is also communicated via the response, using the extension:
`dkhm:registrant_validated`. Please see the command info contact for more information. The state is communicated in this response in order to provide information on the further flow and process of the create domain request.

### create domain request:

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

### create domain response:

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
As for the user entities some mappings are made so all relevant roles are specified.

| EPP | DKHM | Fallback | Note |
| ------------- | ----------- | ----------- | ----------- |
| admin | administrator (fuldmægtig) | registrant | optional, will use fallback |
| billing | billing (betaler) | registrant | optional, will use fallback |
| tech | keyholder (nøgleansvarlig) | | optional, will be ignored if keyholder is specified |
| registrant | registrant | mandatory | |
| registrar | registrar | mandatory | |

Please note that the command supports punycode notation for specifying IDN domain names, but responses are in the specified UTF-8 character set.

![Diagram of role mapping for EPP create domain][epp-role-mapping]

## check domain

Since DK Hostmaster does support a concept of blocked domains. A domain name will be indicated as available if the domain name has the status of blocked. For an explanation of the process please see section 3.3 and in particular section 3.3.2 in the [General Terms and Conditions][General Terms and Conditions].

### check domain request:

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

### check domain response:

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

We have extended the result for the check domain command to reflect this using an extension named `domainAdvisory`. An example response on a blocked domain would look at follows.

```XML
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
    <response>
        <result code="1000">
            <msg>Check result</msg>
        </result>
        <resData>
            <domain:chkData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
                <domain:cd>
                    <domain:name avail="1">blockeddomain.dk</domain:name>
                </domain:cd>
            </domain:chkData>
        </resData>
        <extension>
            <dkhm:domainAdvisory xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-1.2" domain="blockeddomain.dk" advisory="Blocked" />
        </extension>
        <trID>
            <clTRID>2734c8487b96dd80a973edbb3cd73b77</clTRID>
            <svTRID>3475FA4C-4099-11E3-BE43-654514178544</svTRID>
        </trID>
    </response>
</epp>
```

In general this part of the EPP protocol is described in [RFC 5731][RFC5731] and his command adheres to the standard.

## info domain

This part of the EPP protocol is described in [RFC 5731][RFC5731]. This command adheres to the standard.

### info domain request:

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <command>
    <info>
      <domain:info xmlns:domain="urn:ietf:params:xml:ns:domain-1.0" xsi:schemaLocation="urn:ietf:params:xml:ns:domain-1.0 domain-1.0.xsd">
        <domain:name>dk-hostmaster.dk</domain:name>
      </domain:info>
    </info>
    <clTRID>b3350ae28dc86f30feb8d789f795ff67</clTRID>
  </command>
</epp>
```

### info domain response:

```XML
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
  <response>
    <result code="1000">
      <msg>Info result</msg>
    </result>
    <msgQ count="12" id="242">    </msgQ>
    <resData>
      <domain:infData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
        <domain:name>dk-hostmaster.dk</domain:name>
        <domain:roid>DK_HOSTMASTER_DK-DK</domain:roid>
        <domain:status s="serverTransferProhibited" />
        <domain:status s="serverUpdateProhibited" />
        <domain:status s="serverRenewProhibited" />
        <domain:status s="serverDeleteProhibited" />
        <domain:registrant>DKHM1-DK</domain:registrant>
        <domain:contact type="admin">DKHM1-DK</domain:contact>
        <domain:ns>
          <domain:hostObj>auth01.ns.dk-hostmaster.dk</domain:hostObj>
          <domain:hostObj>auth02.ns.dk-hostmaster.dk</domain:hostObj>
          <domain:hostObj>authns1.ngdc.net</domain:hostObj>
        </domain:ns>
        <domain:host>auth01.ns.dk-hostmaster.dk</domain:host>
        <domain:host>auth02.ns.dk-hostmaster.dk</domain:host>
        <domain:host>blocked1.ns.dk-hostmaster.dk</domain:host>
        <domain:host>blocked2.ns.dk-hostmaster.dk</domain:host>
        <domain:host>gr1.dk-hostmaster.dk</domain:host>
        <domain:host>gr2.dk-hostmaster.dk</domain:host>
        <domain:host>missing1nameserver.dk-hostmaster.dk</domain:host>
        <domain:host>missing2nameservers.dk-hostmaster.dk</domain:host>
        <domain:host>ns.dk-hostmaster.dk</domain:host>
        <domain:host>ns1.dk-hostmaster.dk</domain:host>
        <domain:host>ns2.dk-hostmaster.dk</domain:host>
        <domain:host>parat1.dk-hostmaster.dk</domain:host>
        <domain:host>parat2.dk-hostmaster.dk</domain:host>
        <domain:host>venteliste1.dk-hostmaster.dk</domain:host>
        <domain:host>venteliste2.dk-hostmaster.dk</domain:host>
        <domain:clID>DKHM1-DK</domain:clID>
        <domain:crID>n/a</domain:crID>
        <domain:crDate>1998-01-19T00:00:00.0Z</domain:crDate>
      </domain:infData>
    </resData>
    <trID>
      <clTRID>df49a47a9d1058186b97e8b916f0c23f</clTRID>
      <svTRID>40E74ED0-9BE6-11E4-8B24-9C0CC33995C9</svTRID>
    </trID>
  </response>
</epp>
```

## check host

This part of the EPP protocol is described in [RFC 5732][RFC5732]. This command adheres to the standard.

### check host request:

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

### check host response:

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

## info host

This part of the EPP protocol is described in [RFC 5732][RFC5732]. This command adheres to the standard.

Please note that according to the RFC [section 3.1.2][RFC5732-3.1.2], the `CLID` points to the sponsoring client. DK Hostmaster interprets this as the tehnical contact for the nameserver pointing to the host object in question.

### info host request:

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

### info host response:

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

## create contact

This part of the EPP protocol is described in [RFC 5733][RFC5733].

This command has been extended with the following fields:

* User type, which has to be one of:
  * `company` - indicating a company
  * `public_organization` - indicating a public organisation
  * `association` - indicating an association
  * `individual` - indicating an individual

The user type will result in context-specific interpretation of the following fields:

* EAN - this number is only supported for user types: `company`, `public_organization` and `association`. It is only mandatory for `public_organization` and optional for `company` and `association`. [EAN][EAN description] is used by the public sector in Denmark for electronic invoicing, private companies can also be assigned EAN, but this it not so widespread at this time. EAN is required by law for public sector organisations, so this field has to be completed and it has to validate for this type.
* CVR - (VAT number) this is only supported for user types: `company`, `public_organization` and `association`. The number is required for handling VAT correctly, mandatory for user types `company` and `public_organization` and optional for the user type `association`.
* pnumber - (production unit number) this is only supported for user types: `company`, `public_organization` and `association`. The number is used for handling validation correctly and the field is optional.

The contact-id field is auto-generated and assigned by DK Hostmaster. EPP do however open for providing a contact-id in the context of the create contact command, this is not supported by DK Hostmaster at this point.

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

### create contact request:

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
        <contact:email>tech@dk-hostmaster.dk</contact:email>
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

## check contact

This part of the EPP protocol is described in [RFC 5733][RFC5733]. This command adheres to the standard.

### check contact request:

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

### check contact response:

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

## info contact

This part of the EPP protocol is described in [RFC 5733][RFC5733]. This command has been extended with information on whether the contact in queried has been validated according to requirements and policies with DK Hostmaster.

See the extension: `dkhm:contact_validated` in the response.

Please note that the email address (`contact:email`) is masked and the value: `anonymous@dk-hostmaster.dk` is always return for this field.

### info contact request:

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

### info contact response:

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

# Data Collection Policy

This chapter describes the data collection policy announced via the greeting available using the hello command.

Please refer to the [greeting response example](#greeting) included in the [Appendices](#Appendices).

## Access

The EPP service provides access to identified data relating to all available entities (personal and organisational) under the terms and conditions that anonymity will be applied as specified by the entities in question, and in accordance with general terms and conditions and legislation. 

## Purpose Statement

The collected data will be used solely for provisioning and administrative purposes. As specified under access above, and in the recipient statement below, some data are required to be publicly available and therefore some data will be accessible to the public under the circumstances specified in the referred sections.

Address data and contact information is collected as required by danish legislation.

## Recipient Statement

Recipients of data are specified as other and unrelated. As specified in the purpose statement section and under access, identified data is made publicly available, therefore DK Hostmaster will not be able to control how the publicly available information is used.

## Retention Statement

Data will be retained with DK Hostmaster as required by Danish legislation.

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

# Resources

A list of resources for DK Hostmaster EPP support is found below.

## XML Schemas

This is a list of the schemas currently used in the DKHM EPP Service described in this document. Please note that the XSD implementation preserves the original namespace and does not make alterations to this apart from adding the already described XML elements.

* epp-1.0.xsd
* eppcom-1.0.xsd
* contact-1.0.xsd
* domain-1.0.xsd
* host-1.0.xsd
* dkhm-1.4.xsd
* secDNS-1.1.xsd

The files are all available for [download][XSD files].

### XSD Version History

* 1.0
  * EPP Service version 1.0.0
  * Released 2014-02-25 

* 1.1
  * EPP Service version 1.0.9
  * Introduction of `dkhm:domainAdvisory` for support of blocked status for create domain for blocked domain names

* 1.2
  * EPP Service version 1.1.X
  * Introduction of `dkhm:orderConfirmation` for create domain and support of [Pre-activation Service](#pre-activation-service)

* 1.3
  * EPP Service version 1.2.X
  * Introduction of `dkhm:domain_confirmed` for information for create domain
  * Introduction of `dkhm:contact_validated` for information for info contact
  * Introduction of `dkhm:registrant_validated` for information for create domain

* 1.4
  * EPP Service version 1.3.X
  * Introduction of `dkhm:pnumber` for production unit number information for create contact

## Mailing list

DK Hostmaster operates a mailing list for discussion and inquiries  about the DK Hostmaster EPP implementation. To subscribe to this list, write to the address below and follow the instructions. Please note that the list is for technical discussion only, any issues beyond the technical scope will not be responded to, please send these to the contact issue reporting address below and they will be passed on to the appropriate entities within DK Hostmaster.

* epp-discuss+subscribe@liste.dk-hostmaster.dk

## Issue Reporting

For issue reporting related to this specification, the EPP implementation or test, sandbox or production environments, please contact us.  You are of course welcome to post these to the mailing list mentioned above, otherwise use the address specified below:

* tech@dk-hostmaster.dk

## Additional Information

More information is available at the DK Hostmaster website:

* https://www.dk-hostmaster.dk/english/tech-notes/epp/

## Pre-activation Service

More information and documentation on the pre-activation service is available at the DK Hostmaster website:

* https://www.dk-hostmaster.dk/english/technical-administration/tech-notes/pre-activation/

# Appendices

## Greeting

```XML
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">
    <greeting>
        <svID>DK Hostmaster EPP Service (unittest): 0.0.4</svID>
        <svDate>2012-11-02T08:50:49.0Z</svDate>
        <svcMenu>
            <version>1.0</version>
            <lang>en</lang>
            <objURI>urn:ietf:params:xml:ns:host-1.0</objURI>
            <objURI>urn:ietf:params:xml:ns:domain-1.0</objURI>
            <objURI>urn:ietf:params:xml:ns:contact-1.0</objURI>
            <svcExtension>
                <extURI>urn:ietf:params:xml:ns:secDNS-1.1</extURI>
                <extURI>urn:dkhm:params:xml:ns:dkhm-1.0</extURI>
            </svcExtension>
        </svcMenu>
        <dcp>
            <access>
                <personalAndOther />
            </access>
            <statement>
                <purpose>
                    <admin />
                    <prov />
                </purpose>
                <recipient>
                    <other />
                    <unrelated />
                </recipient>
                <retention>
                    <legal />
                </retention>
            </statement>
        </dcp>
    </greeting>
</epp>
```

[General Terms and Conditions]: https://www.dk-hostmaster.dk/fileadmin/filer/pdf/generelle_vilkaar/general-conditions.pdf

[epp-role-mapping]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp-role-resolution.png

[epp-address-resolution]: https://raw.githubusercontent.com/DK-Hostmaster/epp-service-specification/master/images/epp-address-resolution.png

[XSD files]: https://github.com/DK-Hostmaster/epp-xsd-files

[SSL certificate]: https://www.dk-hostmaster.dk/fileadmin/filer/epp/epp.dk-hostmaster.dk_700.pem

[RFC3735]: http://tools.ietf.org/html/rfc3735

[RFC5730]: http://tools.ietf.org/html/rfc5730

[RFC5731]: http://tools.ietf.org/html/rfc5731

[RFC5732]: http://tools.ietf.org/html/rfc5732

[RFC5732-3.1.2]: http://tools.ietf.org/html/rfc5732#section-3.1.2

[RFC5733]: http://tools.ietf.org/html/rfc5733

[RFC5910]: http://tools.ietf.org/html/rfc5910

[Pre-activation Service Specification]: https://github.com/DK-Hostmaster/preactivation-service-specification

[EAN description]: https://en.wikipedia.org/wiki/International_Article_Number_(EAN)

[Current domain registration form]: https://www.dk-hostmaster.dk/fileadmin/formularer/dk-5.00en.txt

[Documentation on the current domain registration form]: https://www.dk-hostmaster.dk/english/technical-administration/forms/register-domainname/
