# Contact Verification Draft

Revision: 1.0
Date: 2024-03-08

## Introduction
This document serves as a draft design covering all technical aspects related to facilitating contact verification between registrars and Punktum dk, aligning with the NIS2 directive.

NIS2 refers to the revised version of the Network and Information Systems Directive, a regulatory framework implemented by the European Union to enhance the overall cybersecurity posture of its member states.

### What is NIS2?
The NIS2 directive focuses on bolstering the resilience of critical information infrastructure, such as communication networks, cloud computing services, and digital service providers. It aims to ensure a high common level of cybersecurity across the European Union, promoting a collaborative and coordinated approach to handling cybersecurity incidents.

### Key Objectives of NIS2
Increased Security Measures:
NIS2 emphasizes the adoption of robust security measures by operators of essential services (OES) and digital service providers (DSPs). This includes implementing measures to prevent and minimize the impact of cybersecurity incidents.

Incident Reporting Requirements:
The directive mandates OES and DSPs to report significant cybersecurity incidents to the competent national authorities. This facilitates a timely and coordinated response to cyber threats, ultimately enhancing the overall cybersecurity resilience of critical services.

Cooperation and Information Sharing:
NIS2 encourages collaboration and information sharing among member states, competent authorities, and relevant stakeholders. This cooperative approach aims to address cross-border cybersecurity challenges effectively.

### Alignment with the Contact Verification Draft
The Contact Verification Draft has been designed with the principles outlined in the NIS2 directive in mind. By aligning with NIS2, the draft ensures that the contact verification process not only complies with technical and legal requirements but also contributes to the overarching cybersecurity goals set forth by the European Union.

In summary, the integration of NIS2 principles into the Contact Verification Draft underscores the commitment to a standardized and secure approach in managing verification processes within the regulatory framework of the European Union.

## Requirements
The primary objective of the proposed solution is to implement minimal changes to existing integrations, ensuring compliance with all legal and technical requirements. The solution should encompass the following key elements:

1. Enable registrars to inform the registry about completed contact verification before registrant creation in the registry systems.
2. Allow registrars to update the registry on verification status changes for existing registrants. This includes modifications to the existing registrant information, triggered by any changes in identity verification status (e.g., from 'pending' to 'verified' or 'verified' to 'unverified').
3. Give registrars the ability to query the registry for the current identity verification status of all registrants under their management.
4. Implement asynchronous messaging to update registrars on registrant verification status changes in the registry.

## Setting Verification Status for Registrar-handled Contacts
Punktum dk does not require detailed documentation on the actual ID verification performed by the registrar, as long as the verification aligns with the requirements outlined in the Registrar Agreement. Consequently, registrars are only obligated to include a verification status flag in the registrar create or update process.

### Registrar Portal

- Enable registrars to specify which verification model should be applied to their account (registrar verification vs. registry verification).
- If registar verification is enabled on account, provide dropdown to specify verification status on the handle creation page.

### EPP (Extensible Provisioning Protocol)
A new extension, `<dkhm:id-verification>`, will be incorporated into contact.create, contact.update, and contact.info. This extension will support values of `eid`, `verified`, `rejected`, `pending` or `undverified`, with no additional information required. The update triggered by this extension will include modifications to existing contact information based on changes in ID verification status.

## Supported Identity Validation Statuses

- `eid` indicates that verification has been completed using electronic identification.
- `verified` indicates that verification has been performed, and relevant documentation has been reviewed and approved by Punktum dk.
- `rejected` indicates that verification has been performed, and relevant documentation has been reviewed and rejected by Punktum dk.
- `pending` indicates that verifcication has been requested and currently awaits a response from the contact.
- `unverified` indicates that verification has not been completed nor initiated on the contact.

The reason for distinguishing between electronic (eID) and documentation-based means of verifcation is due to the fact that using eID solutions provides instantaneous verification of the contact, compared to document-based identity verification, where a waiting period will occur while awaiting receiving documentation from the contact.

In cases where Punktum dk is responsible for performing the verification process, the `pending` status will be relevant to allow the registrar to follow the ongoing status of the verification process.

## Notification of Verification Status Changes
As the verification handled by Punktum dk requires actions to be performed by the contact, the registrar needs to be informed as soon as the verification status changes. Notifications on status changes will be issued to the registrar without delay via poll messaging or email, based on the specific communication settings configured in the Registrar Portal.

## Limitations

- Registrars are only able to provide statuses `eid`, `verified` and `unverified` during contact creation. `pending` and `rejected` are statuses reserved for Punktum dk verification process.
- After a contact has been assigned the status of `eid` or `verified`, these statuses are immutable and cannot be altered

## Questions

- Is it relevant for registrars to be able to update contact verification status from `unverified` to `verified` or `eid` on already existing contacts?
- Should pending identity verification requests fall back to status `unverified` upon expiration or would a status `expired` be useful?

## Providing Feedback
Your input is valuable in refining and improving the Contact Identity Verification Draft. If you have suggestions, comments, or concerns, we encourage you to share your feedback. Your contributions can help enhance the clarity, effectiveness, and completeness of the document.

### How to Provide Feedback

1. **GitHub Issues:**
   - The preferred method for submitting feedback is through GitHub Issues. Visit the [repository's issue tracker](https://github.com/Punktum-dk/epp-service-specification/issues) and create a new issue.
   - Clearly outline your feedback, providing specific details and context where necessary.
   - Tag the issue appropriately, such as "enhancement," "clarification," or "bug," to help categorize and prioritize the feedback.

2. **Pull Requests:**
   - If you have concrete suggestions for changes or improvements, you can submit a pull request.
   - Fork the repository, make your changes, and submit a pull request for review.
   - Include a brief description of the changes made and the rationale behind them.

3. **Contacting Punktum dk:**
   - If you prefer direct communication, you can reach out to Punktum dk through the contact information provided on our official website or relevant communication channels.

Your feedback is essential in ensuring the Contact Identity Verification Draft meets the needs of its users. Punktum dk appreciates your time and effort in contributing to the refinement of this important document.

## Document History
Revision: 1.0
Date: 2024-03-08
Notes: Initial draft version.
