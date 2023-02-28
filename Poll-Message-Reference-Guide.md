# Poll Message Reference Guide

This is a complete list of all EPP poll messages currently available in production in the DK Hostmaster EPP service.

## Document History
2022-02-22  Initial document draft published

## Poll Messages

|Object |Operation                    |Message                                                                                                            |
|-------|-----------------------------|-------------------------------------------------------------------------------------------------------------------|
|domain |create_domain                |Create domain pending for %s.dk                                                                                    |
|domain |create_domain                |Created domain for %s.dk has been approved                                                                         |
|domain |create_domain                |Application for domain for %s.dk has been enqueued and is pending                                                  |
|domain |create_domain                |Application for domain: %s.dk has expired                                                                          |
|domain |transfer_domain              |REG-%i has been removed as registrar for %s.dk                                                                     |
|domain |transfer_domain              |%s.dk has been transferred to registrar REG-%i                                                                     |
|domain |transfer_domain              |Transfer of %s.dk to %s-DK has been completed                                                                      |
|domain |manual_id_control_acknowledge|The new registrant, %s-DK, has completed our mandatory ID check, for the request to transfer %s.dk to %s-DK.       |
|domain |manual_id_control_expire     |The request to transfer %s.dk to %s-DK has expired, as the new registrant has not completed our mandatory ID check.|
|domain |accept_agreement_acknowledge |The new registrant, %s-DK, has accepted our terms and conditions, for the request to transfer %s.dk to %s-DK.      |
|host   |accept_nameserver_role       |Transfer of name server %s has been accepted                                                                       |
|domain |create_domain                |Application for domain: %s.dk has been enqueued and is pending                                                     |
|domain |transfer_domain              |Transfer of %s.dk to %s-DK has expired                                                                             |
|domain |create_domain                |Object exists                                                                                                      |
|domain |change_nameserver            |Nameservers for domain %s.dk has been changed from %s, %s to %s, %s                                                |
|contact|contact_updated              |Contact information has been updated for %s-DK                                                                     |
