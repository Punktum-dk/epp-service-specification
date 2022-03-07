# EPP Status Code Reference Guide

This is a complete list of all EPP status codes and messages supported by DK Hostmaster EPP service.

## Changelog

* 2202-03-02 First version release. 

## General status messages

|Status                               |Status Code|Status Text                                                           |Description                                                                                                             |
|-------------------------------------|:---------:|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_SUCCESS_CODE                     |1000       |Command completed successfully                                        |Command completed and requested data created/updated/deleted                                                            |
|EPP_COMMAND_FAILED                   |2400       |Command did not return a status code                                  |Internal error                                                                                                          |
|EPP_COMMAND_FAILED                   |2400       |Internal error.                                                       |Unhandled internal error                                                                                                |
|EPP_UNIMPLEMENTED                    |2101       |Unimplemented                                                         |The requested command has not been implemented                                                                          |
|EPP_COMMAND_FAILED_FATAL             |2500       |Internal error: Data service operation not configured.                |EPP server misconfiguration                                                                                             |
|EPP_COMMAND_FAILED                   |2400       |Internal error: No connection to data service.                        |No connection from EPP server to internal data service.                                                                                   |
|EPP_COMMAND_FAILED_FATAL             |2500       |Internal error: No prices received from data service.                 |Error in response from internal data service.                                                                                            |
|EPP_BILLING_FAILURE                  |2104       |Insufficient credit. Domain cannot be created.                        |Cannot create domains without sufficient credit on prepaid account                                                      |
|EPP_BILLING_FAILURE                  |2104       |Insufficient credit. Domain cannot be renewed.                        |Cannot renew domains without sufficient credit on prepaid account                                                       |
|EPP_UNKNOWN_COMMAND                  |2000       |No command specified                                                  |No or unknown command specified in EPP request                                                                          |
|EPP_COMMAND_SYNTAX_ERROR             |2001       |Bad command name                                                      |Command name specified contains invalid characters                                                                      |
|EPP_UNKNOWN_COMMAND                  |2000       |Unknown command                                                       |No or unknown command specified in EPP request                                                                          |
|EPP_COMMAND_FAILED                   |2400       |Command does not match endpoint or command data is missing            |hello and logout have empty command tags. Other commands must have command tags                                         |
|EPP_COMMAND_USE_ERROR                |2002       |You need to login first.                                              |Log in before issuing other commands                                                                                    |

## Create contact

|Status                               |Status Code|Status Text                                                           |Description                                                                                                             |
|-------------------------------------|-----------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_PARAMETER_MISSING_ERROR          |2003       |Value required for extension.dkhm:CVR                                 |CVR must be specified for company, public organizations and associations                                                |
|EPP_PARAMETER_POLICY_ERROR           |2306       |Parameter value policy error                                          |Userid must be AUTO_GENERATE or FORCE_GENERATE                                                                          |
|EPP_OBJECT_EXISTS_ERROR              |2302       |Contact already exists                                                |Internal error, userid generated based on initials already exists                                                       |
|EPP_SUCCESS_CODE                     |1000       |Contact already exists. Please re-use.                                |The user data already exists in the database. Please reuse existing contact                                             |
|EPP_SUCCESS_CODE                     |1000       |Contact created.                                                      |New contact created in database                                                                                         |

## Update contact

|Status                               |Status Code|Status Text                                                           |Description                                                                                                             |
|-------------------------------------|-----------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_COMMAND_FAILED                   |2400       |Password change failed                                                |Change password did not succeed.                                                                                        |
|EPP_AUTHORIZATION_ERROR              |2400       |You need to login first.                                              |You need to login first.                                                                                                |
|EPP_AUTHORIZATION_ERROR              |2400       |You are not authorized to change password on that contact             |You are not authorized to change password on that contact                                                               |
|EPP_AUTHORIZATION_ERROR              |2400       |Permission denied to change email                                     |You are not authorized to change email on that contact                                                                  |
|EPP_PENDING_CODE                     |1001       |Command completed. Pending e-mail-address verification.               |Email address change registered and is awaiting verification                                                            |
|EPP_AUTHORIZATION_ERROR              |2400       |You are not authorized to change that contact                         |You are not authorized to change that contact                                                                           |
|EPP_PROHIBITED_OPERATION             |2304       |You do not have permissions to modify field: %s                       |Field specified that user does not have privileges to modify                                                            |
|EPP_PARAMETER_SYNTAX_ERROR           |2005       |Invalid CVR number                                                    |Specified CVR number does not match CVR number specification                                                            |
|EPP_PARAMETER_SYNTAX_ERROR           |2005       |Name and CVR number does not match                                    |Specified name and CVR number does not match data registered at CVR                                                     |
|EPP_PARAMETER_SYNTAX_ERROR           |2005       |You need to specify pnumber                                           |If CVR number has > 1 pnumber attached, pnumber must be specified                                                       |
|EPP_COMMAND_FAILED                   |2400       |Unable to verify CVR number. Please try again later.                  |We are unable to contact CVR for data verification                                                                      |
|EPP_DATA_MANAGEMENT_POLICY_VIOLATION |2308       |Multiple operations perfomed in one request: (%s)                     |The client attempted to perform multiple operations on an object in the same request                                    |

## Info contact

|Status                               |Status Code|Status Text                                                           |Description                                                                                                             |
|-------------------------------------|-----------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_OBJECT_NOT_EXISTS_ERROR          |2302       |Contact not found                                                     |The specified contact does not exist                                                                                    |
|EPP_AUTHORIZATION_ERROR              |2201       |Unauthorized                                                          |The user is not allowed to view info on specified contact                                                               |

## Create domain

|Status                               |Status Code|Status Text                                                           |Description                                                                                                             |
|-------------------------------------|-----------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_PARAMETER_POLICY_ERROR           |2306       |Specifying contacts for registrar managed domains is not allowed      |For registrar managed domains, contacts are assigned automatically                                                      |
|EPP_PARAMETER_SYNTAX_ERROR           |2005       |Invalid period %s. Must be within range                               |The period specified is not supported                                                                                   |
|EPP_AUTHORIZATION_ERROR              |2201       |Contact has previously failed ID control and cannot become registrant.|The contact specified failed ID control and can not become registrant                                              |
|EPP_AUTHORIZATION_ERROR              |2201       |Registrar handle is not permitted for registrant role.                |REG-% handles can not be used as registrants                                                                            |
|EPP_PARAMETER_RANGE_ERROR            |2004       |Invalid or inactive nameserver: %s                                    |The specified nameserver is invalid or inactive                                                                         |
|EPP_PARAMETER_RANGE_ERROR            |2004       |dkhm.autorenew 'N' not allowed with dkhm:management choice "registrant"|Autorenew can not be false for registrant handled domains                                                              |
|EPP_PARAMETER_RANGE_ERROR            |2004       |Too few users for contact type                                        |The number of specified contacts is below minimum for that contact type                                                 |
|EPP_PARAMETER_RANGE_ERROR            |2004       |Too many users for contact type                                       |The number of specified contacts is above maximum for that contact type                                                 |
|EPP_PENDING_CODE                     |1001       |Create domain pending for %s                                          |The domain application has been accepted and domain is pending creation                                                 |
|EPP_INVALID_AUTHORIZATION_INFORMATION|2202       |Invalid authorization information                                     |The registrant userid does not match the userid on the waitinglist                                                      |
|EPP_OBJECT_EXISTS_ERROR              |2302       |Domain Name is occupied                                               |The domain is already registered by another user                                                                        |
|EPP_PARAMETER_POLICY_ERROR           |2306       |Parameter value policy error                                          |The domain name contains invalid characters or are invalid format                                                       |
|EPP_PARAMETER_SYNTAX_ERROR           |2005       |Bad value for dkhm:orderconfirmationToken: >%s<                       |Supplied order confirmation token contains invalid characters                                                           |
|EPP_PARAMETER_RANGE_ERROR            |2004       |dkhm:orderconfirmationToken: >%s< exceeds allowed threshold           |Supplied order confirmation token has expired                                                                           |

## Update domain

|Status                               |Status Code|Status Text                                                           |Description                                                                                                             |
|-------------------------------------|-----------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Unable to resolve nameserver.                                         |Could not find supplied nameserver                                                                                      |
|EPP_PARAMETER_MISSING_ERROR          |2003       |No nameservers supplied.                                              |No nameservers supplied                                                                                                 |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |No nameserver host info.                                              |No nameservers defined for domain                                                                                       |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |No NS info on host.                                                   |Could not find NS info for nameserver                                                                                   |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Too few hosts.                                                        |Number of nameservers supplied below lower limit                                                                        |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Too many hosts.                                                       |Number of nameservers supplied exceeds upper limit                                                                      |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Host is missing NS record when compared to other responses.           |Misconfigured nameserver                                                                                                |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Host has extra NS record when compared to other responses.            |Misconfigured nameserver                                                                                                |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |SOA Serial mismatch between responses.                                |Misconfigured nameserver                                                                                                |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |No nameservers found.                                                 |Could not find nameservers for domain                                                                                   |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Host has alternating SOA serial between queries.                      |Misconfigured nameserver                                                                                                |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |IP address missing in nameserver IP address response                  |Misconfigured nameserver                                                                                                |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Nameserver response contains non-public IP address                    |Misconfigured nameserver                                                                                                |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Failed to query nameservers. Timed out.                               |Could not query nameservers for domain                                                                                  |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Problem with nameserver setup: (%s) not found.                        |Misconfigured nameserver                                                                                                |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Problem with nameserver setup.                                        |Misconfigured nameserver                                                                                                |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Failed to query nameservers.                                          |Could not query nameservers for domain                                                                                  |
|EPP_PARAMETER_POLICY_ERROR           |2306       |Specifying contacts for registrar managed domains is not allowed      |For registgrar managed domains, contacts are assigned automatically                                                     |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Domain does not exist                                                 |Trying to update a non-existing domain                                                                                  |
|EPP_PARAMETER_POLICY_ERROR           |2306       |Remove "all" and specific keys not allowed in same operation.         |Illegal combination of parameters                                                                                       |
|EPP_PARAMETER_POLICY_ERROR           |2306       |Change of registrant cannot be combined with other changes            |Change registrant must be in separate request                                                                           |
|EPP_PROHIBITED_OPERATION             |2304       |Bad userid. User is in a registrar group                              |A user in a registrar group can't be assigned registrant                                                                |
|EPP_AUTHORIZATION_ERROR              |2201       |No privilege. You are not authorized to change registrant             |The user does not have privileges to change registrant                                                                  |
|EPP_COMMAND_USE_ERROR                |2002       |Command use error                                                     |Both pw and null defined for authInfo                                                                                   |
|EPP_AUTHORIZATION_ERROR              |2201       |No privilege. You are not authorized to set authinfo                  |The user does not have privileges to set authinfo                                                                       |
|EPP_AUTHORIZATION_ERROR              |2201       |No privilege. You are not authorized to unset authinfo                |The user does not have privileges to unset authinfo                                                                     |
|EPP_AUTHORIZATION_ERROR              |2201       |No privilege. You are not authorized to change auto_renew             |The user does not have privileges to change auto_renew                                                                  |
|EPP_AUTHORIZATION_ERROR              |2201       |No privilege. You are not authorized to restore domain                |The user does not have privileges to restore domain                                                                     |
|EPP_PARAMETER_POLICY_ERROR           |2306       |Parameter error. %s                                                   |A parameter did not meet specifications. See error message for details                                                  |
|EPP_PENDING_CODE                     |1001       |Command completed. Pending registrant accept                          |Operation OK. Waiting for registrant to accept operation                                                                |
|EPP_PENDING_CODE                     |1001       |Command completed. Pending registrant approval                        |Operation OK. Waiting for registrant to approve operation                                                               |
|EPP_COMMAND_SYNTAX_ERROR             |2001       |Command syntax error                                                  |For Authinfo token, use domain:null or domain:pw with a value of autoredel or autotransfer                              |
|EPP_AUTHORIZATION_ERROR              |2201       |No privilege.                                                         |User is not authorized for requested operation                                                                          |
|EPP_AUTHORIZATION_ERROR              |2201       |Missing privilege                                                     |User is not authorized for requested operation                                                                          |
|EPP_PENDING_CODE                     |1001       |Command completed successfully; action pending                        |Waiting for VID contact to approve                                                                                      |
|EPP_PARAMETER_POLICY_ERROR           |2306       |Role not permitted                                                    |The F and B roles may not be removed                                                                                    |
|EPP_AUTHORIZATION_ERROR              |2201       |Repeated remove                                                       |The same role is specified more than once                                                                               |
|EPP_UNIMPLEMENTED_OBJECT_SERVICE     |2307       |Unimplemented object service                                          |The requested action is not implemented                                                                                 |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |IP not supported for nameserver remove                                |Removing nameserver by specifying IP address instead of hostname is not supported                                       |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Host not found: %s                                                    |The host to be removed is not found                                                                                     |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Host / domain relation not found                                      |The host to be removed is not listed as nameserver for domain in request                                                |
|EPP_PARAMETER_RANGE_ERROR            |2004       |Invalid or inactive nameserver: %s                                    |The host to be added as nameserver is registered as invalid or inactive                                                 |
|EPP_PARAMETER_RANGE_ERROR            |2004       |To few active hosts                                                   |To few of the hosts to be added as nameserver is registered as active                                                   |
|EPP_OBJECT_EXISTS_ERROR              |2302       |Host / domain relation already exists                                 |The host to be added as nameserver is already registered as nameserver for domain in request                            |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |IP not supported for nameserver add                                   |Adding nameserver by specifying IP address instead of hostname is not supported                                         |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Gluerecords required on new nameserver                                |Gluerecords are required for redelegation                                                                               |
|EPP_PARAMETER_POLICY_ERROR           |2306       |Nameserver changes required when specifying authinfo token            |If an authinfo token is specified, at least one nameserver change must be specified                                     |
|EPP_PARAMETER_MISSING_ERROR          |2003       |Value required for authinfo.pw                                        |Authinfo.pw is not specified                                                                                            |
|EPP_PARAMETER_RANGE_ERROR            |2004       |No privilege. You are not NSA for all nameservers (old and new)       |User must be NSA for all nameservers specified in request                                                               |
|EPP_AUTHORIZATION_ERROR              |2201       |Nameserver Change failed                                              |Change nameservers failed dur to missing privileges                                                                     |
|EPP_COMMAND_FAILED                   |2400       |Server Error                                                          |Internal error                                                                                                          |
|EPP_COMMAND_FAILED                   |2400       |Command failed                                                        |Internal error                                                                                                          |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |DS set not found                                                      |DS record to remove is not found                                                                                        |
|EPP_UNIMPLEMENTED_OBJECT_SERVICE     |2307       |maxSigLife not supported                                              |secDNS:maxSigLife parameter is not supported                                                                            |
|EPP_PARAMETER_RANGE_ERROR            |2004       |Bad digest length                                                     |The length of the specified digest is not the length that is expected for that digest type                              |
|EPP_OBJECT_EXISTS_ERROR              |2302       |DS set already exists                                                 |DS record to add already exists                                                                                         |
|EPP_BILLING_FAILURE                  |2104       |Billing failure                                                       |This operation requires a prepaid account                                                                               |
|EPP_DATA_MANAGEMENT_POLICY_VIOLATION |2308       |Multiple operations perfomed in one request: (%s)                     |The client attempted to perform multiple operations on an object in the same request                                    |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |No DS records for domain                                              |Attempt to delete DS records for a domain without DS records assigned                                                   |
|EPP_PROHIBITED_OPERATION             |2304       |Domain status prohibits operation                                     |The current status of the domain prohibits deleting DS records                                                          |


## Info domain

|Status                               |Status Code|Status Text                                                           |Description                                                                                                             |
|-------------------------------------|-----------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Domain not found                                                      |The supplied domain does not exist                                                                                      |

## Delete domain

|Status                               |Status Code|Status Text                                                           |Description                                                                                                             |
|-------------------------------------|-----------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_PROHIBITED_OPERATION             |2304       |Domain delete already pending.                                        |Domain has already been marked for deletion                                                                             |
|EPP_PARAMETER_POLICY_ERROR           |2306       |Parameter value policy error                                          |Delete date rejected. Must not be today or later than paid until date                                                   |
|EPP_PENDING_CODE                     |1001       |Command completed successfully; action pending                        |Pending deletion                                                                                                        |

## Renew domain

|Status                               |Status Code|Status Text                                                           |Description                                                                                                             |
|-------------------------------------|-----------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_PARAMETER_SYNTAX_ERROR           |2005       |The only supported unit is y                                          |The only supported unit for new periods is y                                                                            |
|EPP_PARAMETER_SYNTAX_ERROR           |2005       |Invalid period %s. Must be within range                               |The supplied period in request is not within range of valid periods                                                     |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Domain not found                                                      |Attempt to renew a nonexisting domain                                                                                   |
|EPP_AUTHORIZATION_ERROR              |2201       |You do not have the privilege required to renew this domain           |User must be payer for domain to renew                                                                                  |
|EPP_PARAMETER_POLICY_ERROR           |2306       |Given curExpDate %s does not match current expire date: %s            |The specified expiredate does not match domains expiredate                                                              |
|EPP_PARAMETER_POLICY_ERROR           |2306       |New expire date %s exceeds max %s                                     |The new expire date exceeds max expire date for domain                                                                  |
|EPP_NOT_ELIGIBLE_FOR_RENEWAL         |2105       |Domain is not in a state where it can be renewed                      |Domain must have a subscription id, not be blocked, active, paid, not expired                                           |
|EPP_COMMAND_FAILED                   |2400       |Internal error. Domain cannot be renewed.                             |Internal error trying to renew domain                                                                                   |

## Transfer domain

|Status                               |Status Code|Status Text                                                           |Description                                                           |
|-------------------------------------|-----------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Cannot change registrar on non-existent domain                        |The domain to transfer does not exist                                                                                   |
|EPP_AUTHORIZATION_ERROR              |2201       |No privilege. You are not authorized to transfer domain               |The user does not have sufficient privileges to transfer this domain                                                    |
|EPP_PARAMETER_POLICY_ERROR           |2306       |Object is not registrar managed.                                      |Registrant cannot withdraw from registrant handled domains                                                              |
|EPP_NOT_ELIGIBLE_FOR_TRANSFER        |2106       |Object is not ready for transfer at this time                         |The domain has not been assigned a subscription id yet. Please try again later                                          |
|EPP_NOT_ELIGIBLE_FOR_TRANSFER        |2106       |Object is not eligible for transfer, payment status prohibits transfer|The domain must be paid                                                                                                 |
|EPP_NOT_ELIGIBLE_FOR_TRANSFER        |2106       |Object is not eligible for transfer, domain status prohibits transfer  |The domain is blocked |Yes, there is a typo in the error message                                                        |
|EPP_NOT_ELIGIBLE_FOR_TRANSFER        |2106       |Object is not eligible for transfer, domain status prohibits transfer |The new registrar has not been assigned an account number yet. Please try again later                                   |
|EPP_AUTHORIZATION_ERROR              |2201       |Object is not eligible for transfer, no valid authinfo token          |There must be a valid authorization token                                                                               |
|EPP_AUTHORIZATION_ERROR              |2201       |Object is not eligible for transfer, registrar not allowed to request registrar change |Registrant not allowed to request registrar change                                                     |
|EPP_ASSOCIATION_PROHIBIT_OPERATION   |2305       |Not all domains in list have same registrant                          |All domains being transfered in the same operation must belong to the same registrant                                   |
|EPP_AUTHORIZATION_ERROR              |2201       |No privilege to become registrar                                      |The user must be an active registrar                                                                                    |
|EPP_AUTHORIZATION_ERROR              |2201       |Change of registrar without authinfo token is not currently allowed   |The transistion period where transfer without authinfo token is possible has ended                                      |
|EPP_PARAMETER_POLICY_ERROR           |2306       |Registrar group membership is required for this operation             |User must be member of a registrar group to perform a transfer                                                          |
|EPP_NOT_ELIGIBLE_FOR_TRANSFER        |2106       |Domain has not yet been assigned a subscription id                    |Subscription ID has not been assigned yet. Please try again later                                                       |
|EPP_NOT_ELIGIBLE_FOR_TRANSFER        |2106       |Domain has unpaid invoices                                            |All invoices must be paid before transfer                                                                               |
|EPP_NOT_ELIGIBLE_FOR_TRANSFER        |2106       |Domain is locked                                                      |A locked domain can't be transferred                                                                                    |
|EPP_NOT_ELIGIBLE_FOR_TRANSFER        |2106       |Domain is blocked                                                     |A blocked domain can't be transferred                                                                                   |
|EPP_NOT_ELIGIBLE_FOR_TRANSFER        |2106       |Registrant has not been id validated                                  |Retry transfer after registrant has performed id validation                                                             |
|EPP_AUTHORIZATION_ERROR              |2201       |Cannot change registrar for domain. Missing privilege                 |User does not have privilege to change registrar                                                                        |
|EPP_OBJECT_EXISTS                    |2302       |Domain is already registrar managed.                                  |Transfer without token in the transistion period can only be done for registrant handled domains                        |
|EPP_ASSOCIATION_PROHIBIT_OPERATION   |2305       |No NSA for domain is member of this registrar group                   |If transfer is performed without authorization token in the transistion period, the NSA must be in the registrar group  |
|EPP_INVALID_AUTHORIZATION_INFORMATION|2202       |Supplied authinfo token is not valid for this domain/operation        |Supplied authinfo token is invalid                                                                                      |
|EPP_AUTHORIZATION_ERROR              |2201       |Domain transfer failed                                                |If transfer is performed without authorization token in the transistion period, the NSA must be in the registrar group  |
|EPP_COMMAND_FAILED_FATAL             |2500       |Error occoured taking registrar role                                  |An internal error occoured transferring the domain                                                                      |
|EPP_ASSOCIATION_PROHIBIT_OPERATION   |2305       |Object is not handled by current user                                 |User can't withdraw from a domain that is handled by another user                                                       |

## Create host

|Status                               |Status Code|Status Text                                                           |Description                                                                                                             |
|-------------------------------------|-----------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_PARAMETER_POLICY_ERROR           |2306       |Registrar handle not permitted as NSA                                 |A registrar handle can't be NSA                                                                                         |
|EPP_OBJECT_EXISTS_ERROR              |2302       |Host name already exists                                              |Attempt to create a nameserver that is already created                                                                  |
|EPP_OBJECT_EXISTS_ERROR              |2302       |Host already exists                                                   |Attempt to create a nameserver that is already created                                                                  |
|EPP_PARAMETER_SYNTAX_ERROR           |2005       |Invalid host name.                                                    |The name specified for the nameserver is not a valid host name                                                          |
|EPP_PARAMETER_SYNTAX_ERROR           |2005       |Invalid TLD in host name.                                             |The top-level-domain name for the specified host name is invalid                                                        |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Domain does not exist                                                 |The domainname for the nameserver does not exist                                                                        |
|EPP_PARAMETER_POLICY_ERROR           |2306       |NSA not in registrar group                                            |The specified NSA for the new nameserver must be in the registrar group                                                 |
|EPP_AUTHORIZATION_ERROR              |2201       |No privilege                                                          |The user is not authorized to create nameservers                                                                        |
|EPP_PENDING_CODE                     |1001       |Command completed. Pending registrant approval.                       |Nameserver registered, pending registrar approval                                                                       |
|EPP_PENDING_CODE                     |1001       |Command completed. Pending approval by nameserver contact             |Nameserver registered, pending nameserver contact approval                                                              |

## Update host

|Status                               |Status Code|Status Text                                                           |Description                                                                                                             |
|-------------------------------------|-----------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_PARAMETER_POLICY_ERROR           |2306       |REG handle not permitted for requestedNsAdmin                         |A registrar handle can't be NSA                                                                                         |
|EPP_OBJECT_NOT_EXISTS_ERROR          |2303       |Unknown host                                                          |The nameserver to update does not exist                                                                                 |
|EPP_OBJECT_EXISTS_ERROR              |2302       |Host already exists                                                   |Attempt to rename a nameserver to a new name that is already in use                                                     |
|EPP_AUTHORIZATION_ERROR              |2201       |No privilege                                                          |The user does not have privilege to add/remove ip address or change zone contact                                        |
|EPP_PROHIBITED_OPERATION             |2304       |Not enough ip addresses after change. Minimum one needed.             |A nameserver needs at least 1 IP address                                                                                |
|EPP_UNIMPLEMENTED_SERVICE            |2307       |Unimplemented command                                                 |Changing hostname for a nameserver is not implemented                                                                   |
|EPP_PARAMETER_RANGE_ERROR            |2004       |Host IP address already exists                                        |The assigned IP addres is alreadu in use                                                                                |
|EPP_PENDING_CODE                     |1001       |Command completed successfully; action pending                        |Update registered, pending NSA accept                                                                                   |
|EPP_DATA_MANAGEMENT_POLICY_VIOLATION |2308       |Multiple operations perfomed in one request: (%s)                     |The client attempted to perform multiple operations on an object in the same request                                    |

## Info host

|Status                               |Status Code|Status Text                                                           |Description                                                                                                             |
|-------------------------------------|-----------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_OBJECT_NOT_EXISTS_ERROR          |2302       |Host not found                                                        |Attempt to request info on non-existing nameserver                                                                      |

## Delete host

|Status                               |Status Code|Status Text                                                           |Description                                                                                                             |
|-------------------------------------|-----------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_OBJECT_NOT_EXISTS_ERROR          |2302       |Host does not exist                                                   |Attempt to delete non-existing nameserver                                                                               |
|EPP_ASSOCIATION_PROHIBIT_OPERATION   |2305       |Nameserver has active domains, cannot be deleted                      |A nameserver that is in use can't be deleted                                                                            |
|EPP_AUTHORIZATION_ERROR              |2201       |No privilege                                                          |The user is not allowed to delete this nameserver                                                                       |

## Info balance

|Status                               |Status Code|Status Text                                                           |Description                                                                                                             |
|-------------------------------------|-----------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_OBJECT_NOT_EXISTS_ERROR          |2302       |Contact not found                                                     |The contact that balance is requested for does not exist                                                                |
|EPP_AUTHORIZATION_ERROR              |2201       |Unauthorized                                                          |The user is not authorized to view the balance for the requested user                                                   |

## Login

|Status                               |Status Code|Status Text                                                           |Description                                                                                                             |
|-------------------------------------|-----------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_COMMAND_FAILED                   |2400       |Internal error: IP address missing                                    |EPP server could not determine client ip address                                                                        |
|EPP_AUTHORIZATION_ERROR              |2201       |No privilege                                                          |You are not allowed to change password for other users                                                                  |
|EPP_PARAMETER_MISSING_ERROR          |2003       |Please specify login user and password.                               |Username and password must be supplied for login                                                                        |
|EPP_COMMAND_USE_ERROR                |2002       |Repeated logins are not permitted.                                    |User is already logged in                                                                                               |
|EPP_AUTHENTICATION_ERROR             |2200       |Login failed                                                          |Invalid username or password                                                                                            |

## Logout

|Status                               |Status Code|Status Text                                                           |Description                                                                                                             |
|-------------------------------------|-----------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_SUCCESS_DISCONNECT               |1500       |Not logged in, or already logged out                                  |User is not logged in                                                                                                   |
|EPP_SUCCESS_DISCONNECT               |1500       |                                                                      |User logged out                                                                                                         |

## Poll

|Status                               |Status Code|Status Text                                                           |Description                                                                                                             |
|-------------------------------------|-----------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|EPP_OBJECT_NOT_EXISTS_ERROR          |2302       |Object does not exist                                                 |The ack'ed message id does not exist                                                                                    |
|EPP_SUCCESS_ACK_DEQUEUE              |1301       |Command completed successfully; ack to dequeue                        |Message fetched successfully. Send ACK to dequeue message                                                               |
|EPP_SUCCESS_NO_MSG                   |1300       |Command completed successfully; no messages                           |There are no messages in the queue                                                                                      |
