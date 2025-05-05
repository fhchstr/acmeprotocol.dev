---
weight: 14
title: CT (Certificate Transparency)
---

# CT (Certificate Transparency)

The "[Trust, but verify](https://en.wikipedia.org/wiki/Trust,_but_verify)"
proverb fits particularly well the WebPKI. Clients trust CAs to only issue
certificates after they've performed the necessary verifications. But what if
the CA makes a mistake or if someone manipulates the CA into issuing
certificates for a domain you own? \
**You would certainly want to know about it, wouldn't you?**

That's exactly what CT (Certificate Transparency) is for! Publicly trusted CAs
are obliged to submit all certificates they issue to CT logs, which are public
cryptographically verifiable **append-only data stores** based on
[Merkle trees](https://en.wikipedia.org/wiki/Merkle_tree), which is the same
technology that underpins the
[Bitcoin cryptocurrency](https://en.wikipedia.org/wiki/Bitcoin). This system
works because
[most web browsers](https://developer.mozilla.org/en-US/docs/Web/Security/Certificate_Transparency#browser_requirements)
**only trust certificates submitted to CT logs**.

This page only provides a high-level overview of certificate transparency. Refer
to https://certificate.transparency.dev/ for more information.

Running a CT log is expensive and requires a lot of work. Log operators deserve
a large thank you from all wbe users. They provide a valuable service that most
users are not aware of.

## Origin

Certificate transparency was designed to detect maliciously or mistakenly issued
certificates. It has since evolved into a global database which contains all
publicly trusted TLS web server certificates. This database is used for various
purposes. For example, security and compliance researchers use it to discover
malformed or non-compliant certificates issued by CAs.

Certificate transparency was first standardized in
[RFC 6962](https://datatracker.ietf.org/doc/html/rfc6962), and Certificate
Transparency **<ins>2.0</ins>** was later standardized in
[RFC 9162](https://datatracker.ietf.org/doc/html/rfc9162). In 2024, its
inventors were awarded the
[Levchin Prize](https://rwc.iacr.org/LevchinPrize/winners.html#CT), which honors
major innovations in cryptography.

## Domain Owner Responsibilities

**Site owners are supposed to monitor CT logs for certificates issued for their
domains**. If they find a certificate they did not request, site owners can
contact the CA who issued it to request its revocation. The faster and simpler
way is to [revoke it via ACME](/acme/overview/#revocation).

Domain owners can use tools and services listed at
https://certificate.transparency.dev/monitors/ to monitor certificates issued
for their domains. In particular, https://crt.sh/ is very handy to explore
certificates submitted to CT logs.

{{% details title="Your hostnames are not private !" open=false %}}

Anyone can watch CT logs to discover (potentially internal/non-public) hostnames
and [newly turned up sites](https://dl.acm.org/doi/10.1145/3278532.3278562).
**Hostnames cannot be considered private**!

If you rely on your internal hostnames to be secret to remain secure, first,
just don't.
[Security through obscurity is not a good strategy](https://en.wikipedia.org/wiki/Security_through_obscurity#Criticism).
Second, consider using a private CA to issue TLS web server certificates for
your internal hosts if you want to ensure they won't show up CT logs.

{{% /details %}}

## How does it work ?

CAs must embed [SCTs (Signed Certificate Timestamps)](/webpki/cert/#sct) in
certificates they issue [^1]. A SCT is receipt signed by a CT log to attest it
accepted the certificate. But there is a problem here. Since the certificate
doesn't exist yet, how can it be submitted to the CT log?

[^1]:
    In reality, there are other ways to present SCTs to clients, but embedding
    them in certificates is by far the most common way to do it. See
    [RFC 6962](https://datatracker.ietf.org/doc/html/rfc6962#section-3.3) for
    more information.

**Precertificates to the rescue**! A precertificate is a clone of a certificate
with 3 key differences:

1. It doesn't include SCTs.
2. It includes a [critical](/webpki/cert/#extensions) CT poison extension, which
   is not recognized by any client (by design/on purpose). This ensures that the
   precertificate cannot be used instead of the real certificate.
3. Since there are differences, the signature is obviously also different.

Here is an [example precertificate](https://crt.sh/?id=17531419628) and the
[corresponding certificate](https://crt.sh/?id=17870169097).

SCTs cannot be signed by any CT log. They must be signed by CT logs
trusted/recognized by web browsers. In the same way that browsers only trust
certain CAs, browsers also only trust certain CT logs. Here are the CT logs
recognized by major browsers:

- [Google Chrome](https://www.gstatic.com/ct/log_list/v3/log_list.json)
- [Apple Safari](https://valid.apple.com/ct/log_list/current_log_list.json)
- [Mozilla Firefox](https://blog.transparency.dev/ct-in-firefox#heading-firefox-known-ct-logs)
- Microsoft Edge does not require certificates to be submitted to CT logs

When browsers validate publicly trusted TLS web server certificates, they
extract the SCTs and they verify their signature against their list of trusted
CT logs. The signature verification doesn't require connecting to the CT logs.
