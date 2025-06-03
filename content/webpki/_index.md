---
weight: 3
bookToC: false
title: The WebPKI
description:
  Understand the WebPKI. Learn the role, guarantees, and obligations of publicly
  trusted CAs, and their distinction from private CAs.
---

# The WebPKI

{{% details title="The role of CAs" open=false %}}

CAs (Certificate - or Certification - Authorities) enable clients and servers to
establish trustworthy secure communication channels **without knowing anything
about each other** nor having shared any information (like secret keys)
beforehand.

CAs are **third party** entities who issue certificates that relying parties
agree to trust. In the context of TLS/HTTPS, HTTP clients are the relying
parties (HTTP servers are also relying parties when using
[mTLS](https://en.wikipedia.org/wiki/Mutual_authentication#mTLS)). Relying
parties have various reasons to trust CAs. Generally, they trust CAs because CAs
regularly **prove** that they can be trusted. For example, they publicly
disclose their practices, they maintain tamper evident audit logs recording all
significant events, and they share information allowing relying parties to
verify how they are operated. Relying parties may also simply trust a CA because
it is operated by the same organization or by an entity they
implicitly/unconditionally trust.

There are many different use cases for certificates in the context of TLS/HTTPS
alone. For example: to authenticate public Internet websites, to authenticate
internal application servers, to authenticate servers operated by partners, to
authenticate internal clients, to authenticate partner clients, etc.

Relying parties decide what CAs they trust depending on the use case and whether
they want to optimize for compatibility, reliability, agility, security, etc. In
general, a CA should only be trusted if the value it provides exceeds the risks
introduced by trusting it.

{{% /details %}}

The WebPKI (Web Public Key Infrastructure) is the system of digital
certificates, CAs (Certificate Authorities), web browsers, and other entities
that enable secure communication and authentication on the Internet, primarily
through HTTPS.

The WebPKI started with TLS web servers authentication and encryption via
TLS/HTTPS. It has since evolved to encompass other use cases, such as code
signing or email protection. All of these use cases rely on the same principles
of
[public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography)
and trusted CAs, ensuring secure communication and authentication in various
digital contexts.

The main characteristic of the WebPKI is that **its CAs are <ins>trusted by
default</ins> by almost all consumer products** (web browsers, command-line
utilities, operating systems, email clients, embedded systems, programming
languages, etc.). This is why WebPKI CAs are also called "**publicly trusted
CAs**" or simply "public CAs".

The term "publicly trusted" isn't well-defined. In theory, it means "trusted by
default by **all clients**", but in practice it means "trusted by default by
**most clients**". Concretely, a CA can be considered publicly trusted if it is
trusted by Google (Chrome, Android), Mozilla (Firefox), Apple (Safari, iOS,
MacOS), and Microsoft (Windows, Edge), which all maintain their own **root
store** and root program. Many products and systems derive their trusted CA list
from one or more of these. For example, most Linux distributions trust the same
CAs as Mozilla.

These WebPKI root store operators, and others, have collaborated to create
[CCADB (Common Certification Authority Database)](https://www.ccadb.org/), a
repository of information about CAs they collectively trust. CCADB improves the
security, transparency, and interoperability within the WebPKI ecosystem.

Things get complicated when considering products like gaming consoles, smart
TVs, cars, or connected home appliances (fridge, oven, washing machine, etc.),
which may maintain their own root stores. Since they don't participate in CCADB
and they don't publicly disclose their policies, these root stores may only
trust a subset of the CAs trusted by major root programs. This makes it
difficult to know precisely what CAs are trusted by which clients. These
products occasionally have issues when CAs rotate their issuing CA certificate
chains, which may happen at any time without prior notice as noted by
[Google Trust Services](https://pki.goog/faq/#faq-27) and
[Let's Encrypt](https://letsencrypt.org/2024/03/19/new-intermediate-certificates/#rotating-issuance).

Other systems, like military or governmental infrastructures, financial
institutions, or other large corporations, need to securely communicate with
internal services via TLS/HTTPS, but not necessarily with the Internet. Such
systems are clearly not part of the WebPKI; they rely on **private PKIs**
(a.k.a. "private CAs") instead.

## Root Stores

A root store (a.k.a. trust store) is a collection of CA certificates trusted by
a relying party/HTTP client. Most relying parties don't maintain their own root
store. They use the root store of their underlying operating system, and they
let the operating system maintainers curate the set of trusted CAs.

The root program policy of major WebPKI root store operators defines the
conditions for including/excluding CAs in their respective root store.

- [Chrome Root Program Policy](https://googlechrome.github.io/chromerootprogram/)
- [Mozilla Root Program Policy](https://www.mozilla.org/en-US/about/governance/policies/security-group/certs/policy/)
- [Apple Root Program Policy](https://www.apple.com/certificateauthority/ca_program.html)
- [Microsoft Root Program Policy](https://learn.microsoft.com/en-us/security/trusted-root/program-requirements)

Since all root programs have a common set of requirements, they all base their
policy on the
[BRs (Baseline Requirements)](https://github.com/cabforum/servercert/blob/main/docs/BR.md)
prescribed by the
[CA/B Forum (Certificate Authorities and Browsers Forum)](https://cabforum.org/),
of which they are all active members. The BRs are in turn based on requirements
set by the
[WebTrust framework](https://www.cpacanada.ca/business-and-accounting-resources/audit-and-assurance/overview-of-webtrust-services/principles-and-criteria).
Root store operators regularly propose
[improvements](https://cabforum.org/working-groups/server/ballots/#passed) to
the CA/B Forum BRs to strengthen the WebPKI.

To become, <ins>**and remain**</ins>, trusted by root programs, publicly trusted
CAs must, among other things, undergo annual audits by neutral third party
auditing firms to prove that they comply with all requirements. Publicly trusted
CAs must also publicly disclose all compliance violations on
[Bugzilla](https://bugzilla.mozilla.org/buglist.cgi?product=CA%20Program&component=CA%20Certificate%20Compliance&bug_status=__open__)
and commit to implementing meaningful improvements to ensure similar violations
won't happen again.

Root store operators continuously maintain their root program policies, evaluate
new CA inclusions, monitor the compliance of CAs they trust, and take actions
(e.g. [distrust the CA](https://unmitigatedrisk.com/?p=850)) when violations are
detected. Failing to perform these responsibilities may lead to serious security
issues for those who rely on these root stores!

## Private PKIs

Private PKIs (a.k.a. "private CAs") are **<ins>not</ins> part of the WebPKI**.
CAs are considered private if they are **<ins>not</ins> trusted by default** by
major consumer software or products.

Private CAs are **<ins>not</ins> subject to the continuously evolving WebPKI
requirements** such as [maximal certificate validities](/webpki/cert-lifetime/),
[mandated revocations](https://github.com/cabforum/servercert/blob/main/docs/BR.md#4911-reasons-for-revoking-a-subscriber-certificate),
[required pre-issuance validations](https://github.com/cabforum/servercert/blob/main/docs/BR.md#3224-validation-of-domain-authorization-or-control),
[CAA checking](/webpki/caa/),
[Certificate Transparency submission](/webpki/ct/),
[strict certificate profiles](https://github.com/cabforum/servercert/blob/main/docs/BR.md#7127-subscriber-server-certificate-profile),
etc.

Private CAs should be used when the peers are known in advance or when the peers
are under the control of the same or partner entities. For example: for internal
websites (a.k.a intranet) only accessible by employees or partners, or for
authenticating clients when using
[mTLS](https://en.wikipedia.org/wiki/Mutual_authentication#mTLS). This is the
opposite of the original purpose of the WebPKI and publicly trusted CAs, which
is to enable **anyone** to securely connect to public Internet websites.
