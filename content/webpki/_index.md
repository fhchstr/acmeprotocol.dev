---
weight: 3
bookToC: false
title: The WebPKI
---

# The WebPKI

The WebPKI (Web Public Key Infrastructure) is the system of digital
certificates, CAs (Certificate Authorities), web browsers, and other entities
that enable secure communication and authentication on the Internet, primarily
through HTTPS.

The WebPKI was originally focussed on TLS web servers authentication and
encryption via HTTPS, but the term is increasingly being used to encompass other
use cases, such as code signing or email protection. All of these use cases rely
on the same principles of public-key cryptography and trusted CAs, ensuring
secure communication and authentication in various digital contexts.

On this website, the term "WebPKI" refers to its original meaning only. Email
protection, code signing, and other use cases are not addressed.

## What is a Publicly Trusted CA ?

The WebPKI enables clients and servers to establish trustworthy secure
communication channels without knowing anything about each other nor having
shared any information beforehand.

This system works thanks to CAs, which are **third party** entities that clients
trust. Clients trust that CAs are perform the necessary validations before
issuing certificates that attest that websites is who they claim to be.

**Publicly trusted CAs are trusted by all major clients <ins>by default</ins>**.
In practice, a CA is considered publicly trusted if it trusted by Chrome
(Google), Mozilla (Firefox), Apple, and Microsoft, which all run a so-called
"Root Program". Linux distributions do not run Root Programs, most of them
simply trust the same CAs as Firefox.

CAs who want to be trusted by these Root Programs must comply with all their
requirements (linked below). The CAs must provide evidence to third party
auditing firms, who issue yearly reports attesting that the audited CA complies
with all requirements. When necessary, Root Programs may also request additional
evidence.

- [Chrome Root Store Policy](https://googlechrome.github.io/chromerootprogram/)
- [Mozilla Root Store Policy](https://www.mozilla.org/en-US/about/governance/policies/security-group/certs/policy/)
- [Apple Root Store Policy](https://www.apple.com/certificateauthority/ca_program.html)
- [Microsoft Root Store Policy](https://learn.microsoft.com/en-us/security/trusted-root/program-requirements)

All Root Programs, and publicly trusted CAs are part of the
[CA/B Forum](https://cabforum.org/) (Certificate Authorities and Browsers
Forum). This forum maintains a common set of requirements, called the
[BRs (Baseline Requirements)](https://github.com/cabforum/servercert/blob/main/docs/BR.md)
that all participating CAs must comply with. All these requirements are based on
the
[WebTrust framework](https://cabforum.org/about/information/auditors-and-assessors/webtrust-for-cas/).

**Trust is hard-earned, easily lost, and difficult to reestablish**. The whole
system relies on openness, transparency, and consequences. For example, CAs must
disclose their practices in so-called "Certification Practice Statement"
documents published to their websites, and they must self-report all identified
compliance violations.

**The system is also forgiving**. Mistakes can happen. What matters is how they
are handled, and what safeguards are put in place to ensure they never happen
again. All disclosed CA incidents are publicly visible at
https://wiki.mozilla.org/CA/Incident_Dashboard.

When necessary, Root Programs may decide to stop trusting CAs, like
[it happened to Entrust in 2024](https://security.googleblog.com/2024/06/sustaining-digital-certificate-security.html).
