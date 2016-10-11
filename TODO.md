TODO for the DK Hostmaster EPP Service Specification

- [X] Add diagrams for commands and processes
- [X] Add diagrams for DKH sub-processes
- [X] Add more information on extensions required
- [X] Evaluate possible error situation for create domain / update domain where domain name ends in .dk, but IP address is not specified: `2306`?
- [X] Evaluate RFC:5732 host:chg, `2305`

https://tools.ietf.org/html/rfc5732#page-15

- [X] Evaluate host-1.0.xsd element `host:status` for update host

https://github.com/DK-Hostmaster/epp-xsd-files/blob/epp_nameserver_admin_v1/host-1.0.xsd#L207-L235
>>>>>>> epp_nameserver_admin_v1

- [X] Clarify privileges for registrar vs. billing contact as initiator for renew domain
- [X] Add documentation on sub-processes for update domain
- [ ] Clarify aspects of domain locks on processes
- [X] Evaluate generic attribute manipulation (registration period etc)
- [X] Implement matrix for privileges
- [X] Evaluate whether a `2307` should be returned for update domain, change registrant, instead of `2306`. `2306` is more correct, but perhaps `2307` is better suited for the time being

- [X] Investigate `dkhm:attention`, the use might be useless
- [X] Investigate setting of status codes for domain update and clarify
- [ ] Clarify the use of keyholder for DNSSEC keys

- [ ] Exchange XML examples for renew domain
- [ ] Exchange XML examples for update domain
- [ ] Exchange XML examples for create host
- [ ] Exchange XML examples for update host
- [X] Evaluate delete contact status codes (clientDeleteProhibited and serverDeleteProhibited)
- [X] Document status codes in general

- [X] Correct preact link
- [X] Correct mailform link
- [X] Add EPP service link