# EPP Error Code Reference Guide

## General status messages

|Status                  |Status Code|Status Text|Description|
|------------------------|-----------|-----------|-----------|
|EPP_SUCCESS_CODE        |1000       |Command completed successfully|Command completed and requested data created/updated/deleted|
|EPP_COMMAND_FAILED      |2400       |Command did not return a status code|Internal error|
|EPP_COMMAND_FAILED      |2400       |Internal error.|Unhandled internal error|
|EPP_UNIMPLEMENTED       |2101       |Unimplemented|The requested command has not been implemented|
|EPP_COMMAND_FAILED_FATAL|2500       |Internal error. ECDS operation not found in the configuration|EPP server misconfiguration|
|EPP_COMMAND_FAILED      |2400       |Internal error. No connection to ECDS service.|No connection from EPP server to ECDS|
|EPP_COMMAND_FAILED_FATAL|2500       |Internal error. Can't get prices from ECDS.|Error in response from ECDS|
|EPP_BILLING_FAILURE     |2104       |Insufficient credit. Domain cannot be created.|Cannot create domains without sufficient credit on prepaid account|
|EPP_BILLING_FAILURE     |2104       |Insufficient credit. Domain cannot be renewed.|Cannot renew domains without sufficient credit on prepaid account|
|EPP_UNKNOWN_COMMAND     |2000       |No command specified|No or unknown command specified in EPP request|
|EPP_COMMAND_SYNTAX_ERROR|2001       |Bad command name|Command name specified contains invalid characters|
|EPP_UNKNOWN_COMMAND     |2000       |Unknown command|No or unknown command specified in EPP request|
|EPP_COMMAND_FAILED      |2400       |Command does not match endpoint or command data is missing|hello and logout have empty command tags. Other commands must have command tags|
|EPP_COMMAND_USE_ERROR   |2002       |You need to login first.|Log in before issuing other commands|
