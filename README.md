# Fast Track Transfer Initiation Process

## Introduction

To reduce complexity for the registrant while increasing security by allowing the auth token to be distributed system-to-system instead of via the end user.

The model does not change the transfer rules themselves, but rather the way the transfer is initiated.

## Document History

22-06-2026 Clarified purpose of contact element and contact cloning behaviour during transfer

22-06-2026 Added payload fields table with contact type enum values and examples for individual and company registrants

26-02-2026 Initial draft version published

## Overall Principle

The auth token is treated as a technical authorization artifact that Punktum dk can securely transport on behalf of an authenticated registrant, rather than requiring the token to be handled manually by the user.

The existing model, where the registrant receives and shares the token themselves, is retained as a fallback.

## User Flow (From the Registrant's Perspective)

![Fast Track Transfer Initiation Process](./fast-track-transfer-concept.png)

1. The registrant logs into Punktum dk's self-service portal.
2. The registrant selects **"Transfer domain to registrar"**.
3. The registrant selects a registrar from a list of approved registrars.
4. If the registrar supports fast track, the registrant is presented with two options:
   - Contact the registrar manually using the auth token (classic model), or
   - Request Punktum dk to initiate the transfer directly with the registrar (fast track).
5. If fast track is selected, the registrant provides consent for Punktum dk to share:
   - Domain name  
   - Auth token  
   - Relevant contact information  

   with the selected registrar for the purpose of initiating the transfer process.


## Technical Interaction (Punktum dk → Registrar)

Once the registrant has provided consent, Punktum dk performs a backend call to the registrar's fast track endpoint.

### API key
If the registrar defines API key to use in Fast Track transfer configuration, this will be passed as X-API-KEY header in the request.

### Example Payload

- Domain name  
- Auth token (short-lived and tied to this specific transfer intention)  
- Registrant reference  
- Contact information (in accordance with consent)  
- Timestamp of consent  

In this model, the auth token will be:

- Short-lived  

The `contact` element contains the information of the current registrant of the domain as registered at Punktum dk. It is intended to help the registrar identify and match the registrant against an existing user in their own system, reducing the need for the registrant to re-enter their details during the transfer flow. If no existing contact handle is specified in the subsequent EPP transfer command issued from the registrar to the registry, Punktum dk will automatically clone the complete contact dataset and assign it a new contact ID.

#### Payload Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `contact.id` | string | Yes | The registrant's handle at Punktum dk (e.g. `ABCD1234-DK`) |
| `contact.name` | string | Yes | Full name of the registrant |
| `contact.email` | string | Yes | Email address of the registrant |
| `contact.phone` | string | Yes | Phone number in E.164 format (e.g. `+4512345678`) |
| `contact.address.street` | string | Yes | Street address |
| `contact.address.zip` | string | Yes | Postal code |
| `contact.address.city` | string | Yes | City |
| `contact.address.country` | string | Yes | ISO 3166-1 alpha-2 country code (e.g. `DK`) |
| `contact.type` | enum | Yes | Type of registrant. Equivalent to `dkhm:userType` in the EPP service specification. Allowed values: `individual`, `company`, `public_organization`, `association` |
| `contact.vat_number` | string | No | VAT number (CVR). Only applicable for `company`, `public_organization`, and `association`. Included if registered at Punktum dk, otherwise `null` |
| `contact.p_number` | string | No | Production unit number (P-number). Only applicable for `company`, `public_organization`, and `association`. Included if registered at Punktum dk, otherwise `null` |
| `consent.granted_at` | string (ISO 8601) | Yes | Timestamp when the registrant granted consent |
| `consent.ip_address` | string | Yes | IP address from which consent was granted |
| `domains[].domain_name` | string | Yes | The domain name to transfer (e.g. `eksempel.dk`) |
| `domains[].auth_token.value` | string | Yes | The auth token authorizing the transfer |
| `domains[].auth_token.expires_at` | string (ISO 8601) | Yes | Expiry timestamp of the auth token |
| `domains[].auth_token.scope` | string | Yes | Scope of the token — always `transfer` for this flow |

**Example 1 — Individual registrant** (`vat_number` and `p_number` are `null`):

```json
{
  "contact": {
    "id": "JD1234-DK",
    "name": "Jane Doe",
    "email": "jane.doe@eksempel.dk",
    "phone": "+4512345678",
    "address": {
      "street": "Example Street 12",
      "zip": "2100",
      "city": "Copenhagen",
      "country": "DK"
    },
    "type": "individual",
    "vat_number": null,
    "p_number": null
  },
  "consent": {
    "granted_at": "2026-04-17T11:55:00Z",
    "ip_address": "203.0.113.42"
  },
  "domains": [
    {
      "domain_name": "eksempel.dk",
      "auth_token": {
        "value": "token-domain-1",
        "expires_at": "2026-04-17T12:00:00Z",
        "scope": "transfer"
      }
    },
    {
      "domain_name": "eksempel2.dk",
      "auth_token": {
        "value": "token-domain-2",
        "expires_at": "2026-04-17T12:00:00Z",
        "scope": "transfer"
      }
    },
    {
      "domain_name": "eksempel3.dk",
      "auth_token": {
        "value": "token-domain-3",
        "expires_at": "2026-04-17T12:05:00Z",
        "scope": "transfer"
      }
    }
  ]
}
```

**Example 2 — Company registrant** (using public information from Punktum dk A/S):

```json
{
  "contact": {
    "id": "PDK1234-DK",
    "name": "Punktum dk A/S",
    "email": "administration@eksempel.dk",
    "phone": "+4533646060",
    "address": {
      "street": "Ørestads Boulevard 108, 11.",
      "zip": "2300",
      "city": "Copenhagen S",
      "country": "DK"
    },
    "type": "company",
    "vat_number": "DK24210375",
    "p_number": "1006410698"
  },
  "consent": {
    "granted_at": "2026-04-17T11:55:00Z",
    "ip_address": "203.0.113.42"
  },
  "domains": [
    {
      "domain_name": "eksempel.dk",
      "auth_token": {
        "value": "token-domain-1",
        "expires_at": "2026-04-17T12:00:00Z",
        "scope": "transfer"
      }
    }
  ]
}
```

## Registrar Response

The registrar confirms receipt and returns a unique link representing a created transfer session at the registrar.

- Use standard HTTP headers to communicate status (200 for OK and 4xx and 5xx for errors)
- Required transfer_session_url containing link to transfer flow at registrar

### Example Response
```json
{
  "transfer_session_url": "https://example.dk/transfer/session/e125dec7170048378528a664a1c25d1b"
}
```

Using the received information Punktum dk redirects the registrant from the self-service portal to the provided url allowing the registrant to continue directly in the registrar's transfer flow.

## Security Considerations

This model reduces the exposure of auth tokens, as they are not distributed via email or manual copying. The token is transferred exclusively system-to-system between Punktum dk and the registrar.

Punktum dk remains a neutral party and solely facilitates the technical initiation of the transfer following the registrant's explicit request.

## Requirements for Registrars Supporting Fast Track

Registrars wishing to participate must, via the registrar portal:

- Enable **"Fast Track Transfer"**
- Provide endpoint URL for receiving transfer initialization
- Provide API key if required when calling registrar endpoint

Only registrars that have actively opted in will be displayed as fast track–supporting in the self-service portal.

## Fallback

If a registrar does not support fast track, the existing model applies, where the registrant receives and shares the auth token manually.
