TODO for epp_update_domain_v1 branch of the DK Hostmaster EPP Service Specification

- [ ] Add documentation on sub-processes
- [ ] Clarify aspects of domain locks on processes
- [x] Evaluate generic attribute manipulation (registration period etc)
- [x] Implement matrix for privileges
- [ ] Evaluate whether a `2307` should be returned for update domain, change registrant, instead of `2306`. `2306` is more correct, but perhaps `2307` is better suited for the time being
