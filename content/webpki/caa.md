---
weight: 12
bookToC: false
title: CAA (Certificate Authority Authorization)
params:
  tabTitle: CAA - Certificate Authority Authorization
description:
  Prevent unauthorized certificate issuances. Learn how CAA (Certificate
  Authority Authorization) lets you control which CAs can issue certificates for
  your domains.
---

# CAA (Certificate Authority Authorization)

CAA (Certificate Authority Authorization) is a
[DNS record type](https://en.wikipedia.org/wiki/List_of_DNS_record_types) that
specifies which CAs (Certificate Authorities) are allowed to issue certificates
for a domain name. CAA can also set additional certificate issuance constraints.
[Compliance requirements](https://github.com/cabforum/servercert/blob/main/docs/BR.md#3228-caa-records)
**oblige CAs to honor CAA records** when issuing certificates.

When validating a domain name, CAs must search CAA records by climbing the DNS
name tree from the specified domain name up to the DNS root until a CAA record
is found. In other words, CAA records set on a domain name apply to all
subdomains, unless more specific CAA records are set on subdomains. If no CAA
record is found, no restriction exists, and all CAs are authorized to issue
certificates for that domain name.

DV (Domain Validated) certificates issued by publicly trusted CAs are all
equally trusted. Browsers treat certificates issued by popular CAs the same way
as certificates issued by small regional CAs. As demonstrated by
[this article from 2022](https://unmitigatedrisk.com/?p=673), **98% of all
publicly trusted TLS web server certificates were issued by the top 5 CAs** (out
of 200+ CAs). Current numbers are still in the same ballpark. The vast majority
of publicly trusted TLS web server certificates are issued by a handful of CAs.

If you know you will only request certificates from a few selected CAs, **CAA is
a cheap and easy way to prevent other CAs from mistakenly issuing certificates
for your domains**. CAA alone is not sufficient. You should still monitor
[CT (Certificate Transparency)](/webpki/ct/) logs to detect misissued
certificates.

Here is how a CAA record looks like. This one only authorizes
[Google Trust Services](https://pki.goog/faq/#caa) to issue certificates for
`google.com` and its subdomains (assuming there are no other CAA records set on
subdomains). It is possible to set multiple CAA records on a domain name to
authorize multiple CAs to issue certificates for a domain or to set additional
constraints.

```bash
$ dig +short google.com CAA
0 issue "pki.goog"
```

The most common additional constraints that CAA can set are:

- `issuewild`, which is like `issue` but specific to
  [wildcard domain names](https://www.keyfactor.com/blog/what-is-a-wildcard-certificate/).
- `validationmethods`, which specifies which
  [challenge types](/acme/challenges/) are allowed to prove domain control.
- `accounturi`, which specifies which accounts (e.g.
  [ACME accounts](/acme/overview/#account)) are allowed to request certificates.
- `cansignhttpexchanges`, which specifies whether
  [SXG certificates](https://wicg.github.io/webpackage/draft-yasskin-http-origin-signed-responses.html)
  can be issued.

See
[this excellent article from Let's Encrypt](https://letsencrypt.org/docs/caa/)
for more information about CAA and https://sslmate.com/caa/ to get guidance for
configuring CAA.
