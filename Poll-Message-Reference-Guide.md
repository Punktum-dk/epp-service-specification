# Poll Message Reference Guide

This is a complete list of all EPP poll messages currently available in production in the DK Hostmaster EPP service.

## Document History
2023-02-28 Added new poll messages: contact_updated, delete_contact
2022-02-22 Initial document draft published

## Poll Messages

|Object |Operation                    |Message                                                                                                                  |
|-------|-----------------------------|-------------------------------------------------------------------------------------------------------------------------|
|domain |accept_agreement_expire      |The request to transfer %.dk to %-DK has been cancelled, as the new registrant has not accepted our terms and conditions.|
|domain |change_billing               |REG-% has been removed as billing contact for %.dk                                                                       |
|domain |change_nameserver            |Nameservers for domain %.dk has been changed from %, % to %, %                                                           |
|domain |create_domain                |Create domain pending for %.dk                                                                                           |
|domain |create_domain                |Created domain for %.dk has been approved                                                                                |
|domain |create_domain                |Application for domain for %.dk has been enqueued and is pending                                                         |
|domain |create_domain                |Application for domain: %.dk has expired                                                                                 |
|domain |create_domain                |Object exists                                                                                                            |
|domain |delete_domain                |Domain %.dk has been deleted                                                                                             |
|domain |dsrecords_changed            |DS Records on %.dk has been changed                                                                                      |
|domain |transfer_domain              |REG-% has been removed as registrar for %.dk                                                                             |
|domain |transfer_domain              |%.dk has been transferred to registrar REG-%                                                                             |
|domain |transfer_domain              |Transfer of %.dk to %-DK has been completed                                                                              |
|domain |transfer_domain              |Transfer of %.dk to %-DK has expired                                                                                     |
|domain |manual_id_control_acknowledge|The new registrant, %-DK, has completed our mandatory ID check, for the request to transfer %.dk to %-DK.                |
|domain |manual_id_control_expire     |The request to transfer %.dk to %-DK has expired, as the new registrant has not completed our mandatory ID check.        |
|domain |accept_agreement_acknowledge |The new registrant, %-DK, has accepted our terms and conditions, for the request to transfer %.dk to %-DK.               |
|host   |accept_nameserver_role       |Transfer of name server % has been accepted                                                                              |
|host   |create_host                  |Host % has been created                                                                                                  |
|contact|contact_updated              |Contact information has been updated for %-DK                                                                            |
|contact|delete_contact               |Contact %-DK has been deleted                                                                                            |
