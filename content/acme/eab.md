---
weight: 12
title: EAB (External Account Binding)
bookToC: false
---

# EAB (External Account Binding)

To enforce quota limits, provide technical support, prevent abuse, or other
reasons, some ACME CAs may require **an existing account in a non-ACME system**,
such as a CA customer database, as a **prerequisite for creating new ACME
accounts**. This account association is called EAB (External Account Binding).
It is specified in
[RFC 8555](https://datatracker.ietf.org/doc/html/rfc8555#section-7.3.4).

CAs having this requirement advertise it by setting the
`meta.externalAccountRequired` field to `true` in the JSON object returned by
the [ACME Directory URL](/acme/overview/#directory).

**<ins>Before</ins> creating an account with such CAs, clients must use a
mechanism <ins>outside of ACME</ins>** to retrieve a MAC (Message Authentication
Code) key and a key identifier provided by the CA. Clients can then create a new
[ACME account](/acme/overview/#account) by setting the `externalAccountBinding`
field in the `newAccount` request. The value of this field must be derived from
the MAC key and key identifier previously provided by the CA. See
[RFC 8555](https://datatracker.ietf.org/doc/html/rfc8555#section-7.3.4) for more
information.

The CA returns an `externalAccountRequired` error if it receives a `newAccount`
request without the `externalAccountBinding` field set when it is required.

## Getting the MAC Key and the Key Identifier

Since these values must be retrieved using a mechanism outside of ACME, each CA
implements its own method to get the MAC key and the key identifier. For
example:

- [Google Trust Services](https://cloud.google.com/certificate-manager/docs/public-ca-tutorial#request-key-hmac)
  requires running the `gcloud` CLI.
- [ZeroSSL](https://zerossl.com/documentation/acme/generate-eab-credentials/)
  requires using their REST API.
- [SSL.com](https://www.ssl.com/guide/ssl-tls-certificate-issuance-and-revocation-with-acme/#ftoc-heading-2)
  requires using their web dashboard.

## Setting the `externalAccountBinding` Field

{{% hint danger %}}

**Not all ACME clients support EAB**. Make sure to use a client that supports
EAB if your CA requires it.

{{% /hint %}}

Refer to the documentation of your ACME client to find out how to pass the EAB
MAC key and the key identifier. For example:

- [certbot](https://certbot.eff.org/) supports the `--eab-hmac-key` and
  `--eab-kid` flags.
- [LEGO](https://go-acme.github.io/lego/) supports the `--kid` and `--hmac`
  flags. In addition, the `--eab` flag must also be set.
- [Caddy](https://caddyserver.com/docs/caddyfile/options#acme-eab) supports an
  `acme_eab` TLS Option.
