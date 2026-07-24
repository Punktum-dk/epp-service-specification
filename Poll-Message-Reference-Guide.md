# Poll Message Reference Guide

This is a complete list of all EPP poll messages currently available in production in the Punktum dk EPP service.

## Document History

- 2026-03-11 Added two new poll messages regarding email bounce
    - %reason% field for email bounce can contain the following: "antispam", "bad_checksum", "config", "failed" and "unknown"
    - Updated the domain update registrant messages in order to provide clearer wording
    - Removed deprecated poll message from the domain update registrant section regarding lack of acceptance of terms. Terms are now always accepted before the change of registrant operation are started.
- 2025-12-23 Added two categories of poll messages:
    - contact update primary/secondary email - which are used, if a change of primary/secondary email needs to be confirmed.
    - contact update verification - which are used, if a contact needs a mandatory verification of id and/or data (currently data only includes email)
- 2025-10-07 Corrected the current host create/update messages to reflect that is now possible to use a REG-handle as a name server manager.
- 2025-09-03 Corrected typo in the current message for contact update.
- 2025-03-10 The document has been restructured:
    - Operation now reflects the corresponding EPP command - e.g. create, update, delete replacing e.g. create_domain, domain_updated and delete_domain
    - The ResData type column has been added to reflect the type of ResData which will be returned along with the poll message.

        In some cases we will choose between returning either panData or infData - e.g. messages regardng the deletion of a domain:
        - panData will be returned if the operation is the result of a pending operation requesting the deletion.
        - infData will be returned if the deletion is the result of another operation - e.g deletion due to lack of renewal.

    - Some poll messages regarding the application for a domain has been deprecated since they do not adhere to the standard process for a pending operation:
        - create_domain: "Create domain pending for %.dk"
        - create_domain: "Application for domain for %.dk has been enqueued and is pending"

        Only one poll message with domain:panData containing the final result of the pending operation will now be returned for a pending domain application.

    - The following poll messages regarding the transfer of a registrarhandled domain from one registrant to another has been deprecated since they do not adhere to the standard process for a pending operation:
        - accept_agreement_acknowledge: "The new registrant, %-DK, has accepted our terms and conditions, for the request to transfer %.dk to %-DK."
        - accept_agreement_expire: "The request to transfer %.dk to %-DK has been cancelled, as the new registrant has not accepted our terms and conditions"
        - manual_id_control: "The mandatory ID check of %-DK has been approved, for the request to transfer %.dk to %-DK"
        - manual_id_control_acknowledge: "The new registrant, %-DK, has completed our mandatory ID check, for the request to transfer %.dk to %-DK."

        Only one poll message with domain:panData containing the final result of the pending operation will now be returned for a pending registrant transfer.

    - Adjustments in the text messages returned in the poll message e.g. "Transfer of %.dk to %-DK has been completed" has been changed to "%.dk has been transferred to new registrant %-DK". The changes in the text messages are made in order to provide a more consistent communication across poll messages, emails and event logging.

    - Several new poll messages have been added and some of the existing messages have been split up into several messages with more details e.g. "Created domain for %.dk has been approved" has been split into "%.dk has been registered and activated" and "%.dk has been registered, but not activated due to pending ID check"

- 2025-02-21 Added new poll message domain_activated and missing documentation of existing poll message create_domain (Application for domain: %.dk rejected. User and domain handling mismatch)
- 2024-04-16 Added missing documentation of existing poll messages domain_updated and manual_id_control
- 2023-02-28 Added new poll messages contact_updated and delete_contact
- 2022-02-22 Initial document draft published


## Placeholders

The messages in this guide contain placeholders that are substituted with actual values when a poll message is generated. The placeholders are:

| Placeholder     | Description |
| --------------- | ----------- |
| `%.dk`          |             |
| `%-DK`          |             |
| `%email%`       |             |
| `%responsible%` |             |
| `%reason%`      |             |
| `%`             |             |

## Poll Messages

|Object  |Operation                    |Message                     |Example |ResData type |
|--------|-----------------------------|----------------------------|--------|-------------|
|contact |update |The contact information has been updated for %-DK |[view](#ex-1) |contact:infData |
|contact |update primary email |%-DK has to confirm the new primary email, %email%, to complete the update - %responsible% |[view](#ex-2) |contact:infData |
|contact |update primary email |%-DK has confirmed the new primary email, %email% - %responsible% |[view](#ex-3) |contact:panData |
|contact |update primary email |The new primary email, %email%, was not confirmed for %-DK - %responsible% |[view](#ex-4) |contact:panData |
|contact |update secondary email |%-DK has to confirm new secondary email, %email%, to complete the update - %responsible% |[view](#ex-5) |contact:infData |
|contact |update secondary email |%-DK has confirmed the new secondary email, %email% - %responsible% |[view](#ex-6) |contact:panData |
|contact |update secondary email |The new secondary email, %email%, was not confirmed for %-DK - %responsible% |[view](#ex-7) |contact:panData |
|contact |update verification |%-DK has to complete the mandatory ID and data check - %responsible% |[view](#ex-8) |contact:infData |
|contact |update verification |%-DK has to complete the mandatory ID check - %responsible% |[view](#ex-9) |contact:infData |
|contact |update verification |%-DK has to complete the mandatory data check - %responsible% |[view](#ex-10) |contact:infData |
|contact |update verification |The mandatory ID and data check of %-DK has expired - %responsible% |[view](#ex-11) |contact:infData |
|contact |update verification |The mandatory ID check of %-DK has expired - %responsible% |[view](#ex-12) |contact:infData |
|contact |update verification |The mandatory data check of %-DK has expired - %responsible% |[view](#ex-13) |contact:infData |
|contact |update verification |%-DK has completed the mandatory ID and data check - %responsible% |[view](#ex-14) |contact:infData |
|contact |update verification |%-DK has completed the mandatory ID check - %responsible% |[view](#ex-15) |contact:infData |
|contact |update verification |%-DK has completed the mandatory data check - %responsible% |[view](#ex-16) |contact:infData |
|contact |update verification |The mandatory ID check of %-DK was rejected - %responsible% |[view](#ex-17) |contact:infData |
|contact |update verification |The mandatory ID and data check of %-DK was cancelled - %responsible% |[view](#ex-18) |contact:infData |
|contact |update verification |The mandatory ID check of %-DK was cancelled - %responsible% |[view](#ex-19) |contact:infData |
|contact |update verification |The mandatory data check of %-DK was cancelled - %responsible% |[view](#ex-20) |contact:infData |
|contact |email bounce |Email delivery (failed) %reason% for the primary email, %email%, of %-DK. Please review and correct the email address. |[view](#ex-21) |contact:infData |
|contact |email bounce |Email delivery (failed) %reason% for the secondary email, %email%, of %-DK. Please review and correct the email address. |[view](#ex-22) |contact:infData |
|contact |delete |%-DK has been deleted |[view](#ex-23) |contact:infData |
|domain |create |%.dk has been registered and activated |[view](#ex-24) |domain:panData |
|domain |create |%.dk has been registered, but not activated due to pending ID and/or data check |[view](#ex-25) |domain:panData |
|domain |create |The application for %.dk has been rejected, as the domain was already taken |[view](#ex-26) |domain:panData |
|domain |create |The application for %.dk has been cancelled, as the registrant has not accepted our terms and conditions in time |[view](#ex-27) |domain:panData |
|domain |create |The application for %.dk has been rejected, as the user and domain handling mismatched |[view](#ex-28) |domain:panData |
|domain |create |The application for %.dk has been cancelled |[view](#ex-29) |domain:panData |
|domain |update |%.dk has been activated |[view](#ex-30) |domain:infData |
|domain |update |%.dk has been updated |[view](#ex-31) |domain:infData |
|domain |update billing |REG-% has been removed as billing contact for %.dk |[view](#ex-32) |domain:infData |
|domain |update dsrecords |DS records has been changed for %.dk |[view](#ex-33) |domain:infData |
|domain |update name servers |Name servers has been changed for %.dk, from %, %, … to %, %, … |[view](#ex-34) |domain:infData |
|domain |update registrant |The registrant has been changed to %-DK for %.dk |[view](#ex-35) |domain:panData |
|domain |update registrant |The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was not completed in time |[view](#ex-36) |domain:panData |
|domain |update registrant |The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was rejected |[view](#ex-37) |domain:panData |
|domain |update registrant |The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was not completed |[view](#ex-38) |domain:panData |
|domain |transfer |%.dk has been added to your portfolio |[view](#ex-39) |domain:trnData |
|domain |transfer |%.dk has been removed from your portfolio |[view](#ex-40) |domain:trnData |
|domain |delete |%.dk has been deleted |[view](#ex-41) |domain:panData |
|domain |delete |%.dk has been deleted |[view](#ex-42) |domain:infData |
|domain |delete |%.dk has been extended and cancellation stopped |[view](#ex-43) |domain:panData |
|domain |delete |%.dk has been restored, extended and cancellation stopped |[view](#ex-44) |domain:panData |
|host |create |The name server %.dk has been registered, as the registrant has approved it |[view](#ex-45) |host:panData |
|host |create |The name server %.dk has not been registered, as it has been rejected by the registrant |[view](#ex-46) |host:panData |
|host |create |The name server %.dk has not been registered, as it was not approved by the registrant in time |[view](#ex-47) |host:panData |
|host |create |The name server %.dk has been registered, as the registrant has approved it and % has accepted the name server manager role |[view](#ex-48) |host:panData |
|host |create |The name server %.dk has not been registered, as the name server manager role has been rejected |[view](#ex-49) |host:panData |
|host |create |The name server %.dk has not been registered, as the name server manager role was not accepted in time |[view](#ex-50) |host:panData |
|host |create |The name server %.dk has been registered, as % has accepted the name server manager role |[view](#ex-51) |host:panData |
|host |update |The name server manager role for % has been accepted by % |[view](#ex-52) |host:panData |
|host |update |The name server manager role for % has been rejected by % |[view](#ex-53) |host:panData |
|host |update |The name server manager role for % has not been accepted by % in time |[view](#ex-54) |host:panData |
|host |delete |The name server % has been deleted |[view](#ex-55) |host:infData |

## Examples

Each poll message below has a collapsible XML example. Click **Show XML example** to expand it.

### contact

#### update

<a id="ex-1"></a>
**Message:** The contact information has been updated for %-DK  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

#### update primary email

<a id="ex-2"></a>
**Message:** %-DK has to confirm the new primary email, %email%, to complete the update - %responsible%  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-3"></a>
**Message:** %-DK has confirmed the new primary email, %email% - %responsible%  
**ResData type:** `contact:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-4"></a>
**Message:** The new primary email, %email%, was not confirmed for %-DK - %responsible%  
**ResData type:** `contact:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

#### update secondary email

<a id="ex-5"></a>
**Message:** %-DK has to confirm new secondary email, %email%, to complete the update - %responsible%  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-6"></a>
**Message:** %-DK has confirmed the new secondary email, %email% - %responsible%  
**ResData type:** `contact:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-7"></a>
**Message:** The new secondary email, %email%, was not confirmed for %-DK - %responsible%  
**ResData type:** `contact:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

#### update verification

<a id="ex-8"></a>
**Message:** %-DK has to complete the mandatory ID and data check - %responsible%  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-9"></a>
**Message:** %-DK has to complete the mandatory ID check - %responsible%  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-10"></a>
**Message:** %-DK has to complete the mandatory data check - %responsible%  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-11"></a>
**Message:** The mandatory ID and data check of %-DK has expired - %responsible%  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-12"></a>
**Message:** The mandatory ID check of %-DK has expired - %responsible%  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-13"></a>
**Message:** The mandatory data check of %-DK has expired - %responsible%  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-14"></a>
**Message:** %-DK has completed the mandatory ID and data check - %responsible%  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-15"></a>
**Message:** %-DK has completed the mandatory ID check - %responsible%  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-16"></a>
**Message:** %-DK has completed the mandatory data check - %responsible%  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-17"></a>
**Message:** The mandatory ID check of %-DK was rejected - %responsible%  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-18"></a>
**Message:** The mandatory ID and data check of %-DK was cancelled - %responsible%  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-19"></a>
**Message:** The mandatory ID check of %-DK was cancelled - %responsible%  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-20"></a>
**Message:** The mandatory data check of %-DK was cancelled - %responsible%  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

#### email bounce

<a id="ex-21"></a>
**Message:** Email delivery (failed) %reason% for the primary email, %email%, of %-DK. Please review and correct the email address.  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-22"></a>
**Message:** Email delivery (failed) %reason% for the secondary email, %email%, of %-DK. Please review and correct the email address.  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

#### delete

<a id="ex-23"></a>
**Message:** %-DK has been deleted  
**ResData type:** `contact:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

### domain

#### create

<a id="ex-24"></a>
**Message:** %.dk has been registered and activated  
**ResData type:** `domain:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-25"></a>
**Message:** %.dk has been registered, but not activated due to pending ID and/or data check  
**ResData type:** `domain:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-26"></a>
**Message:** The application for %.dk has been rejected, as the domain was already taken  
**ResData type:** `domain:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-27"></a>
**Message:** The application for %.dk has been cancelled, as the registrant has not accepted our terms and conditions in time  
**ResData type:** `domain:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-28"></a>
**Message:** The application for %.dk has been rejected, as the user and domain handling mismatched  
**ResData type:** `domain:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-29"></a>
**Message:** The application for %.dk has been cancelled  
**ResData type:** `domain:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

#### update

<a id="ex-30"></a>
**Message:** %.dk has been activated  
**ResData type:** `domain:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-31"></a>
**Message:** %.dk has been updated  
**ResData type:** `domain:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

#### update billing

<a id="ex-32"></a>
**Message:** REG-% has been removed as billing contact for %.dk  
**ResData type:** `domain:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

#### update dsrecords

<a id="ex-33"></a>
**Message:** DS records has been changed for %.dk  
**ResData type:** `domain:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

#### update name servers

<a id="ex-34"></a>
**Message:** Name servers has been changed for %.dk, from %, %, … to %, %, …  
**ResData type:** `domain:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

#### update registrant

<a id="ex-35"></a>
**Message:** The registrant has been changed to %-DK for %.dk  
**ResData type:** `domain:panData`  
**Old message (pre 2026-03-11):** %.dk has been transferred to new registrant %-DK  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-36"></a>
**Message:** The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was not completed in time  
**ResData type:** `domain:panData`  
**Old message (pre 2026-03-11):** The transfer of %.dk to the new registrant %-DK has been cancelled, as the mandatory ID check was not completed in time  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-37"></a>
**Message:** The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was rejected  
**ResData type:** `domain:panData`  
**Old message (pre 2026-03-11):** The transfer of %.dk to the new registrant %-DK has been cancelled, as the mandatory ID check was rejected  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-38"></a>
**Message:** The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was not completed  
**ResData type:** `domain:panData`  
**Old message (pre 2026-03-11):** The transfer of %.dk to the new registrant %-DK has been cancelled, as the mandatory ID check was not completed  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

#### transfer

<a id="ex-39"></a>
**Message:** %.dk has been added to your portfolio  
**ResData type:** `domain:trnData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-40"></a>
**Message:** %.dk has been removed from your portfolio  
**ResData type:** `domain:trnData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

#### delete

<a id="ex-41"></a>
**Message:** %.dk has been deleted  
**ResData type:** `domain:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-42"></a>
**Message:** %.dk has been deleted  
**ResData type:** `domain:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-43"></a>
**Message:** %.dk has been extended and cancellation stopped  
**ResData type:** `domain:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-44"></a>
**Message:** %.dk has been restored, extended and cancellation stopped  
**ResData type:** `domain:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

### host

#### create

<a id="ex-45"></a>
**Message:** The name server %.dk has been registered, as the registrant has approved it  
**ResData type:** `host:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-46"></a>
**Message:** The name server %.dk has not been registered, as it has been rejected by the registrant  
**ResData type:** `host:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-47"></a>
**Message:** The name server %.dk has not been registered, as it was not approved by the registrant in time  
**ResData type:** `host:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-48"></a>
**Message:** The name server %.dk has been registered, as the registrant has approved it and % has accepted the name server manager role  
**ResData type:** `host:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-49"></a>
**Message:** The name server %.dk has not been registered, as the name server manager role has been rejected  
**ResData type:** `host:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-50"></a>
**Message:** The name server %.dk has not been registered, as the name server manager role was not accepted in time  
**ResData type:** `host:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-51"></a>
**Message:** The name server %.dk has been registered, as % has accepted the name server manager role  
**ResData type:** `host:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

#### update

<a id="ex-52"></a>
**Message:** The name server manager role for % has been accepted by %  
**ResData type:** `host:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-53"></a>
**Message:** The name server manager role for % has been rejected by %  
**ResData type:** `host:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

<a id="ex-54"></a>
**Message:** The name server manager role for % has not been accepted by % in time  
**ResData type:** `host:panData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>

#### delete

<a id="ex-55"></a>
**Message:** The name server % has been deleted  
**ResData type:** `host:infData`  

<details>
<summary>Show XML example</summary>

```xml

```

</details>
