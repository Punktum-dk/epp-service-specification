# Poll Message Reference Guide

This is a complete list of all EPP poll messages currently available in production in the Punktum dk EPP service.

## Document History
- 2026-09-08 Added three new poll messages regarding the suspension of a domain name
    - Added an XML example for each of the new messages

- 2026-08-03 Added an XML example for each poll message
    - Added a Placeholders section describing the placeholders used across the messages

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

| Placeholder     | Description                                              | Value(s)                                                  |
| --------------- | ------------------------------------------------------- | --------------------------------------------------------- |
| `%.dk`          | A domain name                                            | Any .dk domain name                                       |
| `%host%`        | A name server hostname                                  | Any hostname                                              |
| `%-DK`          | A contact handle                                        | Any contact handle                                        |
| `REG-%`         | A registrar handle                                      | Any registrar handle                                      |
| `%handle%`      | A handle that may be either a contact or registrar handle | Any contact or registrar handle                         |
| `%email%`       | An email address                                        | Any email address                                         |
| `%responsible%` | The party responsible for validating                    | `registry` or `registrar`                                 |
| `%reason%`      | The reason an email delivery failed                     | `antispam`, `bad_checksum`, `config`, `failed`, `unknown` |

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
|domain |create |The application for %.dk has been cancelled |[view](#ex-29) |domain:panData |
|domain |update |%.dk has been activated |[view](#ex-30) |domain:infData |
|domain |update |%.dk has been updated |[view](#ex-31) |domain:infData |
|domain |update billing |REG-% has been removed as billing contact for %.dk |[view](#ex-32) |domain:infData |
|domain |update dsrecords |DS records has been changed for %.dk |[view](#ex-33) |domain:infData |
|domain |update name servers |Name servers has been changed for %.dk, from %host%, %host%, … to %host%, %host%, … |[view](#ex-34) |domain:infData |
|domain |update registrant |The registrant has been changed to %-DK for %.dk |[view](#ex-35) |domain:panData |
|domain |update registrant |The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was not completed in time |[view](#ex-36) |domain:panData |
|domain |update registrant |The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was rejected |[view](#ex-37) |domain:panData |
|domain |update registrant |The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was not completed |[view](#ex-38) |domain:panData |
|domain |transfer |%.dk has been added to your portfolio |[view](#ex-39) |domain:trnData |
|domain |transfer |%.dk has been removed from your portfolio |[view](#ex-40) |domain:trnData |
|domain |suspend |%.dk has been suspended, as the registrant has not completed the ID/data check on time |[view](#ex-56) |domain:infData |
|domain |suspend |%.dk has been suspended, as the domain was cancelled |[view](#ex-57) |domain:infData |
|domain |suspend |%.dk has been suspended, as the domain was set to auto expire |[view](#ex-58) |domain:infData |
|domain |delete |%.dk has been deleted |[view](#ex-41) |domain:panData |
|domain |delete |%.dk has been deleted |[view](#ex-42) |domain:infData |
|domain |delete |%.dk has been extended and cancellation stopped |[view](#ex-43) |domain:panData |
|domain |delete |%.dk has been restored, extended and cancellation stopped |[view](#ex-44) |domain:panData |
|host |create |The name server %host% has been registered, as the registrant has approved it |[view](#ex-45) |host:panData |
|host |create |The name server %host% has not been registered, as it has been rejected by the registrant |[view](#ex-46) |host:panData |
|host |create |The name server %host% has not been registered, as it was not approved by the registrant in time |[view](#ex-47) |host:panData |
|host |create |The name server %host% has been registered, as the registrant has approved it and %handle% has accepted the name server manager role |[view](#ex-48) |host:panData |
|host |create |The name server %host% has not been registered, as the name server manager role has been rejected |[view](#ex-49) |host:panData |
|host |create |The name server %host% has not been registered, as the name server manager role was not accepted in time |[view](#ex-50) |host:panData |
|host |create |The name server %host% has been registered, as %handle% has accepted the name server manager role |[view](#ex-51) |host:panData |
|host |update |The name server manager role for %host% has been accepted by %handle% |[view](#ex-52) |host:panData |
|host |update |The name server manager role for %host% has been rejected by %handle% |[view](#ex-53) |host:panData |
|host |update |The name server manager role for %host% has not been accepted by %handle% in time |[view](#ex-54) |host:panData |
|host |delete |The name server %host% has been deleted |[view](#ex-55) |host:infData |

## Superseded Messages

The following poll messages were reworded on 2026-03-11. The table maps the previous wording to the current message.

|Old message (pre 2026-03-11) |Current message |Example |
|-----------------------------|----------------|--------|
|%.dk has been transferred to new registrant %-DK |The registrant has been changed to %-DK for %.dk |[view](#ex-35) |
|The transfer of %.dk to the new registrant %-DK has been cancelled, as the mandatory ID check was not completed in time |The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was not completed in time |[view](#ex-36) |
|The transfer of %.dk to the new registrant %-DK has been cancelled, as the mandatory ID check was rejected |The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was rejected |[view](#ex-37) |
|The transfer of %.dk to the new registrant %-DK has been cancelled, as the mandatory ID check was not completed |The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was not completed |[view](#ex-38) |

## Examples

Each poll message below has a collapsible XML example. Click **Show XML example** to expand it.

### contact

<a id="ex-1"></a>
**Operation:** update  
**Message:** The contact information has been updated for %-DK  
**ResData type:** `contact:infData`  
**Trigger:** The contact information on a handle has been updated

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>The contact information has been updated for DKHM1-DK</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>vitester@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666-DK</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>0
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id status="pending" responsible="registry" expdate="2026-04-10T21:59:59.0Z" >false</dkhm:verified_id>
				<dkhm:verified_email status="pending" responsible="registry" expdate="2026-04-10T21:59:59.0Z" >false</dkhm:verified_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-2"></a>
**Operation:** update primary email  
**Message:** %-DK has to confirm the new primary email, %email%, to complete the update - %responsible%  
**ResData type:** `contact:infData`  
**Trigger:** The registrant must confirm a new primary email address before the change takes effect.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>DKHM1-DK has to confirm the new primary email, registrar@punktum.dk, to complete the update - registry</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>vitester@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>0
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id status="pending" responsible="registry" expdate="2026-04-10T21:59:59.0Z" >false</dkhm:verified_id>
				<dkhm:verified_email status="pending" responsible="registry" expdate="2026-04-10T21:59:59.0Z" >false</dkhm:verified_email>
                <dkhm:confirm_email expdate="2026-03-12T22:59:59.0Z" responsible="registry" status="pending">registrar@punktum.dk</dkhm:confirm_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-3"></a>
**Operation:** update primary email  
**Message:** %-DK has confirmed the new primary email, %email% - %responsible%  
**ResData type:** `contact:panData`  
**Trigger:** The registrant has confirmed the new primary email address.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="452" id="6816761">
			<qDate>2026-01-07T19:51:30.0Z</qDate>
			<msg>DKHM1-DK has confirmed the new primary email, test123@punktum.dk - registry</msg>
		</msgQ>
		<resData>
			<contact:panData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id paResult="1">DKHM1-DK</contact:id>
				<contact:paTRID>
					<clTRID>55229dce-3409-4e2e-be58-c2132ac071d6</clTRID>
					<svTRID>E896C620-EC01-11F0-A321-AC9246E169D7</svTRID>
				</contact:paTRID>
				<contact:paDate>2026-01-07T19:45:30.0Z</contact:paDate>
			</contact:panData>
		</resData>
		<trID>
			<clTRID>f753cd75d4144beff4e820447dbde993</clTRID>
			<svTRID>47CEF78F-2033-3EE9-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-4"></a>
**Operation:** update primary email  
**Message:** The new primary email, %email%, was not confirmed for %-DK - %responsible%  
**ResData type:** `contact:panData`  
**Trigger:** The registrant did not confirm the new primary email address. The primary email is not changed.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="42" id="6118763">
			<qDate>2026-01-09T19:51:30.0Z</qDate>
			<msg>The new primary email, test@punktum.dk, was not confirmed for DKHM1-DK - registry</msg>
		</msgQ>
		<resData>
			<contact:panData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id paResult="0">DKHM1-DK</contact:id>
				<contact:paTRID>
					<clTRID>55229dce-3409-4e2e-be58-c2132ac071d6</clTRID>
					<svTRID>E896C620-EC01-11F0-A321-AC9246E169D7</svTRID>
				</contact:paTRID>
				<contact:paDate>2026-01-09T19:45:30.0Z</contact:paDate>
			</contact:panData>
		</resData>
		<trID>
			<clTRID>f753cd75d4144beff4e820447dbde993</clTRID>
			<svTRID>47CEF78F-2033-3EE9-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-5"></a>
**Operation:** update secondary email  
**Message:** %-DK has to confirm new secondary email, %email%, to complete the update - %responsible%  
**ResData type:** `contact:infData`  
**Trigger:** The registrant must confirm a new secondary email address before the change takes effect.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>DKHM1-DK has to confirm new secondary email, registrar@punktum.dk, to complete the update - registry</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>vitester@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id status="completed" >true</dkhm:verified_id>
				<dkhm:verified_email  status="completed" >true</dkhm:verified_email>
                <dkhm:confirm_secondary_email expdate="2026-03-13T22:59:59.0Z" responsible="registry" status="pending">registrar@punktum.dk</dkhm:confirm_secondary_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-6"></a>
**Operation:** update secondary email  
**Message:** %-DK has confirmed the new secondary email, %email% - %responsible%  
**ResData type:** `contact:panData`  
**Trigger:** The registrant has confirmed the new secondary email address.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="259" id="6809010">
			<qDate>2026-04-20T18:48:45.0Z</qDate>
			<msg>DKHM1-DK has confirmed the new secondary email, registrar@punktum.dk - registry</msg>
		</msgQ>
		<resData>
			<contact:panData xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id paResult="1">DKHM1-DK</contact:id>
				<contact:paTRID>
					<clTRID>8072e041-c1cd-479e-b98e-22686ea78228</clTRID>
					<svTRID>A7940DBE-8F21-11F1-B390-05474682B364</svTRID>
				</contact:paTRID>
				<contact:paDate>2026-04-20T18:48:45.0Z</contact:paDate>
			</contact:panData>
		</resData>
		<trID>
			<clTRID>8807a738592149ff9e2c6d9ef89ecefc</clTRID>
			<svTRID>E9D94BFC-31B6-BD9E-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-7"></a>
**Operation:** update secondary email  
**Message:** The new secondary email, %email%, was not confirmed for %-DK - %responsible%  
**ResData type:** `contact:panData`  
**Trigger:** The registrant did not confirm the new secondary email address. Secondary email has not been updated/added.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="228" id="6806477">
			<qDate>2026-02-27T11:19:38.0Z</qDate>
			<msg>The new secondary email, registrar@punktum.dk, was not confirmed for DKHM1-DK - registry</msg>
		</msgQ>
		<resData>
			<contact:panData xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id paResult="0">DKHM1-DK</contact:id>
				<contact:paTRID>
					<clTRID>1c71b636-bc40-470c-9756-62d2c6e8f204</clTRID>
					<svTRID>A7940F07-8F21-11F1-86FE-05474682B364</svTRID>
				</contact:paTRID>
				<contact:paDate>2026-02-27T11:19:38.0Z</contact:paDate>
			</contact:panData>
		</resData>
		<trID>
			<clTRID>294167a5dfd0499f8545f9247e731344</clTRID>
			<svTRID>CF176383-F163-2EB8-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-8"></a>
**Operation:** update verification  
**Message:** %-DK has to complete the mandatory ID and data check - %responsible%  
**ResData type:** `contact:infData`  
**Trigger:** The registrant must complete both the mandatory ID check and email verification.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>DKHM1-DK has to complete the mandatory ID and data check - registry</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>vitester@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>0
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id status="pending" responsible="registry" expdate="2026-04-10T21:59:59.0Z" >false</dkhm:verified_id>
				<dkhm:verified_email status="pending" responsible="registry" expdate="2026-04-10T21:59:59.0Z" >false</dkhm:verified_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-9"></a>
**Operation:** update verification  
**Message:** %-DK has to complete the mandatory ID check - %responsible%  
**ResData type:** `contact:infData`  
**Trigger:** The registrant must complete the mandatory ID check only.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>DKHM1-DK has to complete the mandatory ID check - registry</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>vitester@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>0
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id status="pending" responsible="registry" expdate="2026-04-10T21:59:59.0Z" >false</dkhm:verified_id>
				<dkhm:verified_email status="completed" >true</dkhm:verified_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-10"></a>
**Operation:** update verification  
**Message:** %-DK has to complete the mandatory data check - %responsible%  
**ResData type:** `contact:infData`  
**Trigger:** The registrant must complete the mandatory email verification only.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>DKHM1-DK has to complete the mandatory data check - registry</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>vitester@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>0
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id  status="notRequired" >false</dkhm:verified_id>
				<dkhm:verified_email status="pending" responsible="registry" expdate="2026-04-10T21:59:59.0Z" >false</dkhm:verified_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-11"></a>
**Operation:** update verification  
**Message:** The mandatory ID and data check of %-DK has expired - %responsible%  
**ResData type:** `contact:infData`  
**Trigger:** The registrant did not complete the mandatory ID check and email verification within the deadline; the associated domain name(s) are suspended.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>The mandatory ID and data check of DKHM1-DK has expired - registry</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>vitester@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>0
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id  status="expired" >false</dkhm:verified_id>
				<dkhm:verified_email status="expired" >false</dkhm:verified_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-12"></a>
**Operation:** update verification  
**Message:** The mandatory ID check of %-DK has expired - %responsible%  
**ResData type:** `contact:infData`  
**Trigger:** The registrant did not complete the mandatory ID check within the deadline; the associated domain name(s) are suspended.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>The mandatory ID check of DKHM1-DK has expired - registry</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>vitester@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>0
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id  status="expired" >false</dkhm:verified_id>
				<dkhm:verified_email status="completed" >true</dkhm:verified_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-13"></a>
**Operation:** update verification  
**Message:** The mandatory data check of %-DK has expired - %responsible%  
**ResData type:** `contact:infData`  
**Trigger:** The registrant did not complete the mandatory email verification within the deadline; the associated domain name(s) are suspended.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>The mandatory data check of DKHM1-DK has expired - registry</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>vitester@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>0
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id  status="notRequired" >false</dkhm:verified_id>
				<dkhm:verified_email status="expired" >false</dkhm:verified_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-14"></a>
**Operation:** update verification  
**Message:** %-DK has completed the mandatory ID and data check - %responsible%  
**ResData type:** `contact:infData`  
**Trigger:** The registrant completed the mandatory ID check and email verification; the domain name is activated if it was not already active.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>DKHM1-DK has completed the mandatory ID and data check - registry</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>vitester@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id  status="completed" >true</dkhm:verified_id>
				<dkhm:verified_email status="completed" >true</dkhm:verified_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-15"></a>
**Operation:** update verification  
**Message:** %-DK has completed the mandatory ID check - %responsible%  
**ResData type:** `contact:infData`  
**Trigger:** The registrant completed the mandatory ID check; the domain name is activated if it was not already active.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>DKHM1-DK has completed the mandatory ID check - registry</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>vitester@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id  status="completed" >true</dkhm:verified_id>
				<dkhm:verified_email status="completed" >true</dkhm:verified_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-16"></a>
**Operation:** update verification  
**Message:** %-DK has completed the mandatory data check - %responsible%  
**ResData type:** `contact:infData`  
**Trigger:** The registrant completed the mandatory email verification; an ID-control can still be active and needs completion before domain is activated

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>DKHM1-DK has completed the mandatory data check - registry</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>vitester@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id  status="notRequired" >false</dkhm:verified_id>
				<dkhm:verified_email status="completed" >true</dkhm:verified_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-17"></a>
**Operation:** update verification  
**Message:** The mandatory ID check of %-DK was rejected - %responsible%  
**ResData type:** `contact:infData`  
**Trigger:** The registrant's mandatory ID check was rejected; the associated domain name(s) are suspended.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>The mandatory ID check of DKHM1-DK was rejected - registry</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>vitester@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>0
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id  status="rejected" >false</dkhm:verified_id>
				<dkhm:verified_email status="completed" >true</dkhm:verified_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-18"></a>
**Operation:** update verification  
**Message:** The mandatory ID and data check of %-DK was cancelled - %responsible%  
**ResData type:** `contact:infData`  
**Trigger:** The mandatory ID check and email verification were cancelled; the registrant no longer needs to complete them, and the domain name is activated if it was not already active.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>The mandatory ID and data check of DKHM1-DK was cancelled - registry</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>vitester@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>0
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id  status="notRequired" >false</dkhm:verified_id>
				<dkhm:verified_email status="expired" >false</dkhm:verified_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-19"></a>
**Operation:** update verification  
**Message:** The mandatory ID check of %-DK was cancelled - %responsible%  
**ResData type:** `contact:infData`  
**Trigger:** The mandatory ID check was cancelled; the registrant no longer needs to complete it, and the domain name is activated if it was not already active.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>The mandatory ID check of DKHM1-DK was cancelled - registry</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>vitester@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>0
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id  status="notRequired" >false</dkhm:verified_id>
				<dkhm:verified_email status="completed" >true</dkhm:verified_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-20"></a>
**Operation:** update verification  
**Message:** The mandatory data check of %-DK was cancelled - %responsible%  
**ResData type:** `contact:infData`  
**Trigger:** The mandatory email verification was cancelled; the registrant no longer needs to complete it, and the domain name is activated if it was not already active.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>The mandatory data check of DKHM1-DK was cancelled - registry</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>vitester@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>0
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id  status="notRequired" >false</dkhm:verified_id>
				<dkhm:verified_email status="notRequired" >false</dkhm:verified_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-21"></a>
**Operation:** email bounce  
**Message:** Email delivery (failed) %reason% for the primary email, %email%, of %-DK. Please review and correct the email address.  
**ResData type:** `contact:infData`  
**Trigger:** The registrant's primary email address cannot receive mail and must be corrected.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>Email delivery (failed) failed for the primary email, registrar@punktum.dk, of DKHM1-DK. Please review and correct the email address.</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>registrar@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id  status="completed" >true</dkhm:verified_id>
				<dkhm:verified_email status="completed" >true</dkhm:verified_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-22"></a>
**Operation:** email bounce  
**Message:** Email delivery (failed) %reason% for the secondary email, %email%, of %-DK. Please review and correct the email address.  
**ResData type:** `contact:infData`  
**Trigger:** The registrant's secondary email address cannot receive mail and must be corrected.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>Email delivery (failed) antispam for the secondary email, example@punktum.dk, of DKHM1-DK. Please review and correct the email address.</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>registrar@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
            <dkhm:secondaryEmail
                xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-4.5">example@punktum.dk
            </dkhm:secondaryEmail>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id  status="completed" >true</dkhm:verified_id>
				<dkhm:verified_email status="completed" >true</dkhm:verified_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-23"></a>
**Operation:** delete  
**Message:** %-DK has been deleted  
**ResData type:** `contact:infData`  
**Trigger:** The user-id in the registrar's portfolio has been deleted, which happens automatically when the user-id has been left empty for 14 days.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="3" id="6824360">
			<qDate>2026-03-11T10:32:08.0Z</qDate>
			<msg>DKHM1-DK has been deleted</msg>
		</msgQ>
		<resData>
			<contact:infData
				xmlns:contact="urn:ietf:params:xml:ns:contact-1.0">
				<contact:id>DKHM1-DK</contact:id>
				<contact:roid>DKHM1-DK</contact:roid>
				<contact:status s="serverDeleteProhibited"/>
				<contact:status s="serverTransferProhibited"/>
				<contact:postalInfo type="loc">
					<contact:name>Punktum dk A/S</contact:name>
					<contact:addr>
						<contact:street>Ørestads Boulevard 108, 11.</contact:street>
						<contact:city>København S</contact:city>
						<contact:pc>2300</contact:pc>
						<contact:cc>DK</contact:cc>
					</contact:addr>
				</contact:postalInfo>
				<contact:voice>+45.33646060</contact:voice>
				<contact:email>registrar@punktum.dk</contact:email>
				<contact:clID>REG-666666</contact:clID>
				<contact:crID>REG-666666</contact:crID>
				<contact:crDate>2026-03-11T10:30:21.0Z</contact:crDate>
				<contact:upID>REG-666666</contact:upID>
				<contact:upDate>2026-03-11T10:32:08.0Z</contact:upDate>
			</contact:infData>
		</resData>
		<extension>
			<dkhm:contact_validated
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1
			</dkhm:contact_validated>
			<dkhm:mobilephone
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>+45.22777004
			</dkhm:mobilephone>
			<dkhm:CVR
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>24210375
			</dkhm:CVR>
			<dkhm:userType
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>company
			</dkhm:userType>
			<dkhm:sole_proprietorship
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false
			</dkhm:sole_proprietorship>
			<dkhm:contact_verification
				xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>
				<dkhm:responsible>registry</dkhm:responsible>
				<dkhm:verified_id  status="completed" >true</dkhm:verified_id>
				<dkhm:verified_email status="completed" >true</dkhm:verified_email>
			</dkhm:contact_verification>
		</extension>
		<trID>
			<clTRID>ABC-123</clTRID>
			<svTRID>4CBE2574-453F-A6D7-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

### domain

<a id="ex-24"></a>
**Operation:** create  
**Message:** %.dk has been registered and activated  
**ResData type:** `domain:panData`  
**Trigger:** The domain name has been registered and activated.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="66" id="6801533">
			<qDate>2026-01-15T03:05:44.0Z</qDate>
			<msg>test.dk has been registered and activated</msg>
		</msgQ>
		<resData>
			<domain:panData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name paResult="1">test.dk</domain:name>
				<domain:paTRID>
					<clTRID>e3d66d8d-279c-4c84-bbfd-ddbfe547e9c6</clTRID>
					<svTRID>A794105A-8F21-11F1-910D-05474682B364</svTRID>
				</domain:paTRID>
				<domain:paDate>2026-01-15T03:05:44.0Z</domain:paDate>
			</domain:panData>
		</resData>
		<trID>
			<clTRID>e9f93bca097d46358e86365bb92fe3ec</clTRID>
			<svTRID>16A9520A-E711-C100-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-25"></a>
**Operation:** create  
**Message:** %.dk has been registered, but not activated due to pending ID and/or data check  
**ResData type:** `domain:panData`  
**Trigger:** The domain name has been registered but is not activated until the registrant completes the mandatory email verification and/or ID check.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="149" id="6814731">
			<qDate>2026-02-06T12:28:51.0Z</qDate>
			<msg>test1.dk has been registered, but not activated due to pending ID and/or data check</msg>
		</msgQ>
		<resData>
			<domain:panData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name paResult="1">test1.dk</domain:name>
				<domain:paTRID>
					<clTRID>5f22daeb-f451-4ece-9952-4d1b5cf7f2e9</clTRID>
					<svTRID>A79411C4-8F21-11F1-81A0-05474682B364</svTRID>
				</domain:paTRID>
				<domain:paDate>2026-02-06T12:28:51.0Z</domain:paDate>
			</domain:panData>
		</resData>
		<trID>
			<clTRID>4ffc3b77c7a64f6cac3aac6521f49038</clTRID>
			<svTRID>4F6C7495-72BF-63AF-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-26"></a>
**Operation:** create  
**Message:** The application for %.dk has been rejected, as the domain was already taken  
**ResData type:** `domain:panData`  
**Trigger:** The domain name was not registered, as it was already taken.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="262" id="6816028">
			<qDate>2026-02-08T19:04:56.0Z</qDate>
			<msg>The application for example.dk has been rejected, as the domain was already taken</msg>
		</msgQ>
		<resData>
			<domain:panData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name paResult="0">example.dk</domain:name>
				<domain:paTRID>
					<clTRID>62162fc4-2e3f-44f7-be79-1995f663fd95</clTRID>
					<svTRID>A79412E2-8F21-11F1-8F8D-05474682B364</svTRID>
				</domain:paTRID>
				<domain:paDate>2026-02-08T19:04:56.0Z</domain:paDate>
			</domain:panData>
		</resData>
		<trID>
			<clTRID>5d133c1d15c94d188c2a245c7af0a8a7</clTRID>
			<svTRID>A572DD96-457C-C18C-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-29"></a>
**Operation:** create  
**Message:** The application for %.dk has been cancelled  
**ResData type:** `domain:panData`  
**Trigger:** The domain name application was cancelled. No further information is provided; contact Punktum dk to find out why.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="422" id="6814079">
			<qDate>2026-03-18T06:27:22.0Z</qDate>
			<msg>The application for domain.dk has been cancelled</msg>
		</msgQ>
		<resData>
			<domain:panData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name paResult="0">domain.dk</domain:name>
				<domain:paTRID>
					<clTRID>037cb349-7933-44b1-b717-71aabc157fd4</clTRID>
					<svTRID>A794181E-8F21-11F1-987A-05474682B364</svTRID>
				</domain:paTRID>
				<domain:paDate>2026-03-18T06:27:22.0Z</domain:paDate>
			</domain:panData>
		</resData>
		<trID>
			<clTRID>2ab4e1438da74713956c67e0f4b5a56b</clTRID>
			<svTRID>0ABF39DF-F9F4-FADD-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-30"></a>
**Operation:** update  
**Message:** %.dk has been activated  
**ResData type:** `domain:infData`  
**Trigger:** The domain name has been activated after having been suspended.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="422" id="6814079">
			<qDate>2026-03-18T06:27:22.0Z</qDate>
			<msg>test.dk has been activated</msg>
		</msgQ>
        <resData>
            <domain:infData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
                <domain:name>test.dk</domain:name>
                <domain:roid>TEST_DK-DK</domain:roid>
                <domain:status s="ok"/>
                <domain:registrant>DKHM1-DK</domain:registrant>
                <domain:ns>
                    <domain:hostObj>auth01.ns.dk-hostmaster.dk</domain:hostObj>
                    <domain:hostObj>auth02.ns.dk-hostmaster.dk</domain:hostObj>
                </domain:ns>
                <domain:clID>REG-666666</domain:clID>
                <domain:crDate>2026-03-18T06:27:22.0Z</domain:crDate>
                <domain:upDate>2026-03-18T06:27:22.0Z</domain:upDate>
                <domain:exDate>2027-03-18T21:59:59.0Z</domain:exDate>
            </domain:infData>
        </resData>
        <extension>
            <dkhm:registrant_validated xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1</dkhm:registrant_validated>
            <secDNS:infData xmlns:secDNS='urn:ietf:params:xml:ns:secDNS-1.1'>
                <secDNS:dsData>
                    <secDNS:keyTag>21836</secDNS:keyTag>
                    <secDNS:alg>8</secDNS:alg>
                    <secDNS:digestType>2</secDNS:digestType>
                    <secDNS:digest>3b3596534d1a0aa8a33cd8ac1deb8a239fe7baa8c31e4e390bcde5ca90ee6d22</secDNS:digest>
                </secDNS:dsData>
            </secDNS:infData>
            <dkhm:autoRenew xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>true</dkhm:autoRenew>
            <dkhm:vid xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false</dkhm:vid>
        </extension>
        <trID>
            <clTRID>ABC-123</clTRID>
            <svTRID>12D5E75C-8F29-11F1-B164-9E4826B23308</svTRID>
        </trID>
    </response>
</epp>
```

</details>

---

<a id="ex-31"></a>
**Operation:** update  
**Message:** %.dk has been updated  
**ResData type:** `domain:infData`  
**Trigger:** The domain name has been updated with regard to status, expiry date, etc.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="422" id="6814079">
			<qDate>2026-03-18T06:27:22.0Z</qDate>
			<msg>test.dk has been updated</msg>
		</msgQ>
        <resData>
            <domain:infData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
                <domain:name>test.dk</domain:name>
                <domain:roid>TEST_DK-DK</domain:roid>
                <domain:status s="ok"/>
                <domain:registrant>DKHM1-DK</domain:registrant>
                <domain:ns>
                    <domain:hostObj>ns1.punktum.dk</domain:hostObj>
                    <domain:hostObj>ns2.punktum.dk</domain:hostObj>
                </domain:ns>
                <domain:clID>REG-666666</domain:clID>
                <domain:crDate>2026-03-18T06:27:22.0Z</domain:crDate>
                <domain:upDate>2026-03-18T06:27:22.0Z</domain:upDate>
                <domain:exDate>2027-03-18T21:59:59.0Z</domain:exDate>
            </domain:infData>
        </resData>
        <extension>
            <dkhm:registrant_validated xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1</dkhm:registrant_validated>
            <secDNS:infData xmlns:secDNS='urn:ietf:params:xml:ns:secDNS-1.1'>
                <secDNS:dsData>
                    <secDNS:keyTag>21836</secDNS:keyTag>
                    <secDNS:alg>8</secDNS:alg>
                    <secDNS:digestType>2</secDNS:digestType>
                    <secDNS:digest>3b3596534d1a0aa8a33cd8ac1deb8a239fe7baa8c31e4e390bcde5ca90ee6d22</secDNS:digest>
                </secDNS:dsData>
            </secDNS:infData>
            <dkhm:autoRenew xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>true</dkhm:autoRenew>
            <dkhm:vid xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false</dkhm:vid>
        </extension>
        <trID>
            <clTRID>ABC-123</clTRID>
            <svTRID>12D5E75C-8F29-11F1-B164-9E4826B23308</svTRID>
        </trID>
    </response>
</epp>
```

</details>

---

<a id="ex-32"></a>
**Operation:** update billing  
**Message:** REG-% has been removed as billing contact for %.dk  
**ResData type:** `domain:infData`  
**Trigger:** The registrar has been removed as the billing contact for a domain name.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="422" id="6814079">
			<qDate>2026-03-18T06:27:22.0Z</qDate>
			<msg>REG-666666 has been removed as billing contact for test.dk</msg>
		</msgQ>
        <resData>
            <domain:infData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
                <domain:name>test.dk</domain:name>
                <domain:roid>TEST_DK-DK</domain:roid>
                <domain:status s="ok"/>
                <domain:registrant>DKHM1-DK</domain:registrant>
                <domain:ns>
                    <domain:hostObj>ns1.punktum.dk</domain:hostObj>
                    <domain:hostObj>ns2.punktum.dk</domain:hostObj>
                </domain:ns>
                <domain:clID>REG-666666</domain:clID>
                <domain:crDate>2026-03-18T06:27:22.0Z</domain:crDate>
                <domain:upDate>2026-03-18T06:27:22.0Z</domain:upDate>
                <domain:exDate>2027-03-18T21:59:59.0Z</domain:exDate>
            </domain:infData>
        </resData>
        <extension>
            <dkhm:registrant_validated xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1</dkhm:registrant_validated>
            <secDNS:infData xmlns:secDNS='urn:ietf:params:xml:ns:secDNS-1.1'>
                <secDNS:dsData>
                    <secDNS:keyTag>21836</secDNS:keyTag>
                    <secDNS:alg>8</secDNS:alg>
                    <secDNS:digestType>2</secDNS:digestType>
                    <secDNS:digest>3b3596534d1a0aa8a33cd8ac1deb8a239fe7baa8c31e4e390bcde5ca90ee6d22</secDNS:digest>
                </secDNS:dsData>
            </secDNS:infData>
            <dkhm:autoRenew xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>true</dkhm:autoRenew>
            <dkhm:vid xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false</dkhm:vid>
        </extension>
        <trID>
            <clTRID>ABC-123</clTRID>
            <svTRID>12D5E75C-8F29-11F1-B164-9E4826B23308</svTRID>
        </trID>
    </response>
</epp>
```

</details>

---

<a id="ex-33"></a>
**Operation:** update dsrecords  
**Message:** DS records has been changed for %.dk  
**ResData type:** `domain:infData`  
**Trigger:** The DS records for the domain name have been changed.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="422" id="6814079">
			<qDate>2026-03-18T06:27:22.0Z</qDate>
			<msg>DS records has been changed for test.dk</msg>
		</msgQ>
        <resData>
            <domain:infData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
                <domain:name>test.dk</domain:name>
                <domain:roid>TEST_DK-DK</domain:roid>
                <domain:status s="ok"/>
                <domain:registrant>DKHM1-DK</domain:registrant>
                <domain:ns>
                    <domain:hostObj>ns1.punktum.dk</domain:hostObj>
                    <domain:hostObj>ns2.punktum.dk</domain:hostObj>
                </domain:ns>
                <domain:clID>REG-666666</domain:clID>
                <domain:crDate>2026-03-18T06:27:22.0Z</domain:crDate>
                <domain:upDate>2026-03-18T06:27:22.0Z</domain:upDate>
                <domain:exDate>2027-03-18T21:59:59.0Z</domain:exDate>
            </domain:infData>
        </resData>
        <extension>
            <dkhm:registrant_validated xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1</dkhm:registrant_validated>
<secDNS:infData xmlns:secDNS='urn:ietf:params:xml:ns:secDNS-1.1'>
    <secDNS:dsData>
        <secDNS:keyTag>15110</secDNS:keyTag>
        <secDNS:alg>8</secDNS:alg>
        <secDNS:digestType>2</secDNS:digestType>
        <secDNS:digest>1ed24f9b3d41ad5e3e0e19ab90a6dc107da35b0f0384e947174842513a2b2db0</secDNS:digest>
    </secDNS:dsData>
    <secDNS:dsData>
        <secDNS:keyTag>29869</secDNS:keyTag>
        <secDNS:alg>13</secDNS:alg>
        <secDNS:digestType>2</secDNS:digestType>
        <secDNS:digest>823aeef5675616190e9348381e5736078e7129e6a410811f75b7ffa2f9eee75a</secDNS:digest>
    </secDNS:dsData>
</secDNS:infData>
            <dkhm:autoRenew xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>true</dkhm:autoRenew>
            <dkhm:vid xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false</dkhm:vid>
        </extension>
        <trID>
            <clTRID>ABC-123</clTRID>
            <svTRID>12D5E75C-8F29-11F1-B164-9E4826B23308</svTRID>
        </trID>
    </response>
</epp>
```

</details>

---

<a id="ex-34"></a>
**Operation:** update name servers  
**Message:** Name servers has been changed for %.dk, from %host%, %host%, … to %host%, %host%, …  
**ResData type:** `domain:infData`  
**Trigger:** The name servers for the domain name have been changed.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="422" id="6814079">
			<qDate>2026-03-18T06:27:22.0Z</qDate>
			<msg>Name servers has been changed for test.dk, from ns01.example.dk, ns02.example.dk to ns1.punktum.dk, ns2.punktum.dk</msg>
		</msgQ>
        <resData>
            <domain:infData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
                <domain:name>test.dk</domain:name>
                <domain:roid>TEST_DK-DK</domain:roid>
                <domain:status s="ok"/>
                <domain:registrant>DKHM1-DK</domain:registrant>
                <domain:ns>
                    <domain:hostObj>ns1.punktum.dk</domain:hostObj>
                    <domain:hostObj>ns2.punktum.dk</domain:hostObj>
                </domain:ns>
                <domain:clID>REG-666666</domain:clID>
                <domain:crDate>2026-03-18T06:27:22.0Z</domain:crDate>
                <domain:upDate>2026-03-18T06:27:22.0Z</domain:upDate>
                <domain:exDate>2027-03-18T21:59:59.0Z</domain:exDate>
            </domain:infData>
        </resData>
        <extension>
            <dkhm:registrant_validated xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1</dkhm:registrant_validated>
            <secDNS:infData xmlns:secDNS='urn:ietf:params:xml:ns:secDNS-1.1'>
                <secDNS:dsData>
                    <secDNS:keyTag>21836</secDNS:keyTag>
                    <secDNS:alg>8</secDNS:alg>
                    <secDNS:digestType>2</secDNS:digestType>
                    <secDNS:digest>3b3596534d1a0aa8a33cd8ac1deb8a239fe7baa8c31e4e390bcde5ca90ee6d22</secDNS:digest>
                </secDNS:dsData>
            </secDNS:infData>
            <dkhm:autoRenew xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>true</dkhm:autoRenew>
            <dkhm:vid xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false</dkhm:vid>
        </extension>
        <trID>
            <clTRID>ABC-123</clTRID>
            <svTRID>12D5E75C-8F29-11F1-B164-9E4826B23308</svTRID>
        </trID>
    </response>
</epp>
```

</details>

---

<a id="ex-35"></a>
**Operation:** change registrant  
**Message:** The registrant has been changed to %-DK for %.dk  
**ResData type:** `domain:panData`  
**Trigger:** Confirmation that the domain name has changed registrant.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="369" id="6809683">
			<qDate>2026-03-11T09:27:53.0Z</qDate>
			<msg>The registrant has been changed to DKHM1-DK for test.dk</msg>
		</msgQ>
		<resData>
			<domain:panData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name paResult="1">test.dk</domain:name>
				<domain:paTRID>
					<clTRID>d1817589-feb0-4f73-8612-94a95eb130b8</clTRID>
					<svTRID>A7941ABC-8F21-11F1-8E1C-05474682B364</svTRID>
				</domain:paTRID>
				<domain:paDate>2026-03-11T09:27:53.0Z</domain:paDate>
			</domain:panData>
		</resData>
		<trID>
			<clTRID>58969e8f885644e793730c420a8d9b98</clTRID>
			<svTRID>0F00577A-8A09-DD7D-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-36"></a>
**Operation:** change registrant   
**Message:** The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was not completed in time  
**ResData type:** `domain:panData`  
**Trigger:** The registrant change was not completed, as the mandatory ID check and/or email verification was not completed within the deadline.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="243" id="6824803">
			<qDate>2026-03-10T02:36:37.0Z</qDate>
			<msg>The registrant has not been changed to DKHM1-DK for test.dk, as the mandatory ID and/or data check was not completed in time</msg>
		</msgQ>
		<resData>
			<domain:panData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name paResult="0">test.dk</domain:name>
				<domain:paTRID>
					<clTRID>32c4326f-e09b-47c7-9dcb-41dd859701d3</clTRID>
					<svTRID>A7941C12-8F21-11F1-BB5D-05474682B364</svTRID>
				</domain:paTRID>
				<domain:paDate>2026-03-10T02:36:37.0Z</domain:paDate>
			</domain:panData>
		</resData>
		<trID>
			<clTRID>d9fd7f5c7c56470f8839d8ea571d109f</clTRID>
			<svTRID>39024B63-6A79-49C5-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-37"></a>
**Operation:** change registrant    
**Message:** The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was rejected  
**ResData type:** `domain:panData`  
**Trigger:** The registrant change was not completed, as the documentation for the mandatory ID check was rejected.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="434" id="6808350">
			<qDate>2026-02-28T06:07:03.0Z</qDate>
			<msg>The registrant has not been changed to DKHM1-DK for example.dk, as the mandatory ID and/or data check was rejected</msg>
		</msgQ>
		<resData>
			<domain:panData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name paResult="0">example.dk</domain:name>
				<domain:paTRID>
					<clTRID>64ca97bb-7873-4535-aabc-822f38fafe5a</clTRID>
					<svTRID>A7941D2F-8F21-11F1-9EE2-05474682B364</svTRID>
				</domain:paTRID>
				<domain:paDate>2026-02-28T06:07:03.0Z</domain:paDate>
			</domain:panData>
		</resData>
		<trID>
			<clTRID>797b7c5e12554a988b136931080c84ac</clTRID>
			<svTRID>61F0C015-CCC0-F2BB-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-38"></a>
**Operation:** change registrant   
**Message:** The registrant has not been changed to %-DK for %.dk, as the mandatory ID and/or data check was not completed  
**ResData type:** `domain:panData`  
**Trigger:** The registrant change was cancelled, as the new registrant has rejected the change.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="399" id="6823964">
			<qDate>2026-03-16T03:16:39.0Z</qDate>
			<msg>The registrant has not been changed to DKHM1-DK for punktum.dk, as the mandatory ID and/or data check was not completed</msg>
		</msgQ>
		<resData>
			<domain:panData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name paResult="0">punktum.dk</domain:name>
				<domain:paTRID>
					<clTRID>605c8f37-aff1-479a-bd67-2a288b7d2d0f</clTRID>
					<svTRID>A7941E40-8F21-11F1-A2B8-05474682B364</svTRID>
				</domain:paTRID>
				<domain:paDate>2026-03-16T03:16:39.0Z</domain:paDate>
			</domain:panData>
		</resData>
		<trID>
			<clTRID>549de41cc44e4036a308b40de04a75c5</clTRID>
			<svTRID>76C02671-DADD-9E7E-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-39"></a>
**Operation:** transfer  
**Message:** %.dk has been added to your portfolio  
**ResData type:** `domain:trnData`  
**Trigger:** The domain name has been added to your portfolio through a transfer.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="104" id="6809226">
			<qDate>2026-03-24T11:36:25.0Z</qDate>
			<msg>test.dk has been added to your portfolio</msg>
		</msgQ>
		<resData>
			<domain:trnData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name>test.dk</domain:name>
				<domain:trStatus>clientApproved</domain:trStatus>
				<domain:reID>REG-855170</domain:reID>
				<domain:reDate>2026-03-24T11:36:19.0Z</domain:reDate>
				<domain:acID>DKHM1-DK</domain:acID>
				<domain:acDate>2026-03-24T11:36:25.0Z</domain:acDate>
			</domain:trnData>
		</resData>
		<trID>
			<clTRID>30ed395866db4759a5cad0070ddf6d3a</clTRID>
			<svTRID>8FB8A6A3-4C51-776D-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-40"></a>
**Operation:** transfer  
**Message:** %.dk has been removed from your portfolio  
**ResData type:** `domain:trnData`  
**Trigger:** The domain name has been removed from your portfolio through a transfer to another registrar.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="104" id="6809226">
			<qDate>2026-03-24T11:36:25.0Z</qDate>
			<msg>punktum.dk has been removed from your portfolio</msg>
		</msgQ>
		<resData>
			<domain:trnData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name>punktum.dk</domain:name>
				<domain:trStatus>clientApproved</domain:trStatus>
				<domain:reID>REG-855170</domain:reID>
				<domain:reDate>2026-03-24T11:36:19.0Z</domain:reDate>
				<domain:acID>DKHM1-DK</domain:acID>
				<domain:acDate>2026-03-24T11:36:25.0Z</domain:acDate>
			</domain:trnData>
		</resData>
		<trID>
			<clTRID>30ed395866db4759a5cad0070ddf6d3a</clTRID>
			<svTRID>8FB8A6A3-4C51-776D-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-41"></a>
**Operation:** delete  
**Message:** %.dk has been deleted  
**ResData type:** `domain:panData`  
**Trigger:** The domain name has been deleted as the result of a pending delete operation initiated by the registrar.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="370" id="6820098">
			<qDate>2026-03-21T21:06:12.0Z</qDate>
			<msg>test123.dk has been deleted</msg>
		</msgQ>
		<resData>
			<domain:panData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name paResult="1">test123.dk</domain:name>
				<domain:paTRID>
					<clTRID>d15f015d-0017-456b-84a4-2de7235b5171</clTRID>
					<svTRID>A79420DA-8F21-11F1-9C45-05474682B364</svTRID>
				</domain:paTRID>
				<domain:paDate>2026-03-21T21:06:12.0Z</domain:paDate>
			</domain:panData>
		</resData>
		<trID>
			<clTRID>97f8e967159e4baa89709d2623a6aa2a</clTRID>
			<svTRID>2A513D02-71EB-13CE-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-42"></a>
**Operation:** delete  
**Message:** %.dk has been deleted  
**ResData type:** `domain:infData`  
**Trigger:** The domain name has been deleted as the result of another process rather than a registrar-initiated deletion, such as a failure to complete the mandatory ID or auto-expire.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="422" id="6814079">
			<qDate>2026-03-18T06:27:22.0Z</qDate>
			<msg>test.dk has been deleted</msg>
		</msgQ>
        <resData>
            <domain:infData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
                <domain:name>test.dk</domain:name>
                <domain:roid>TEST_DK-DK</domain:roid>
                <domain:status s="pendingDelete"/>
                <domain:status s="serverHold"/>
                <domain:status s="serverRenewProhibited"/>
                <domain:status s="serverTransferProhibited"/>
                <domain:status s="serverUpdateProhibited"/>
                <domain:registrant>DKHM1-DK</domain:registrant>
                <domain:ns>
                    <domain:hostObj>ns1.punktum.dk</domain:hostObj>
                    <domain:hostObj>ns2.punktum.dk</domain:hostObj>
                </domain:ns>
                <domain:clID>REG-666666</domain:clID>
                <domain:crDate>2026-03-18T06:27:22.0Z</domain:crDate>
                <domain:upDate>2026-03-18T06:27:22.0Z</domain:upDate>
                <domain:exDate>2027-03-18T21:59:59.0Z</domain:exDate>
            </domain:infData>
        </resData>
        <extension>
            <rgp:infData xmlns:rgp="urn:ietf:params:xml:ns:rgp-1.0" xsi:schemaLocation="urn:ietf:params:xml:ns:rgp-1.0 rgp-1.0.xsd">
                <rgp:rgpStatus s="redemptionPeriod"/>
            </rgp:infData>
            <dkhm:domainAdvisory xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5' domain="test.dk" advisory="pendingDeletionDate" date="2026-03-18T22:00:00.0Z"      />
            <dkhm:registrant_validated xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1</dkhm:registrant_validated>
            <secDNS:infData xmlns:secDNS='urn:ietf:params:xml:ns:secDNS-1.1'>
                <secDNS:dsData>
                    <secDNS:keyTag>21836</secDNS:keyTag>
                    <secDNS:alg>8</secDNS:alg>
                    <secDNS:digestType>2</secDNS:digestType>
                    <secDNS:digest>3b3596534d1a0aa8a33cd8ac1deb8a239fe7baa8c31e4e390bcde5ca90ee6d22</secDNS:digest>
                </secDNS:dsData>
            </secDNS:infData>
            <dkhm:autoRenew xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>true</dkhm:autoRenew>
            <dkhm:vid xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false</dkhm:vid>
        </extension>
        <trID>
            <clTRID>ABC-123</clTRID>
            <svTRID>12D5E75C-8F29-11F1-B164-9E4826B23308</svTRID>
        </trID>
    </response>
</epp>
```

</details>

---

<a id="ex-43"></a>
**Operation:** restore  
**Message:** %.dk has been extended and cancellation stopped  
**ResData type:** `domain:panData`  
**Trigger:** The domain name was marked for cancellation and has been renewed; the cancellation is stopped.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="81" id="6809600">
			<qDate>2026-01-23T15:07:19.0Z</qDate>
			<msg>test.dk has been extended and cancellation stopped</msg>
		</msgQ>
		<resData>
			<domain:panData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name paResult="1">test.dk</domain:name>
				<domain:paTRID>
					<clTRID>e273ee57-b0b4-4e2c-9751-8469d52162a1</clTRID>
					<svTRID>A794236D-8F21-11F1-894D-05474682B364</svTRID>
				</domain:paTRID>
				<domain:paDate>2026-01-23T15:07:19.0Z</domain:paDate>
			</domain:panData>
		</resData>
		<trID>
			<clTRID>5dcbde00c90c44489b429b74102dd49e</clTRID>
			<svTRID>44BE4FA8-E647-49E6-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-44"></a>
**Operation:** restore  
**Message:** %.dk has been restored, extended and cancellation stopped  
**ResData type:** `domain:panData`  
**Trigger:** The domain name has been restored and renewed; the cancellation is stopped.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="266" id="6819500">
			<qDate>2026-01-28T09:44:07.0Z</qDate>
			<msg>example.dk has been restored, extended and cancellation stopped</msg>
		</msgQ>
		<resData>
			<domain:panData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name paResult="1">example.dk</domain:name>
				<domain:paTRID>
					<clTRID>cc4014f1-e0ef-4780-9ef2-b53db9db32ab</clTRID>
					<svTRID>A79424A5-8F21-11F1-B4DD-05474682B364</svTRID>
				</domain:paTRID>
				<domain:paDate>2026-01-28T09:44:07.0Z</domain:paDate>
			</domain:panData>
		</resData>
		<trID>
			<clTRID>b279c4190b994fb6bd5a112a0dd504b4</clTRID>
			<svTRID>F69909ED-7D0D-CE53-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-56"></a>
**Operation:** suspend  
**Message:** %.dk has been suspended, as the registrant has not completed the ID/data check on time  
**ResData type:** `domain:infData`  
**Trigger:** The registrant did not complete the mandatory ID and/or data check within the deadline. The domain name is suspended and enters the deletion process.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="1" id="6873562">
			<qDate>2026-09-07T07:15:32.0Z</qDate>
			<msg>punktum.dk has been suspended, as the registrant has not completed the ID/data check on time</msg>
		</msgQ>
		<resData>
			<domain:infData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name>punktum.dk</domain:name>
				<domain:roid>PUNKTUM_DK-DK</domain:roid>
				<domain:status s="pendingDelete"/>
				<domain:status s="serverDeleteProhibited"/>
				<domain:status s="serverHold"/>
				<domain:status s="serverRenewProhibited"/>
				<domain:status s="serverTransferProhibited"/>
				<domain:status s="serverUpdateProhibited"/>
				<domain:registrant>DKHM1-DK</domain:registrant>
				<domain:ns>
					<domain:hostObj>ns1.punktum.dk</domain:hostObj>
					<domain:hostObj>ns2.punktum.dk</domain:hostObj>
				</domain:ns>
				<domain:clID>REG-666666</domain:clID>
				<domain:crDate>1996-06-06T22:00:00.0Z</domain:crDate>
				<domain:upDate>2026-07-01T01:10:29.0Z</domain:upDate>
				<domain:exDate>2027-06-30T21:59:59.0Z</domain:exDate>
			</domain:infData>
		</resData>
		<extension>
			<rgp:infData xmlns:rgp="urn:ietf:params:xml:ns:rgp-1.0" xsi:schemaLocation="urn:ietf:params:xml:ns:rgp-1.0 rgp-1.0.xsd">
				<rgp:rgpStatus s="pendingDelete"/>
			</rgp:infData>
			<dkhm:domainAdvisory xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5' domain="punktum.dk" advisory="pendingDeletionDate" date="2026-09-07T22:00:00.0Z"/>
			<dkhm:registrant_validated xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1</dkhm:registrant_validated>
			<secDNS:infData xmlns:secDNS='urn:ietf:params:xml:ns:secDNS-1.1'>
				<secDNS:dsData>
					<secDNS:keyTag>20918</secDNS:keyTag>
					<secDNS:alg>13</secDNS:alg>
					<secDNS:digestType>2</secDNS:digestType>
					<secDNS:digest>7e79534be5675143670647520667e70ac05134c97cc6c3810f7ca998c163a427</secDNS:digest>
				</secDNS:dsData>
			</secDNS:infData>
			<dkhm:autoRenew xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>true</dkhm:autoRenew>
			<dkhm:vid xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false</dkhm:vid>
		</extension>
		<trID>
			<clTRID>015e384bf5b5d60382dff3440b3929a5</clTRID>
			<svTRID>5AE0364B-F013-65FF-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-57"></a>
**Operation:** suspend  
**Message:** %.dk has been suspended, as the domain was cancelled  
**ResData type:** `domain:infData`  
**Trigger:** The domain name was cancelled and now enters the 30-day redemption period.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="1" id="6873564">
			<qDate>2026-09-07T08:05:30.0Z</qDate>
			<msg>punktum.dk has been suspended, as the domain was cancelled</msg>
		</msgQ>
		<resData>
			<domain:infData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name>punktum.dk</domain:name>
				<domain:roid>PUNKTUM_DK-DK</domain:roid>
				<domain:status s="pendingDelete"/>
				<domain:status s="serverDeleteProhibited"/>
				<domain:status s="serverHold"/>
				<domain:status s="serverRenewProhibited"/>
				<domain:status s="serverTransferProhibited"/>
				<domain:status s="serverUpdateProhibited"/>
				<domain:registrant>DKHM1-DK</domain:registrant>
				<domain:ns>
					<domain:hostObj>ns1.punktum.dk</domain:hostObj>
					<domain:hostObj>ns2.punktum.dk</domain:hostObj>
				</domain:ns>
				<domain:clID>REG-666666</domain:clID>
				<domain:crDate>1997-03-10T23:00:00.0Z</domain:crDate>
				<domain:upDate>2026-03-26T16:42:54.0Z</domain:upDate>
				<domain:exDate>2026-03-31T21:59:59.0Z</domain:exDate>
			</domain:infData>
		</resData>
		<extension>
			<rgp:infData xmlns:rgp="urn:ietf:params:xml:ns:rgp-1.0" xsi:schemaLocation="urn:ietf:params:xml:ns:rgp-1.0 rgp-1.0.xsd">
				<rgp:rgpStatus s="redemptionPeriod"/>
			</rgp:infData>
			<dkhm:domainAdvisory xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5' domain="punktum.dk" advisory="pendingDeletionDate" date="2026-09-07T22:00:00.0Z"/>
			<dkhm:registrant_validated xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1</dkhm:registrant_validated>
			<dkhm:autoRenew xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>true</dkhm:autoRenew>
			<dkhm:vid xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false</dkhm:vid>
		</extension>
		<trID>
			<clTRID>8fb32820efd258a592cee944ee403ab8</clTRID>
			<svTRID>5AE114DA-2633-19FF-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-58"></a>
**Operation:** suspend  
**Message:** %.dk has been suspended, as the domain was set to auto expire  
**ResData type:** `domain:infData`  
**Trigger:** The domain name was set to auto expire and has now reached the end of its paid period. The domain name is suspended and enters the 30-day redemption period.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="1" id="6873566">
			<qDate>2026-09-07T08:07:14.0Z</qDate>
			<msg>punktum.dk has been suspended, as the domain was set to auto expire</msg>
		</msgQ>
		<resData>
			<domain:infData xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">
				<domain:name>punktum.dk</domain:name>
				<domain:roid>PUNKTUM_DK-DK</domain:roid>
				<domain:status s="pendingDelete"/>
				<domain:status s="serverHold"/>
				<domain:status s="serverRenewProhibited"/>
				<domain:status s="serverTransferProhibited"/>
				<domain:status s="serverUpdateProhibited"/>
				<domain:registrant>DKHM1-DK</domain:registrant>
				<domain:ns>
					<domain:hostObj>ns1.punktum.dk</domain:hostObj>
					<domain:hostObj>ns2.punktum.dk</domain:hostObj>
				</domain:ns>
				<domain:clID>REG-666666</domain:clID>
				<domain:crDate>1997-04-16T22:00:00.0Z</domain:crDate>
				<domain:upDate>2024-05-22T09:49:17.0Z</domain:upDate>
				<domain:exDate>2028-06-30T21:59:59.0Z</domain:exDate>
			</domain:infData>
		</resData>
		<extension>
			<rgp:infData xmlns:rgp="urn:ietf:params:xml:ns:rgp-1.0" xsi:schemaLocation="urn:ietf:params:xml:ns:rgp-1.0 rgp-1.0.xsd">
				<rgp:rgpStatus s="redemptionPeriod"/>
			</rgp:infData>
			<dkhm:domainAdvisory xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5' domain="punktum.dk" advisory="pendingDeletionDate" date="2026-09-07T22:00:00.0Z"/>
			<dkhm:registrant_validated xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>1</dkhm:registrant_validated>
			<dkhm:autoRenew xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>true</dkhm:autoRenew>
			<dkhm:vid xmlns:dkhm='urn:dkhm:params:xml:ns:dkhm-4.5'>false</dkhm:vid>
		</extension>
		<trID>
			<clTRID>6805a8221de883e2b82f78b16185b7e1</clTRID>
			<svTRID>5AE114DA-2638-19FF-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

### host

<a id="ex-45"></a>
**Operation:** create  
**Message:** The name server %host% has been registered, as the registrant has approved it  
**ResData type:** `host:panData`  
**Trigger:** The name server has been registered, as the registrant approved its creation.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="1" id="6824622">
			<qDate>2026-03-26T23:25:15.0Z</qDate>
			<msg>The name server ns1.punktum.dk has been registered, as the registrant has approved it</msg>
		</msgQ>
		<resData>
			<host:panData xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name paResult="1">ns1.punktum.dk</host:name>
				<host:paTRID>
					<clTRID>370a2037-66a7-49ea-99a3-0e38015c7721</clTRID>
					<svTRID>A79425BE-8F21-11F1-8EDE-05474682B364</svTRID>
				</host:paTRID>
				<host:paDate>2026-03-26T23:25:15.0Z</host:paDate>
			</host:panData>
		</resData>
		<trID>
			<clTRID>52f735724ba84ee9b022a2298277f133</clTRID>
			<svTRID>D1206E93-A2DB-746A-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-46"></a>
**Operation:** create  
**Message:** The name server %host% has not been registered, as it has been rejected by the registrant  
**ResData type:** `host:panData`  
**Trigger:** The name server was not registered, as the registrant actively rejected it.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="361" id="6807432">
			<qDate>2026-02-09T16:22:08.0Z</qDate>
			<msg>The name server ns1.frewald.dk has not been registered, as it has been rejected by the registrant</msg>
		</msgQ>
		<resData>
			<host:panData xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name paResult="0">ns1.frewald.dk</host:name>
				<host:paTRID>
					<clTRID>fe496e95-dc0e-413e-a6a6-088f7a158839</clTRID>
					<svTRID>A79426CE-8F21-11F1-A2CE-05474682B364</svTRID>
				</host:paTRID>
				<host:paDate>2026-02-09T16:22:08.0Z</host:paDate>
			</host:panData>
		</resData>
		<trID>
			<clTRID>1c756f01ddd248df91f5c6d23a2a83b7</clTRID>
			<svTRID>7EDA0FCC-6B79-FCFD-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-47"></a>
**Operation:** create  
**Message:** The name server %host% has not been registered, as it was not approved by the registrant in time  
**ResData type:** `host:panData`  
**Trigger:** The name server was not registered, as the registrant did not respond to the request in time.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="201" id="6808351">
			<qDate>2026-02-03T23:12:55.0Z</qDate>
			<msg>The name server ns01.punktum.dk has not been registered, as it was not approved by the registrant in time</msg>
		</msgQ>
		<resData>
			<host:panData xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name paResult="0">ns01.punktum.dk</host:name>
				<host:paTRID>
					<clTRID>85d45450-c784-4fa4-b0de-781d98f94918</clTRID>
					<svTRID>A7942970-8F21-11F1-9E44-05474682B364</svTRID>
				</host:paTRID>
				<host:paDate>2026-02-03T23:12:55.0Z</host:paDate>
			</host:panData>
		</resData>
		<trID>
			<clTRID>54129e1e9808441da492b0195784d79a</clTRID>
			<svTRID>2932E9D4-C6BE-1146-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-48"></a>
**Operation:** create  
**Message:** The name server %host% has been registered, as the registrant has approved it and %handle% has accepted the name server manager role  
**ResData type:** `host:panData`  
**Trigger:** The name server has been registered, as both the registrant and the name server manager accepted their respective roles.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="366" id="6802525">
			<qDate>2026-02-28T08:13:02.0Z</qDate>
			<msg>The name server ns1.test.dk has been registered, as the registrant has approved it and DKHM1-DK has accepted the name server manager role</msg>
		</msgQ>
		<resData>
			<host:panData xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name paResult="1">ns1.test.dk</host:name>
				<host:paTRID>
					<clTRID>2ffc9d96-51dd-4f19-9aee-f953f10f809c</clTRID>
					<svTRID>A7942BFB-8F21-11F1-99A5-05474682B364</svTRID>
				</host:paTRID>
				<host:paDate>2026-02-28T08:13:02.0Z</host:paDate>
			</host:panData>
		</resData>
		<trID>
			<clTRID>4013c1effba14b8daeafa8abdde4b3d2</clTRID>
			<svTRID>C0C817E5-E4A4-C894-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-49"></a>
**Operation:** create  
**Message:** The name server %host% has not been registered, as the name server manager role has been rejected  
**ResData type:** `host:panData`  
**Trigger:** The name server was not registered, as the name server manager actively rejected the role.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="163" id="6824528">
			<qDate>2026-02-04T11:20:35.0Z</qDate>
			<msg>The name server ns11.test.dk has not been registered, as the name server manager role has been rejected</msg>
		</msgQ>
		<resData>
			<host:panData xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name paResult="0">ns11.test.dk</host:name>
				<host:paTRID>
					<clTRID>17cc6f70-1d70-4884-b7a7-6dba7b1d0683</clTRID>
					<svTRID>A7942D9B-8F21-11F1-B4A8-05474682B364</svTRID>
				</host:paTRID>
				<host:paDate>2026-02-04T11:20:35.0Z</host:paDate>
			</host:panData>
		</resData>
		<trID>
			<clTRID>ee3e1e5b11ac423fa4070d2d2be45a0a</clTRID>
			<svTRID>FA6DF9D2-2A59-765D-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-50"></a>
**Operation:** create  
**Message:** The name server %host% has not been registered, as the name server manager role was not accepted in time  
**ResData type:** `host:panData`  
**Trigger:** The name server was not registered, as the name server manager did not accept the role in time.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="217" id="6810526">
			<qDate>2026-01-06T14:41:31.0Z</qDate>
			<msg>The name server ns12.test.dk has not been registered, as the name server manager role was not accepted in time</msg>
		</msgQ>
		<resData>
			<host:panData xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name paResult="0">ns12.test.dk</host:name>
				<host:paTRID>
					<clTRID>6c1f7ddf-57bc-47c5-ba9d-8abf9a21f0b1</clTRID>
					<svTRID>A7943028-8F21-11F1-9320-05474682B364</svTRID>
				</host:paTRID>
				<host:paDate>2026-01-06T14:41:31.0Z</host:paDate>
			</host:panData>
		</resData>
		<trID>
			<clTRID>a8858d4055bf4ad5928cbd1802142af6</clTRID>
			<svTRID>0B6C64E6-83C3-E13C-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-51"></a>
**Operation:** create  
**Message:** The name server %host% has been registered, as %handle% has accepted the name server manager role  
**ResData type:** `host:panData`  
**Trigger:** The name server has been registered, as the name server manager accepted the role.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="217" id="6817458">
			<qDate>2026-03-14T20:54:40.0Z</qDate>
			<msg>The name server ns5.example.dk has been registered, as DKHM1-DK has accepted the name server manager role</msg>
		</msgQ>
		<resData>
			<host:panData xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name paResult="1">ns5.example.dk</host:name>
				<host:paTRID>
					<clTRID>30ea594b-063d-4f1a-a206-972d5fac8878</clTRID>
					<svTRID>A79432CD-8F21-11F1-85A5-05474682B364</svTRID>
				</host:paTRID>
				<host:paDate>2026-03-14T20:54:40.0Z</host:paDate>
			</host:panData>
		</resData>
		<trID>
			<clTRID>2e3c7580435f49e082018670c80f9aac</clTRID>
			<svTRID>EE82C829-D3EB-C539-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-52"></a>
**Operation:** update  
**Message:** The name server manager role for %host% has been accepted by %handle%  
**ResData type:** `host:panData`  
**Trigger:** During a name server handover, the role was accepted by the new name server manager.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="450" id="6800022">
			<qDate>2026-03-13T23:05:35.0Z</qDate>
			<msg>The name server manager role for ns3.test.dk has been accepted by DKHM1-DK</msg>
		</msgQ>
		<resData>
			<host:panData xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name paResult="1">ns3.test.dk</host:name>
				<host:paTRID>
					<clTRID>0f38bd27-dd5a-4178-b37a-51c5324d90c7</clTRID>
					<svTRID>A794342E-8F21-11F1-8B41-05474682B364</svTRID>
				</host:paTRID>
				<host:paDate>2026-03-13T23:05:35.0Z</host:paDate>
			</host:panData>
		</resData>
		<trID>
			<clTRID>effae1ed96f146f5be8a034c3d22389a</clTRID>
			<svTRID>148027AC-8432-D3C2-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-53"></a>
**Operation:** update  
**Message:** The name server manager role for %host% has been rejected by %handle%  
**ResData type:** `host:panData`  
**Trigger:** During a name server handover, the name server manager role was actively rejected.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="401" id="6823664">
			<qDate>2026-03-31T07:55:40.0Z</qDate>
			<msg>The name server manager role for ns.punktum.dk has been rejected by DKHM1-DK</msg>
		</msgQ>
		<resData>
			<host:panData xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name paResult="0">ns.punktum.dk</host:name>
				<host:paTRID>
					<clTRID>1dce25ea-9699-4b49-aa2b-1416584650b3</clTRID>
					<svTRID>A794353B-8F21-11F1-AC33-05474682B364</svTRID>
				</host:paTRID>
				<host:paDate>2026-03-31T07:55:40.0Z</host:paDate>
			</host:panData>
		</resData>
		<trID>
			<clTRID>3409c452b50b43a6aaa7ef45031d160d</clTRID>
			<svTRID>CC52615D-415A-5DEC-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-54"></a>
**Operation:** update  
**Message:** The name server manager role for %host% has not been accepted by %handle% in time  
**ResData type:** `host:panData`  
**Trigger:** During a name server handover, the name server manager role was not accepted within the deadline.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="158" id="6805106">
			<qDate>2026-01-10T11:26:40.0Z</qDate>
			<msg>The name server manager role for ns15.test.dk has not been accepted by DKHM1-DK in time</msg>
		</msgQ>
		<resData>
			<host:panData xmlns:host="urn:ietf:params:xml:ns:host-1.0">
				<host:name paResult="0">ns15.test.dk</host:name>
				<host:paTRID>
					<clTRID>79713775-a6bf-4ea0-beae-e3fef9633ac6</clTRID>
					<svTRID>A79437B6-8F21-11F1-891C-05474682B364</svTRID>
				</host:paTRID>
				<host:paDate>2026-01-10T11:26:40.0Z</host:paDate>
			</host:panData>
		</resData>
		<trID>
			<clTRID>acb7a659cf6b412587d130f4d01af012</clTRID>
			<svTRID>95E9E7CE-3497-9147-E065-000000000202</svTRID>
		</trID>
	</response>
</epp>
```

</details>

---

<a id="ex-55"></a>
**Operation:** delete  
**Message:** The name server %host% has been deleted  
**ResData type:** `host:infData`  
**Trigger:** The name server has been deleted.

<details>
<summary>Show XML example</summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<epp
	xmlns="urn:ietf:params:xml:ns:epp-1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
	<response>
		<result code="1301">
			<msg>Command completed successfully; ack to dequeue</msg>
		</result>
		<msgQ count="158" id="6805106">
			<qDate>2026-01-10T11:26:40.0Z</qDate>
			<msg>The name server ns1.test.dk has been deleted</msg>
		</msgQ>
        <resData>
            <host:infData xmlns:host="urn:ietf:params:xml:ns:host-1.0">
                <host:name>ns1.test.dk</host:name>
                <host:roid>NS1_TEST_DK-DK</host:roid>
                <host:status s="ok"/>
                <host:clID>DKHM1-DK</host:clID>
                <host:crID>DKHM1-DK</host:crID>
                <host:crDate>2013-11-06T07:37:01.0Z</host:crDate>
            </host:infData>
        </resData>
        <trID>
            <clTRID>0af6ad7a3a611e1bca89e1f5a61951ce</clTRID>
            <svTRID>DD5B6F88-8F2E-11F1-A2AC-C56C229A15CC</svTRID>
        </trID>
    </response>
</epp>
```

</details>
