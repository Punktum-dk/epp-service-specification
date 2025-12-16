# NIS2 EPP documentation - EPP 5.2.0 -en

This document describes the new EPP extensions being introduced as part of the NIS2 updates.

Version EPP 5.2.0 will be deployed to production on January 14, 2026.

The document covers the following extensions:

- dkhm:sole_proprietorship
- dkhm:contact_verification
  - dkhm:responsible
  - dkhm:verified_id
  - dkhm:verified_email
  - dkhm:confirm_email
  - dkhm:confirm_secondary_email

These extensions implement the requirements set out by the NIS2 legislation. They allow registrars to:

- perform ID verification,
- validate the contact's email address, and
- indicate whether a company is a sole proprietorship.

Relevant extensions in the examples provided in the appendices are highlighted in green.

Table of Contents

[NIS2 EPP documentation - EPP 5.2.0 -en 1](#_Toc213752210)

[dkhm:sole_proprietorship 2](#_Toc213752211)

[dkhm:sole_proprietorship - Info contact: 2](#_Toc213752212)

[dkhm:sole_proprietorship - Create contact and update contact 2](#_Toc213752213)

[dkhm:contact_verification 3](#_Toc213752214)

[dkhm:contact_verification / dkhm:responsible 3](#_Toc213752215)

[dkhm:contact_verification / dkhm:verified_id 4](#_Toc213752216)

[dkhm:contact_verification / dkhm:verified_email 4](#_Toc213752217)

[Practical Information 5](#_Toc213752218)

[Poll messages 6](#_Toc213752219)

[Appendix for dkhm:sole_proprietorship 7](#_Toc213752220)

[Appendix 1: info contact 7](#_Toc213752221)

[Appendix 2: contact create 8](#_Toc213752222)

[Appendix for dkhm:contact_verification 9](#_Toc213752223)

[Appendix 3: info contact dkhm:responsible 9](#_Toc213752224)

[Appendix 4: contact create dkhm:verified_id and dkhm:verified_email 10](#_Toc213752225)

[Appendix 5: contact info dkhm:verified_id and dkhm:verified_email 11](#_Toc213752226)

# dkhm:sole_proprietorship

In accordance with the NIS2 requirement to publish email addresses in WHOIS for business registrations, it must be possible to indicate whether a company is a **sole proprietorship**.

This is necessary because a sole proprietorship is, legally speaking, **the same entity as its owner (a private individual)**.

Therefore, email addresses belonging to sole proprietorships must **not be published** in WHOIS.

This extension is **only relevant for foreign companies**, as Danish companies are automatically validated via the **CVR register**.

For Danish contacts, the CVR number is locked, meaning the information cannot be modified manually.

The extension is included in the following EPP commands:

- contact:create - used to specify whether the company is a sole proprietorship.
- contact:update - used to change this information, if relevant.
- contact:info - the value is displayed here as part of the contact information.

The field can have two values:

- true - indicates that the contact is a sole proprietorship
- false - indicates that the contact is _not_ a sole proprietorship

If dkhm:sole_proprietorship is **omitted** in a contact:create command, the value is automatically assumed to be "_false_."

## dkhm:sole_proprietorship - Info contact

You can check whether the contact is **specified as a sole proprietorship** by performing a contact:info command.

See the example in [Appendix 1](#_Appendix_1:_info), where the contact is indicated as a sole proprietorship in the Contact info response.

## dkhm:sole_proprietorship - Create contact and update contact

In contact:create and contact:update, you can specify whether a foreign company is a sole proprietorship.

If the dkhm:sole_proprietorship extension is provided for a contact with the **country code DK**, the system will return an **error**.

See [Appendix 2](#_Appendix_2:_contact) for an example of a contact:create request where dkhm:sole_proprietorship is set to "_true_."

# dkhm:contact_verification

This extension is used to indicate which contact information **the registrar has validated**, as well as to show **who performed the validation** and the **current status** of the contact's verification.

dkhm:contact_verification consists of the following sub-extensions:

- dkhm:contact_verification / responsible
- dkhm:contact_verification / verified_id
- dkhm:contact_verification / verified_email
- dkhm:confirm_email
- dkhm:confirm_secondary_email

In a **contact:update**, the _contact_verification_ field cannot be used when updating the email or address on a contact.

The following default logic applies when a contacts email or address is updated:

- When **responsible = registrar**, any changes to the email address or physical address will automatically set the value to "_true_".
- When **responsible = registry**, the same changes will automatically set the value to "_false_".

This logic is based on the assumption that when _registrar_ is set as _responsible_, the registrar has already verified the contact information before submitting the update to us.

When **responsible = registry**, the registrar is not able to perform their own verification during a **contact:update**. In these cases, the update will be processed through our risk engine, which may trigger validation of name and address, as well as email validation if the email address is changed.

## dkhm:contact_verification / dkhm:responsible

dkhm:contact_verification/dkhm:responsible specifies **who is responsible for validating the customer**.

This is a **visual sub-extension**, **displayed only** in a contact:info response.

dkhm:responsible can have the following values:

- registrar - indicates that **the registrar** is responsible for validating the contact.
- registry - indicates that **Punktum dk** is responsible for validating the contact.

The value can only be set to "_registrar_" if the contact is **registrar-managed** and you have specified in the registrar portal that **you**, **as the registrar**, are **responsible for ID validation**.

Contacts that are registrant-managed will always have the value set to "_registry_."

This sub-extension can be viewed by performing a contact:info request.

An example where "_registry_" is specified as the responsible party can be found in [Appendix 3](#_Appendix_3:_info), in the form of a contact:info response.

However, **you have the option to validate a contact in advance** if you have already completed the verification before creating it - **regardless** of whether the registry (Punktum dk) is listed as responsible.

If you set dkhm:verified_id and dkhm:verified_email to "_true_" in a contact:create command, **Punktum dk will not perform additional ID or email validation**, even if Punktum dk was originally listed as **responsible** for the verification.

## dkhm:contact_verification / dkhm:verified_id

This sub-extension is used to indicate whether the contact's name and address have been validated.

dkhm:verified_id can have two values:

- true - indicates that the contact's name and address have been validated.
- false - indicates that the contact's name and address have _not_ been validated.

These values can be provided when creating or updating a contact, or they may be omitted, in which case the value will default to "_false_."

See [Appendix 4](#_Appendix_4:_contact) for an example of a contact:create request where dkhm:verified_id is set to "_true_."

When performing a contact:info command, a status for dkhm:verified_id is also displayed.

The status field can show the following values:

- notRequired - indicates that the contact does not require validation.
- pending - indicates that the contact has an ongoing ID verification.
- expired - indicates that the ID verification was not completed within the 25-day deadline.
- completed - indicates that the contact has successfully completed ID verification.

There may also be an additional field:

- expdate - shown only when the contact's verified_id status is pending; it specifies when the ID verification expires.

If **verified_id** ends in an "_expired_" status, the domain name will be suspended.

See [Appendix 5](#_Appendix_5:_contact) for an example of a contact:info response where the ID validation status is shown as pending.

## dkhm:contact_verification / dkhm:verified_email

This sub-extension is used to indicate whether the contact's email address has been validated.

dkhm:verified_email can have two values:

- true - indicates that the contact's email address has been validated.
- false - indicates that the contact's email address has not been validated.

As with verified_id, these values must be provided when creating or updating a contact, or they may be omitted, in which case the value will default to "_false_."

See [Appendix 4](#_Appendix_4:_contact) for an example of a contact:create request where dkhm:verified_email is set to "_true_."

When performing a contact:info command, a status for dkhm:verified_email is also displayed.

The status field can show the following values:

- notRequired - indicates that the contact does not require validation.
- pending - indicates that the contact has an ongoing email verification.
- expired - indicates that the email verification was not completed within the 25-day deadline.
- completed - indicates that the contact has successfully completed email verification.

There may also be an additional field:

- expdate - shown only when the contact's verified_email status is pending; it specifies when the email verification expires.

See [Appendix 5](#_Appendix_5:_contact) for an example of a contact:info response where the email verification status is shown as pending.

A domain name will only be suspended if a request to validate the primary email in a **contact:create** operation ends in an **"expired"** status, or if an existing customer has never previously validated their email and an email validation is initiated. This type of request has a lifetime of **30 days**.

If the email address is not validated during a **contact:update**, the email change will simply not be applied. In this scenario, the system will keep the previously validated email address. This type of request does **not** affect the domain's status and will never lead to suspension.  
The validation request has a lifetime of **48 hours**.

Status for primary and/or secondary email updates can be found via two additional sub-extensions:

- **dkhm:confirm_email** - displays the status for the primary email
- **dkhm:confirm_secondary_email** - displays the status for the secondary email

These extensions only reflect the status of a pending email-change request and have no impact on the domain status (the domain will not be suspended if the request is not completed).

The extensions are only included in a **contact:info** response when there is an active email-change request.

They contain:

- **expdate**, indicating when the request expires
- **status**, which will always appear as "_pending_" because the extensions are only shown when a request is active
- The **email address** is associated with the request

An example of _confirm_email_ and _confirm_secondary_email_ in a **contact:info** response can be found in [_Appendix 6_](#_Appendix_6:_contact:info).

Both the primary and secondary email addresses must be validated. If no secondary email address is provided, only the primary email address requires validation.

# Practical Information

In this section, we review the practical use of the described extensions.

It is not possible to specify different values for dkhm:verified_id and dkhm:verified_email when creating a contact.

See the example below:

&lt;dkhm:contact_verification xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"&gt;

&lt;dkhm:verified_id&gt;**false**&lt;/dkhm:verified_id&gt;

&lt;dkhm:verified_email&gt;**true**&lt;/dkhm:verified_email&gt;

&lt;/dkhm:contact_verification&gt;

This will result in the error code 2306:

msg: Same value required for both verified_id and verified_email.

If you, as **the registrar**, are **responsible for validating** a contact, and it ends up with the status "_expired_" for either dkhm:verified_email or dkhm:verified_id, the associated domain name **will be suspended** for a period of 30 days.

However, you still have the option to update the expired value to "true", after which the domain name will be automatically **reactivated,** and the ID verification status will be updated to "_completed_."

The deadline for completing ID or email verification is 25 days from the date the process was initiated.

You do **not** have the option to change the values of dkhm:verified_id or dkhm:verified_email if Punktum dk has an **ongoing ID verification for the contact**.

If you indicate that a **Danish customer** has been validated, but we **cannot lock the contact to the CVR/CPR** **register**, Punktum dk will be required to perform the ID verification.

This is because we need information from the **CVR/CPR register** to determine whether a customer has **name and address protection** or is a **sole proprietorship**. At the same time, we use the register to automatically **update the address and name** when these change in the CPR or CVR register.

# Appendix for dkhm:sole_proprietorship

## Appendix 1: info contact

&lt;?xml version="1.0" encoding="UTF-8" standalone="no"?&gt;

&lt;epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd"&gt;

&nbsp;   &lt;response&gt;

&nbsp;       &lt;result code="1000"&gt;

&nbsp;           &lt;msg&gt;Command completed successfully&lt;/msg&gt;

&nbsp;       &lt;/result&gt;

&nbsp;       &lt;msgQ count="4" id="2618875"&gt;    &lt;/msgQ&gt;

&nbsp;       &lt;resData&gt;

&nbsp;           &lt;contact:infData xmlns:contact="urn:ietf:params:xml:ns:contact-1.0"&gt;

&nbsp;               &lt;contact:id&gt;TA20593-DK&lt;/contact:id&gt;

&nbsp;               &lt;contact:roid&gt;TA20593-DK&lt;/contact:roid&gt;

&nbsp;               &lt;contact:status s="serverDeleteProhibited"/&gt;

&nbsp;               &lt;contact:status s="serverTransferProhibited"/&gt;

&nbsp;               &lt;contact:postalInfo type="loc"&gt;

&nbsp;                   &lt;contact:name&gt;Test A/S&lt;/contact:name&gt;

&nbsp;                   &lt;contact:addr&gt;

&nbsp;                       &lt;contact:street&gt;Postbox 123&lt;/contact:street&gt;

&nbsp;                       &lt;contact:city&gt;Paris&lt;/contact:city&gt;

&nbsp;                       &lt;contact:pc&gt;14588&lt;/contact:pc&gt;

&nbsp;                       &lt;contact:cc&gt;FR&lt;/contact:cc&gt;

&nbsp;                   &lt;/contact:addr&gt;

&nbsp;               &lt;/contact:postalInfo&gt;

&nbsp;               &lt;contact:voice&gt;+33.330325458877&lt;/contact:voice&gt;

&nbsp;               &lt;contact:email&gt;<vitester@dk-hostmaster.dk>&lt;/contact:email&gt;

&nbsp;               &lt;contact:clID&gt;REG-666666&lt;/contact:clID&gt;

&nbsp;               &lt;contact:crID&gt;REG-666666&lt;/contact:crID&gt;

&nbsp;               &lt;contact:crDate&gt;2025-11-05T08:52:06.0Z&lt;/contact:crDate&gt;

&nbsp;           &lt;/contact:infData&gt;

&nbsp;       &lt;/resData&gt;

&nbsp;       &lt;extension&gt;

&nbsp;           &lt;dkhm:contact_validated xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'&gt;1&lt;/dkhm:contact_validated&gt;

&nbsp;           &lt;dkhm:CVR xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'&gt;1414441&lt;/dkhm:CVR&gt;

&nbsp;           &lt;dkhm:userType xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'&gt;association&lt;/dkhm:userType&gt;

&nbsp;           &lt;dkhm:sole_proprietorship xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'&gt;true&lt;/dkhm:sole_proprietorship&gt;

&nbsp;       &lt;/extension&gt;

## Appendix 2: contact create

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;epp xmlns="urn:ietf:params:xml:ns:epp-1.0"&gt;

&lt;command&gt;

&lt;create&gt;

&lt;contact:create xmlns:contact="urn:ietf:params:xml:ns:contact-1.0"&gt;

&lt;contact:id&gt;force&lt;/contact:id&gt;

&lt;contact:postalInfo type="loc"&gt;

&lt;contact:name&gt;Test A/S&lt;/contact:name&gt;

&lt;contact:addr&gt;

&lt;contact:street&gt;Postbox 123&lt;/contact:street&gt;

&lt;contact:city&gt;Paris&lt;/contact:city&gt;

&lt;contact:sp/&gt;

&lt;contact:pc&gt;14588&lt;/contact:pc&gt;

&lt;contact:cc&gt;FR&lt;/contact:cc&gt;

&lt;/contact:addr&gt;

&lt;/contact:postalInfo&gt;

&lt;contact:voice&gt;+33.0325458877&lt;/contact:voice&gt;

&lt;contact:email&gt;<vitester@dk-hostmaster.dk>&lt;/contact:email&gt;

&lt;contact:authInfo&gt;

&lt;contact:pw/&gt;

&lt;/contact:authInfo&gt;

&lt;/contact:create&gt;

&lt;/create&gt;

&lt;extension&gt;

&lt;dkhm:userType xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"&gt;association&lt;/dkhm:userType&gt;

&lt;dkhm:CVR xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"&gt;1414441&lt;/dkhm:CVR&gt;

&lt;dkhm:management xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"&gt;registrar&lt;/dkhm:management&gt;

&lt;dkhm:sole_proprietorship xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"&gt;true&lt;/dkhm:sole_proprietorship&gt;

&lt;/extension&gt;&lt;clTRID&gt;ce9b58ce7aa563aebcbff9aca061a711&lt;/clTRID&gt;

&lt;/command&gt;

&lt;/epp&gt;

# Appendix for dkhm:contact_verification

## Appendix 3: info:contact dkhm:responsible

&lt;?xml version="1.0" encoding="UTF-8" standalone="no"?&gt;

&lt;epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd"&gt;

&lt;response&gt;

&lt;result code="1000"&gt;

&lt;msg&gt;Command completed successfully&lt;/msg&gt;

&lt;/result&gt;

&lt;msgQ count="4" id="2618875"&gt; &lt;/msgQ&gt;

&lt;resData&gt;

&lt;contact:infData xmlns:contact="urn:ietf:params:xml:ns:contact-1.0"&gt;

&lt;contact:id&gt;TA20593-DK&lt;/contact:id&gt;

&lt;contact:roid&gt;TA20593-DK&lt;/contact:roid&gt;

&lt;contact:status s="serverDeleteProhibited"/&gt;

&lt;contact:status s="serverTransferProhibited"/&gt;

&lt;contact:postalInfo type="loc"&gt;

&lt;contact:name&gt;Test A/S&lt;/contact:name&gt;

&lt;contact:addr&gt;

&lt;contact:street&gt;Postbox 123&lt;/contact:street&gt;

&lt;contact:city&gt;Paris&lt;/contact:city&gt;

&lt;contact:pc&gt;14588&lt;/contact:pc&gt;

&lt;contact:cc&gt;FR&lt;/contact:cc&gt;

&lt;/contact:addr&gt;

&lt;/contact:postalInfo&gt;

&lt;contact:voice&gt;+33.330325458877&lt;/contact:voice&gt;

&lt;contact:email&gt;<vitester@dk-hostmaster.dk>&lt;/contact:email&gt;

&lt;contact:clID&gt;REG-666666&lt;/contact:clID&gt;

&lt;contact:crID&gt;REG-666666&lt;/contact:crID&gt;

&lt;contact:crDate&gt;2025-11-05T08:52:06.0Z&lt;/contact:crDate&gt;

&lt;/contact:infData&gt;

&lt;/resData&gt;

&lt;extension&gt;

&lt;dkhm:contact_validated xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'&gt;1&lt;/dkhm:contact_validated&gt;

&lt;dkhm:CVR xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'&gt;1414441&lt;/dkhm:CVR&gt;

&lt;dkhm:userType xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'&gt;association&lt;/dkhm:userType&gt;

&lt;dkhm:sole_proprietorship xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'&gt;true&lt;/dkhm:sole_proprietorship&gt;

&lt;dkhm:contact_verification xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'&gt;

&lt;dkhm:responsible&gt;registry&lt;/dkhm:responsible&gt;

&lt;dkhm:verified_id status="notRequired" &gt;false&lt;/dkhm:verified_id&gt;

&lt;dkhm:verified_email status="notRequired" &gt;false&lt;/dkhm:verified_email&gt;

&lt;/dkhm:contact_verification&gt;

&lt;/extension&gt;

&lt;trID&gt;

&lt;clTRID&gt;f66aa8d9ad158d527047da69175c4846&lt;/clTRID&gt;

&lt;svTRID&gt;BF18D09A-BA24-11F0-82F6-DDF1145B6CAF&lt;/svTRID&gt;

&lt;/trID&gt;

&lt;/response&gt;

&lt;/epp&gt;

## Appendix 4: contact:create dkhm:verified_id and dkhm:verified_email

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;epp xmlns="urn:ietf:params:xml:ns:epp-1.0"&gt;

&lt;command&gt;

&lt;create&gt;

&lt;contact:create xmlns:contact="urn:ietf:params:xml:ns:contact-1.0"&gt;

&lt;contact:id&gt;force&lt;/contact:id&gt;

&lt;contact:postalInfo type="loc"&gt;

&lt;contact:name&gt;Punktum dk A/S&lt;/contact:name&gt;

&lt;contact:addr&gt;

&lt;contact:street&gt;Ørestads Boulevard 108, 11.&lt;/contact:street&gt;

&lt;contact:city&gt;København S&lt;/contact:city&gt;

&lt;contact:sp/&gt;

&lt;contact:pc&gt;2300&lt;/contact:pc&gt;

&lt;contact:cc&gt;DK&lt;/contact:cc&gt;

&lt;/contact:addr&gt;

&lt;/contact:postalInfo&gt;

&lt;contact:voice&gt;+45.25780611&lt;/contact:voice&gt;

&lt;contact:email&gt;<vitester@dk-hostmaster.dk>&lt;/contact:email&gt;

&lt;contact:authInfo&gt;

&lt;contact:pw/&gt;

&lt;/contact:authInfo&gt;

&lt;/contact:create&gt;

&lt;/create&gt;

&lt;extension&gt;

&lt;dkhm:userType xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"&gt;company&lt;/dkhm:userType&gt;

&lt;dkhm:CVR xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"&gt;24210375&lt;/dkhm:CVR&gt;

&lt;dkhm:management xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"&gt;registrar&lt;/dkhm:management&gt;

&lt;dkhm:contact_verification xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"&gt;

&lt;dkhm:verified_id&gt;true&lt;/dkhm:verified_id&gt;

&lt;dkhm:verified_email&gt;true&lt;/dkhm:verified_email&gt;

&lt;/dkhm:contact_verification&gt;

&lt;/extension&gt;

&lt;/command&gt;

&lt;/epp&gt;

## Appendix 5: contact:info dkhm:verified_id and dkhm:verified_email

&lt;?xml version="1.0" encoding="UTF-8" standalone="no"?&gt;

&lt;epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd"&gt;

&lt;response&gt;

&lt;result code="1000"&gt;

&lt;msg&gt;Command completed successfully&lt;/msg&gt;

&lt;/result&gt;

&lt;msgQ count="4" id="2618875"&gt; &lt;/msgQ&gt;

&lt;resData&gt;

&lt;contact:infData xmlns:contact="urn:ietf:params:xml:ns:contact-1.0"&gt;

&lt;contact:id&gt;TB15120-DK&lt;/contact:id&gt;

&lt;contact:roid&gt;TB15120-DK&lt;/contact:roid&gt;

&lt;contact:status s="serverDeleteProhibited"/&gt;

&lt;contact:status s="serverTransferProhibited"/&gt;

&lt;contact:postalInfo type="loc"&gt;

&lt;contact:name&gt;Tester Bertram&lt;/contact:name&gt;

&lt;contact:addr&gt;

&lt;contact:street&gt;Ørestads Boulevard 108, 11.&lt;/contact:street&gt;

&lt;contact:city&gt;København S&lt;/contact:city&gt;

&lt;contact:pc&gt;2300&lt;/contact:pc&gt;

&lt;contact:cc&gt;DK&lt;/contact:cc&gt;

&lt;/contact:addr&gt;

&lt;/contact:postalInfo&gt;

&lt;contact:voice&gt;+45.25780611&lt;/contact:voice&gt;

&lt;contact:email&gt;<vitester@dk-hostmaster.dk>&lt;/contact:email&gt;

&lt;contact:clID&gt;REG-666666&lt;/contact:clID&gt;

&lt;contact:crID&gt;REG-666666&lt;/contact:crID&gt;

&lt;contact:crDate&gt;2025-11-05T12:34:21.0Z&lt;/contact:crDate&gt;

&lt;contact:upID&gt;DKHM1-DK&lt;/contact:upID&gt;

&lt;contact:upDate&gt;2025-11-05T12:36:32.0Z&lt;/contact:upDate&gt;

&lt;/contact:infData&gt;

&lt;/resData&gt;

&lt;extension&gt;

&lt;dkhm:contact_validated xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'&gt;0&lt;/dkhm:contact_validated&gt;

&lt;dkhm:userType xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'&gt;individual&lt;/dkhm:userType&gt;

&lt;dkhm:sole_proprietorship xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'&gt;false&lt;/dkhm:sole_proprietorship&gt;

&lt;dkhm:contact_verification xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'&gt;

&lt;dkhm:responsible&gt;registry&lt;/dkhm:responsible&gt;

&lt;dkhm:verified_id status="pending" responsible="registry" expdate="2025-12-05T22:59:59.0Z" &gt;false&lt;/dkhm:verified_id&gt;

&lt;dkhm:verified_email status="pending" responsible="registry" expdate="2025-12-05T22:59:59.0Z" &gt;false&lt;/dkhm:verified_email&gt;

&lt;/dkhm:contact_verification&gt;

&lt;/extension&gt;

&lt;trID&gt;

&lt;clTRID&gt;f66aa8d9ad158d527047da69175c4846&lt;/clTRID&gt;

&lt;svTRID&gt;4B6C691A-BA45-11F0-9997-A3AD3985F547&lt;/svTRID&gt;

&lt;/trID&gt;

&lt;/response&gt;

&lt;/epp&gt;

## Appendix 6: contact:info confirm_email

&lt;epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd"&gt;

&lt;response&gt;

&lt;result code="1000"&gt;

&lt;msg&gt;Command completed successfully&lt;/msg&gt; &lt;/result&gt;

&lt;resData&gt;

&lt;contact:infData xmlns:contact="urn:ietf:params:xml:ns:contact-1.0"&gt;

&lt;contact:id&gt; BTF78-DK &lt;/contact:id&gt;

&lt;contact:roid&gt; BTF78-DK &lt;/contact:roid&gt;

&lt;contact:status s="linked"/&gt;

&lt;contact:status s="serverDeleteProhibited"/&gt;

&lt;contact:status s="serverTransferProhibited"/&gt;

&lt;contact:postalInfo type="loc"&gt;

&lt;contact:name&gt;Bertram Testar&lt;/contact:name&gt;

&lt;contact:addr&gt;

&lt;contact:street&gt;Ørestads Boulevard 100 24.&lt;/contact:street&gt;

&lt;contact:city&gt;København S&lt;/contact:city&gt;

&lt;contact:pc&gt;2300&lt;/contact:pc&gt;

&lt;contact:cc&gt;DK&lt;/contact:cc&gt; &lt;/contact:addr&gt; &lt;/contact:postalInfo&gt;

&lt;contact:voice&gt;+45.32145698&lt;/contact:voice&gt;

&lt;contact:email&gt;<test@test.dk>&lt;/contact:email&gt;

&lt;contact:clID&gt;REG-666666&lt;/contact:clID&gt;

&lt;contact:crID&gt;REG-666666&lt;/contact:crID&gt;

&lt;contact:crDate&gt;2025-10-23T13:12:47.0Z&lt;/contact:crDate&gt;

&lt;contact:upID&gt;DKHM1-DK&lt;/contact:upID&gt;

&lt;contact:upDate&gt;2025-11-27T17:08:13.0Z&lt;/contact:upDate&gt;&lt;/contact:infData&gt; &lt;/resData&gt;

&lt;extension&gt;

&lt;dkhm:contact_validated xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"&gt;0&lt;/dkhm:contact_validated&gt;

&lt;dkhm:mobilephone xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"&gt;+45.32145698&lt;/dkhm:mobilephone&gt;

&lt;dkhm:userType xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"&gt;individual&lt;/dkhm:userType&gt;

&lt;dkhm:sole_proprietorship xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"&gt;false&lt;/dkhm:sole_proprietorship&gt;

&lt;dkhm:contact_verification xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5"&gt;

&lt;dkhm:responsible&gt;registry&lt;/dkhm:responsible&gt;

&lt;dkhm:verified_id expdate="2025-12-27T22:59:59.0Z" responsible="registry" status="pending"&gt;false&lt;/dkhm:verified_id&gt;

&lt;dkhm:verified_email expdate="2025-12-27T22:59:59.0Z" responsible="registry" status="pending"&gt;false&lt;/dkhm:verified_email&gt;

&lt;dkhm:confirm_email expdate="2025-11-28T22:59:59.0Z" responsible="registry" status="pending"&gt;<new_email@for_this_contact.nu>&lt;/dkhm:confirm_email&gt;

&lt;dkhm:confirm_secondary_email expdate="2025-11-28T22:59:59.0Z" responsible="registry" status="pending"&gt;<new_email2@for_this_contact.nu>&lt;/dkhm:confirm_secondary_email&gt;

&lt;/dkhm:contact_verification&gt;

&lt;/extension&gt;

&lt;trID&gt;

&lt;clTRID&gt;648bedcfb723e046e12ea55be1548557&lt;/clTRID&gt;

&lt;svTRID&gt;D7719B0E-CBB3-11F0-AAA7-CFEC4ECF5867&lt;/svTRID&gt;

&lt;/trID&gt;

&lt;/response&gt;

&lt;/epp&gt;test
