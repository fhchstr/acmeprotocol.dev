---
weight: 11
title: Challenge Types
params:
  tabTitle: ACME Challenge Types
---

# ACME Challenge Types

ACME challenges provide the CA with assurance that certificate requesters
control the identifiers (domain name or IP address) requested to be included
certificates. To successfully complete challenges, clients must both prove that
they control the identifiers in question and that they hold the private key of
the account key pair used to respond to the challenges.

ACME challenges typically require the client to set up some network accessible
resource that the CA can query (via DNS or HTTP)
[from multiple vantage points worldwide](/webpki/mpic/) in order to validate
that the client controls an identifier. Once the challenge is validated, the
client should de-provision the resource.

If the CA queries the resource too early, for example before the information had
time to propagate globally, the validation may fail. This is why clients should
only ask the CA to validate challenges once they believe the request will
succeed. This is particularly important since **some CAs refuse to retry failed
challenge validations**!

Most ACME challenges make use of a "key authorization string". This string
concatenates the [CA-provided token](/acme/overview/#authz) for the challenge
with the ACME account key fingerprint, separated by a "`.`" character

```
keyAuthorization = token || '.' || base64url(sha256(accountKey))
```

## http-01

The `http-01` challenge type requires the client to provision a file, with a
specific content, at a specific path on a web server. The file must be available
over HTTP on TCP port 80, and not just HTTPS.

The URL at which the file must be provisioned is
`http://{domain}/.well-known/acme-challenge/{token}`. Its content must be the
ASCII representation of the key authorization.

See [RFC 8555](https://datatracker.ietf.org/doc/html/rfc8555#section-8.3) and
[the excellent summary from Let's Encrypt](https://letsencrypt.org/docs/challenge-types/#http-01-challenge)
for more information about the `http-01` challenge type and its pros/cons.

## dns-01

The `dns-01` challenge type requires the client to provision a `TXT` resource
record containing a designated value on DNS. The value is the SHA-256 digest of
the key authorization, and the validation record name is
`_acme-challenge.{domain}`.

The `dns-01` challenge type can be used to issue certificates containing
[wildcard domain names](https://www.keyfactor.com/blog/what-is-a-wildcard-certificate/).

See [RFC 8555](https://datatracker.ietf.org/doc/html/rfc8555#section-8.4) and
[the excellent summary from Let's Encrypt](https://letsencrypt.org/docs/challenge-types/#dns-01-challenge)
for more information about the `dns-01` challenge type and its pros/cons.

## dns-account-01

The `dns-account-01` challenge type is almost the same as `dns-01`. The main
difference is that the validation record name is not just
`_acme-challenge.{domain}`. Instead, it contains an extra label derived from the
ACME account key. For example: `_ujmmovf2vn55tgye._acme-challenge.{domain}`.

The `dns-account-01` challenge type can be used to issue certificates containing
[wildcard domain names](https://www.keyfactor.com/blog/what-is-a-wildcard-certificate/).

The `dns-account-01` challenge type enables multiple independent systems to
authorize a single domain name **concurrently** since it relies on a different
DNS validation record name for each ACME account. The `dns-account-01` challenge
type avoids CNAME delegation conflicts inherent to `dns-01`. This is
particularly valuable for multi-region or multi-cloud deployments that wish to
rely upon DNS-based domain control validation and need to independently obtain
certificates for the same domain.

The `dns-account-01` challenge type is not standardized yet. See the
<ins>**draft**</ins>
[RFC](https://datatracker.ietf.org/doc/draft-ietf-acme-dns-account-label/) for
more information.

## tls-alpn-01

The `tls-alpn-01` challenge type requires to construct a **self-signed
certificate** containing a designed value, and serving it to clients negotiating
the "`acme-tls/1`" application-layer protocol in the ALPN (Application Layer
Protocol Negotiation) extension during the TLS handshake when connecting to TCP
port 443.

The self-signed certificate must contain the domain name being validated as a
`dnsName` in the `subjectAlternativeName` extension, and it must include the
`acmeIdentifier` extension which must contain the SHA-256 digest of the key
authorization.

See [RFC 8737](https://datatracker.ietf.org/doc/html/rfc8737) and
[the excellent summary from Let's Encrypt](https://letsencrypt.org/docs/challenge-types/#tls-alpn-01)
for more information about the `tls-alpn-01` challenge type and its pros/cons.
