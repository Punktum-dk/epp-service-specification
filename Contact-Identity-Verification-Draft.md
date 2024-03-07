# Contact Identity Verification Draft

Revision: 1.0
Date: 2024-03-08

## Introduction
This document serves as a draft design covering all technical aspects related to facilitating contact ID verification between registrars and Punktum dk, aligning with the NIS2 directive.

## Requirements
The primary objective of the proposed solution is to implement minimal changes to existing integrations, ensuring compliance with all legal and technical requirements. The solution should encompass the following key elements:

1. Enable registrars to inform the registry about completed itendity verification before registrant creation in the registry systems.
2. Allow registrars to update the registry on identity verification status changes for existing registrants. This includes modifications to the existing registrant information, triggered by any changes in identity verification status (e.g., from 'pending' to 'verified' or 'verified' to 'unverified').
3. Give registrars the ability to query the registry for the current identity verification status of all registrants under their management.
4. Implement asynchronous messaging to update registrars on registrant identity verification status changes in the registry.

## Setting Identity Verification Status for Registrar-handled Contacts
Punktum dk does not require detailed documentation on the actual ID verification performed by the registrar, as long as the verification aligns with the requirements outlined in the Registrar Agreement. Consequently, registrars are only obligated to include a verification status flag in the registrar create or update process.

### Registrar Portal
A checkbox will be added to the handle creation page, allowing registrars to confirm that ID verification has already been performed.

### EPP (Extensible Provisioning Protocol)
A new extension, `<dkhm:id-verification>`, will be incorporated into contact.create, contact.update, and contact.info. This extension will support values of `eid`, `verified`, `rejected`, `pending` or `undverified`, with no additional information required. The update triggered by this extension will include modifications to existing contact information based on changes in ID verification status.

## Supported Identity Validation Statuses

- `eid` indicates that identity control has been completed using electronic identification.
- `verified` indicates that identity control has been performed, and relevant documentation has been reviewed and approved by Punktum dk.
- `rejected` indicates that identity control has been performed, and relevant documentation has been reviewed and rejected by Punktum dk.
- `pending` indicates that identity control has been requested and currently awaits a response from the contact.
- `unverified` indicates that identity control has not been completed nor initiated on the contact. Setting this value on contact.create commands specifies that Punktum dk is requested to handle the identity verification process. For contacts residing in Denmark, this automatically triggers a request for identity veritication using MitID, whereas foreign contacts will be subjected to a risk-based evaulation of the provided data, and a manual identification requests will be initiated, if deemed necessary by the risk evaluation results.

The reason for distinguishing between electronic and documentation-based means of identification is due to the fact that using eID solutions provides instantaneous verification of the contact, compared to document-based identity verification, where a waiting period will occur while awaiting receiving documentation from the contact.

In cases where Punktum dk is responsible for performing the verification process, the `pending` status will be relevant to allow the registrar to follow the ongoing status of the verification process.

## Notification of Identity Verification Status Changes
As the verification handled by Punktum dk requires actions to be performed by the contact, the registrar needs to be informed as soon as the identity verification status changes. Notifications on status changes will be issued to the registrar via poll messaging or email, based on the specific communication settings configured in the Registrar Portal.

## Limitations
* Registrars are only able to provide statuses `eid`, `verified` and `unverified` during contact creation. `pending` and `rejected` are statuses reserved for Punktum dk identity verification process.
* Once a contact has been assigned status `eid` or `verified` these statuses are locked and cannot been changed.

## Questions
* Is it relevant for registrars to be able to update contact verification status from `unverified` to `verified` or `eid` on already existing contacts?
* Should pending identity verification requests fall back to status `unverified` upon expiration or would a status `expired` be useful?

## Document History
Revision: 1.0
Date: 2024-03-08
Notes: Initial draft version.
