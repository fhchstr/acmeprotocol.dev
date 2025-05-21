---
weight: 2
title: The ACME Protocol
description:
  Demystify the ACME protocol and understand why free DV (Domain Validated)
  certificates are often your best choice.
---

## What is ACME ?

**Automatic Certificate Management Environment (ACME)** is a standardized
([RFC 8555](https://datatracker.ietf.org/doc/html/rfc8555)) communication
protocol to automate the process of obtaining and renewing **digital
certificates**, and automate the process of proving domain control, which is a
prerequisite for obtaining publicly trusted X.509 TLS certificates for web
servers.

## Why use ACME ?

Because it makes **HTTPS easy and automatic**. ACME enables you to quickly and
easily get TLS web server (`serverAuth`) certificates for your HTTPS websites.
ACME eliminates manual hassle and the risk of expired certificates!

## How much does ACME cost ?

ACME is a standardized protocol, so it cannot "cost" anything.

Some publicly trusted ACME CAs (Certificate Authorities) such as
[Google Trust Services](https://pki.goog) and
[Let's Encrypt](https://letsencrypt.org) **provide TLS web server certificates
for free**. Their free Domain Validated (DV) TLS web server certificates provide
the same security as paid certificates.

These CAs offer free certificates to enable everyone to enjoy a secure and
privacy-respecting Internet.

## Why do some CAs charge money for TLS web server certificates ?

{{% hint info %}}

This answer is only about **publicly trusted TLS web server certificates**. It
does not apply to private CAs nor other types of certificates like document/code
signing or client authentication certificates.

{{% /hint %}}

**Short answer**: Because people who do not know better are still willing to pay
for TLS web server certificates.

**Long answer**: Running a CA is expensive. It requires many roles (developers,
site reliability engineers, security/compliance officers, support personnel,
etc.), a reliable infrastructure and specialized hardware, as well as regular
audits performed by third party firms.

In addition, paying for services related to TLS web server certificates, such as
24/7 support, higher quota limits, or access to monitoring/visualization
dashboards, is fair and justified.

Some CAs offer, and charge for, OV (Organization Validated), EV (Extended
Validation), or QWAC (Qualified Website Authentication Certificate)
certificates. Unlike DV (Domain Validated) certificates, which can be issued
within seconds in a fully automated manner thanks to ACME, OV, EV, and QWAC
certificates require additional validations, which are generally **performed
manually** by
[Validation Specialists](https://github.com/cabforum/servercert/blob/main/docs/BR.md#:~:text=Validation%20Specialist%3A).
These additional manual validations justify the extra costs for OV, EV, and QWAC
certificates.

**OV, EV, and QWAC certificates are <ins>not</ins> more secure than DV
certificates**. They use the same technology and cryptographic algorithms as DV
certificates. Some regulatory frameworks may require the use of OV, EV, or QWAC
certificates in certain industries, but if you don't have to comply with such
regulations, **just use free DV certificates**. OV, EV, and QWAC certificates
include additional fields and attributes, such as the name, locality, or even
the VAT number of the entity that requested the certificate. Even if this
additional information was thoroughly validated by the CA, web browsers ignore
it. Thus, there is no point in paying extra money for OV, EV, or QWAC
certificates.

HTTPS doesn't guarantee that a site is secure or that the entity that operates
it is trustworthy. As noted in
[this Chromium blog post](https://blog.chromium.org/2023/05/an-update-on-lock-icon.html),
HTTPS only guarantees that you are connecting to the site you intended to and
that **the connection is secure** (not the site!). Even if this is less than you
expected, these are extremely important guarantees that underpin the Internet's
security.

Finally, some CAs charge additional fees to include additional subdomains,
domain names, or wildcard domain names in certificates. This is not worth paying
for DV certificates automatically issued using ACME. Don't fall for it. **Use
free DV certificates instead**.
