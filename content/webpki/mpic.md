---
weight: 13
bookToC: false
title: MPIC (Multi-Perspective Issuance Corroboration)
params:
  tabTitle: MPIC - Multi-Perspective Issuance Corroboration
description:
  Multi-Perspective Issuance Corroboration (MPIC) protects Certificate
  Authorities (CAs) against network hijacking attacks when issuing certificates.
---

# MPIC (Multi-Perspective Issuance Corroboration)

Before issuing certificates, CAs (Certificate Authorities) verify that
requesters control the identifiers (domain name or IP address) requested to be
included. Different [challenge types](/acme/challenges/) exist to enable
requesters to prove they control an identifier.

All challenge types require the CA to query a network resource via DNS or HTTP
and verify that it holds the expected value. The queries made by the CA are
performed over unauthenticated channels. This means that **they are vulnerable
to network hijacking attacks**.

If attackers can influence how packets are routed on the network, **they could
successfully solve ACME challenges** by redirecting the validation requests made
by the CA to attacker-controlled servers instead of their intended destinations,
resulting in the CA issuing certificates for identifiers not legitimately
controlled by the attackers.

**This is not just a theoretical risk**. Researchers have already proven that
[this attack works](https://www.usenix.org/conference/usenixsecurity18/presentation/birge-lee)!

To mitigate such attacks,
[Let's Encrypt](https://letsencrypt.org/2020/02/19/multi-perspective-validation/)
and
[Google Trust Services](https://security.googleblog.com/2023/05/google-trust-services-acme-api_0503894189.html)
started sending their **validation requests from multiple vantage points
worldwide** in 2020 and 2023, respectively. At the time, this concept was called
MPDV (Multi-Perspective Domain Validation). In parallel, a new
[compliance requirement](https://github.com/cabforum/servercert/blob/main/docs/BR.md#3229-multi-perspective-issuance-corroboration)
was adopted to **require all publicly trusted CAs to perform MPIC validations**
(Multi-Perspective Issuance Corroboration) by 2025-2026. See the announcement on
the
[Google Security Blog](https://security.googleblog.com/2025/03/new-security-requirements-adopted-by.html)
for more information.

Sending validation requests from multiple vantage points worldwide and
corroborating the results, by requiring that the majority of the requests to
result in successful challenge validation, has been proven to be an effective
defense mechanism against network hijacking attacks.

To ensure successful MPIC validations, sites must **allow traffic from any IP
address**, both on their HTTP and DNS servers, since validation requests from
CAs may originate from anywhere. CAs usually do not disclose their source IP
addresses, and they may change them without notice.
