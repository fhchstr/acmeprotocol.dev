---
weight: 11
title: Certificate Lifetime
params:
  tabTitle: 47-Day Certificate Lifespan
description:
  Get ready for 47-day certificate validity by 2029. Understand why shorter
  lifespans are crucial for WebPKI security and what you should do about it.
---

# Certificate Validity Period

The maximal allowed validity of certificates is set by (continuously evolving)
[WebPKI](/webpki/) requirements that all publicly trusted CAs must comply with.
Here is the evolution of the maximal allowed validity of TLS web server
certificates.

- In 2012, it was set to 60 months (5 years).
- In 2015, it was reduced to 39 months (3 years and 3 months).
- In 2018, it went down to 825 days (a bit more than 2 years).
- In 2021, it was reduced further to 398 days (1 year and 1 month).
- In 2025, a plan
  ([ballot SC-081](https://github.com/cabforum/servercert/pull/553)) to
  gradually reduce the maximal allowed validity was endorsed by all major web
  browsers and most publicly trusted CAs as follows:
  - In 2026, it will be reduced to 200 days (to allow for a half-yearly renewal
    cadence)
  - In 2027, it will be reduced to 100 days (to allow for a quarterly renewal
    cadence)
  - In 2029, it will be reduced to 47 days (to allow for a monthly renewal
    cadence)

[Google Trust Services](https://pki.goog/) and
[Let's Encrypt](https://letsencrypt.org) have always set the maximal validity of
their certificates below 100 days, as shown by
[this article published in 2015](https://letsencrypt.org/2015/11/09/why-90-days/)
by Let's Encrypt.

## Why Is It Shrinking ? {#why}

There are many reasons for reducing the maximal allowed validity of
certificates:

1. Certificates represent a **point in time** state of reality, which may
   diverge over time. For instance, domain owners can legitimately request
   certificates with maximal validity 1 day prior their domain name registration
   expires. See https://insecure.design/, which explains this problem in depth.
2. To **increase the agility** of clients by forcing them to automate their
   certificate lifecycle management (which is now easy thanks to
   [ACME](/getting-started/)). Even if this is an existing expectation, history
   has shown that too many clients lack the ability to quickly rotate their
   certificates in case of emergency such as exposed private keys,
   [Heartbleed](https://www.heartbleed.com/)-like vulnerabilities, or
   [mandated revocations](https://blog.mozilla.org/security/2025/03/12/enhancing-ca-practices-key-updates-in-mozilla-root-store-policy-v3-0/).
3. To **reduce reliance on certificate revocation**, which is imperfect
   ([[1]](https://www.imperialviolet.org/2014/04/29/revocationagain.html),
   [[2]](https://scotthelme.co.uk/revocation-is-broken/)). Web browsers
   implemented mechanisms such as
   [CRLSets](https://www.chromium.org/Home/chromium-security/crlsets/) and
   [CRLite](https://blog.mozilla.org/security/2020/01/09/crlite-part-1-all-web-pki-revocations-compressed/)
   to work around the limitations of certificates revocation, but these
   mechanisms only solve the problem for web browsers. Other certificate
   consumers are still subject to the imperfections of certificate revocations.
   Some clients don't even check the revocation status of certificates!
4. To facilitate the **swift deprecation of obsolete cryptographic algorithms**.
   The lengthy process of phasing out SHA-1 highlighted the need for a more
   agile ecosystem. This urgency is amplified by the emergence of quantum
   computers, which threaten to render the cryptographic algorithms currently
   used in certificates obsolete.
5. To **transition inappropriate use cases to private CAs**. Too many systems
   use publicly trusted certificates because it is easy, and not because they
   actually need publicly trusted certificates. Since these systems often lack
   quick update capabilities, they are slowing down innovations in the WebPKI.
   Changes and improvements are hard to introduce because they risk breaking
   such ossified systems who would be better served by private CAs.

The validity of certificates **<ins>is not</ins>** shrinking to enable CAs to
make more money by selling more certificates. If they haven't already, CAs will
have to adapt their pricing models to offer a subscription program with
unlimited certificates for the paid time period. If they don't, they will lose
their customers to competitors or free CAs such as
[Google Trust Services](https://pki.goog) and
[Let's Encrypt](https://letsencrypt.org).

## What is in Scope ?

The timeline for reducing the maximal allowed validity of certificates described
on this page **only affects publicly trusted TLS web server certificates**.
Other certificate types, such as code signing, document signing, or email
protection certificates are not subject to the same timeline, but note that they
may each have their own timeline for reducing the maximal allowed certificate
validity.

The DCV (Domain Control Validation) reuse periods are also being reduced. By
2029, valid authorizations for domain names will only be reusable for 10 days.
Passed that delay, certificate applicants will have to prove again that they
control the domains in question when requesting new certificates.

**Other reuse periods specific to OV, EV, or QWAC certificates are not
affected**. When requesting such certificates, applicants will have to
frequently prove that they control each domain name (which is easy and
automatic), but they won't have to prove as frequently that the legal entity who
owns a domain name is still the same/still exists (which can't be easily
automated).

Private CAs are not affected since they do not have to comply with the WebPKI
requirements.

## What Should I Do ?

You must automate your certificate lifecycle management using [ACME](/acme/) or
other automation methods supported by your CA. The maximal allowed validity is
shrinking gradually to give enough time to certificate consumers to automate
their certificate lifecycle management. Get ahead of the wave and do it now!

If automation is not possible, ask yourself whether you really need publicly
trusted certificates. In general, publicly trusted certificates are only needed
for public-facing websites exposed to the Internet since their visitors are very
diverse/unknown. Internal websites can use certificates issued by private CAs
since it should be possible to distribute the private CA certificate in the
trust store of all their visitors.

**Client authentication certificates must also migrate to private CAs !**
Publicly trusted WebPKI certificates are only for servers. The
[Chrome Root Store Policy](https://googlechrome.github.io/chromerootprogram/#322-pki-hierarchies-included-in-the-chrome-root-store)
already announced that publicly trusted CAs will be forbidden to issue
`clientAuth` certificates in 2026.

It is now easier than ever to spin up private CAs using
[Google Cloud's Certificate Authority Service](https://cloud.google.com/security/products/certificate-authority-service)
for example, or by partnering with security vendors that can manage private CAs
for you.

If you operate appliances that don't support automation, pressure your vendors
to add support for it. If they don't, they will loose all their customers since
the validity period reduction is enforced by all publicly trusted CAs.
Alternatively, install a reverse HTTPS proxy which supports certificate
automation in front of legacy systems. See
[Getting started with ACME](/getting-started/) for guidance.

If your CA doesn't support any automation method, leave them. For the record,
the
[Chrome Root Store Policy](https://googlechrome.github.io/chromerootprogram/#331-automation-support)
requires all new CAs to support automation. This requirement will eventually be
enforced for all CAs.

## Short-lived Certificates

Short-lived certificates are valid for less than 7 days (or 10 days if they were
issued before 2026-03-15). **Short-lived certificates don't have to be
revocable**. Since revocation is imperfect, status information can take days to
become globally consistent when certificates are revoked. Some clients even
forget to query the revocation status of certificates. Short-lived certificates
solve this problem since **they are guaranteed to become invalid after just a
few days**.

[Google Trust Services](https://pki.goog/) can issue certificate valid for as
low as a single day and they limit the maximal validity of
[IP address certificates](https://pki.goog/faq/#faq-IPCerts) to just a few days.

Let's Encrypt
[announced support for short-lived certificates](https://letsencrypt.org/2025/01/16/6-day-and-ip-certs/)
and declared that these certificates will also support IP addresses in addition
to domain names.
