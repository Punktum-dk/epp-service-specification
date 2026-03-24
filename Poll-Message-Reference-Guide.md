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

## Poll Messages

|Object  |Operation                    |Message                     |Old message (pre 2026-03-11) |ResData type |
|--------|-----------------------------|----------------------------|-----------------------------|-------------|
|contact |update |The contact information has been updated for %-DK | |contact:infData |
|contact |update primary email |%-DK has to confirm the new primary email, %email%, to complete the update - %responsible% | |contact:infData |
|contact |update primary email |%-DK has confirmed the new primary email, %email% - %responsible% | |contact:panData |
|contact |update primary email |The new primary email, %email%, was not confirmed for %-DK - %responsible% | |contact:panData |
|contact |update secondary email |%-DK has to confirm new secondary email, %email%, to complete the update - %responsible% | |contact:infData |
|contact |update secondary email |%-DK has confirmed the new secondary email, %email% - %responsible% | |contact:panData |
|contact |update secondary email |The new secondary email, %email%, was not confirmed for %-DK - %responsible% | |contact:panData |
|contact |update verification |%-DK has to complete the mandatory ID and data check - %responsible% | |contact:infData |
|contact |update verification |%-DK has to complete the mandatory ID check - %responsible% | |contact:infData |
|contact |update verification |%-DK has to complete the mandatory data check - %responsible% | |contact:infData |
|contact |update verification |The mandatory ID and data check of %-DK has expired - %responsible% | |contact:infData |
|contact |update verification |The mandatory ID check of %-DK has expired - %responsible% | |contact:infData |
|contact |update verification |The mandatory data check of %-DK has expired - %responsible% | |contact:infData |
|contact |update verification |%-DK has completed the mandatory ID and data check - %responsible% | |contact:infData |
|contact |update verification |%-DK has completed the mandatory ID check - %responsible% | |contact:infData |
|contact |update verification |%-DK has completed the mandatory data check - %responsible% | |contact:infData |
|contact |update verification |The mandatory ID check of %-DK was rejected - %responsible% | |contact:infData |
|contact |update verification |The mandatory ID and data check of %-DK was cancelled - %responsible% | |contact:infData |
|contact |update verification |The mandatory ID check of %-DK was cancelled - %responsible% | |contact:infData |
|contact |update verification |The mandatory data check of %-DK was cancelled - %responsible% | |contact:infData |
|contact |email bounce |Email delivery (failed) %reason% for the primary email, %email%, of %-DK. Please review and correct the email address. | |contact:infData |
|contact |email bounce |Email delivery (failed) %reason% for the secondary email, %email%, of %-DK. Please review and correct the email address. | |contact:infData |
|contact |delete |%-DK has been deleted | |contact:infData |
|domain |create |%.dk has been registered and activated | |domain:panData |
|domain |create |%.dk has been registered, but not activated due to pending ID and/or data check | |domain:panData |
|domain |create |The application for %.dk has been rejected, as the domain was already taken | |domain:panData |
|domain |create |The application for %.dk has been cancelled, as the registrant has not accepted our terms and conditions in time | |domain:panData |
|domain |create |The application for %.dk has been rejected, as the user and domain handling mismatched | |domain:panData |
|domain |create |The application for %.dk has been cancelled | |domain:panData |
|domain |update |%.dk has been activated | |domain:infData |
|domain |update |%.dk has been updated | |domain:infData |
|domain |update billing |REG-% has been removed as billing contact for %.dk | |domain:infData |
|domain |update dsrecords |DS records has been changed for %.dk | |domain:infData |
|domain |update name servers |Name servers has been changed for %.dk, from %, %, … to %, %, … | |domain:infData |
|domain |update registrant |The registrant has been changed to %-DK for %.dk |%.dk has been transferred to new registrant %-DK |domain:panData |
|domain |update registrant |The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was not completed in time |The transfer of %.dk to the new registrant %-DK has been cancelled, as the mandatory ID check was not completed in time |domain:panData |
|domain |update registrant |The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was rejected |The transfer of %.dk to the new registrant %-DK has been cancelled, as the mandatory ID check was rejected |domain:panData |
|domain |update registrant |The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was not completed |The transfer of %.dk to the new registrant %-DK has been cancelled, as the mandatory ID check was not completed |domain:panData |
|domain |transfer |%.dk has been added to your portfolio | | domain:trnData |
|domain |transfer |%.dk has been removed from your portfolio | |domain:trnData |
|domain |delete |%.dk has been deleted | |domain:panData |
|domain |delete |%.dk has been deleted | |domain:infData |
|domain |delete |%.dk has been extended and cancellation stopped | |domain:panData |
|domain |delete |%.dk has been restored, extended and cancellation stopped | |domain:panData |
|host |create |The name server %.dk has been registered, as the registrant has approved it | |host:panData |
|host |create |The name server %.dk has not been registered, as it has been rejected by the registrant | |host:panData |
|host |create |The name server %.dk has not been registered, as it was not approved by the registrant in time | |host:panData |
|host |create |The name server %.dk has been registered, as the registrant has approved it and % has accepted the name server manager role | |host:panData |
|host |create |The name server %.dk has not been registered, as the name server manager role has been rejected | |host:panData |
|host |create |The name server %.dk has not been registered, as the name server manager role was not accepted in time | |host:panData |
|host |create |The name server %.dk has been registered, as % has accepted the name server manager role | |host:panData |
|host |update |The name server manager role for % has been accepted by % | |host:panData |
|host |update |The name server manager role for % has been rejected by % | |host:panData |
|host |update |The name server manager role for % has not been accepted by % in time | |host:panData |
|host |delete |The name server % has been deleted | | host:infData |
