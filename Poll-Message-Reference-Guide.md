# Poll Message Reference Guide

This is a complete list of all EPP poll messages currently available in production in the Punktum dk EPP service.

## Document History

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

|Object |Operation                    |Message                     |Old message (pre 2025-03-10) |ResData type |
|-------|-----------------------------|----------------------------|-----------------------------|-------------|
|contact|update |The contact information has been updated for %-DK |Contact information has been updated for %-DK |contact:infData |
|contact|delete |%-DK has been deleted |Contact %-DK has been deleted |contact:infData |
|domain |create |%.dk has been registered and activated |Created domain for %.dk has been approved |domain:panData |
|domain |create |%.dk has been registered, but not activated due to pending ID check |Created domain for %.dk has been approved |domain:panData |
|domain |create |The application for %.dk has been rejected, as the domain was already taken |Object exists |domain:panData |
|domain |create |The application for %.dk has been cancelled, as the registrant has not accepted our terms and conditions in time |Application for domain: %.dk has expired |domain:panData |
|domain |create |The application for %.dk has been rejected, as the user and domain handling mismatched |Application for domain: %.dk rejected. User and domain handling mismatch |domain:panData |
|domain |create |The application for %.dk has been cancelled | |domain:panData |
|domain |update |%.dk has been activated |Domain %.dk has been activated |domain:infData |
|domain |update |%.dk has been updated |Domain %.dk has been updated |domain:infData |
|domain |update billing |REG-% has been removed as billing contact for %.dk |% has been removed as billing contact for %.dk |domain:infData |
|domain |update dsrecords |DS records has been changed for %.dk |DS Records on %.dk has been changed |domain:infData |
|domain |update name servers |Name servers has been changed for %.dk, from %, %, … to %, %, … |Nameservers for domain %.dk has been changed from %, % to %, % |domain:infData |
|domain |update registrant |%.dk has been transferred to new registrant %-DK |Transfer of %.dk to %-DK has been completed |domain:panData |
|domain |update registrant |The transfer of %.dk to the new registrant %-DK has been cancelled, as our terms was not accepted in time |Transfer of %.dk to %-DK has expired |domain:panData |
|domain |update registrant |The transfer of %.dk to the new registrant %-DK has been cancelled, as the mandatory ID check was rejected |The mandatory ID check of %-DK has been rejected, for the request to transfer %.dk to %-DK |domain:panData |
|domain |update registrant |The transfer of %.dk to the new registrant %-DK has been cancelled, as the mandatory ID check was not completed in time | |domain:panData |
|domain |update registrant |The transfer of %.dk to the new registrant %-DK has been cancelled, as the mandatory ID check was not completed |The request to transfer %.dk to %-DK has expired, as the new registrant has not completed our mandatory ID check. |domain:panData |
|domain |transfer |%.dk has been added to your portfolio |%.dk has been transferred to registrar REG-%| domain:trnData |
|domain |transfer |%.dk has been removed from your portfolio |REG-% has been removed as registrar for %.dk |domain:trnData |
|domain |delete |%.dk has been deleted |Domain %.dk has been deleted |domain:panData |
|domain |delete |%.dk has been deleted |Domain %.dk has been deleted |domain:infData |
|domain |delete |%.dk has been extended and cancellation stopped | |domain:panData |
|domain |delete |%.dk has been restored, extended and cancellation stopped | |domain:panData |
|host |create |The name server %.dk has been registered, as the registrant has approved it |Host % has been created |host:panData |
|host |create |The name server %.dk has not been registered, as it has been rejected by the registrant | |host:panData |
|host |create |The name server %.dk has not been registered, as it was not approved by the registrant in time | |host:panData |
|host |create |The name server %.dk has been registered, as the registrant has approved it and % has accepted the name server manager role |Host % has been created |host:panData |
|host |create |The name server %.dk has not been registered, as the name server manager role has been rejected | |host:panData |
|host |create |The name server %.dk has not been registered, as the name server manager role was not accepted in time | |host:panData |
|host |create |The name server %.dk has been registered, as % has accepted the name server manager role |Host % has been created |host:panData |
|host |update |The name server manager role for % has been accepted by % |Transfer of name server % has been accepted |host:panData |
|host |update |The name server manager role for % has been rejected by % | |host:panData |
|host |update |The name server manager role for % has not been accepted by % in time | |host:panData |
|host |delete |The name server % has been deleted | | host:infData |
