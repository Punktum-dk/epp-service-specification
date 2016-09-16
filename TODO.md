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
- [ ] Add documentation on sub-processes for iådate domain
- [ ] Clarify aspects of domain locks on processes
- [x] Evaluate generic attribute manipulation (registration period etc)
- [x] Implement matrix for privileges
- [ ] Evaluate whether a `2307` should be returned for update domain, change registrant, instead of `2306`. `2306` is more correct, but perhaps `2307` is better suited for the time being
