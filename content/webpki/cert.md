---
weight: 10
title: Anatomy of a Certificate
params:
  tabTitle: What is a digital certificate?
---

# Analogy with the Physical World

**Digital certificates are like identification documents** (passport, identity
card, driver license, employee badge, library membership card, retail store
fidelity card, etc.), but for websites. They tie a name to an identity, they
hold additional metadata, they are issued by some (more or less widely
recognized/trusted) entity, and they can be used/accepted in specific
circumstances.

Digital certificates are issued by CAs (Certificate - or Certification -
Authorities). CAs can either be public or private. Certificates issued by
publicly trusted CAs are trusted/accepted by default by major web browsers and
operating systems (like government-issued passports). Certificate issued by
private CAs are only trusted/accepted by clients who agreed to trust them (like
retail store fidelity cards).

In the same way that an identification document attests that a person has the
given name, **a digital certificate attests that a public key (and its
corresponding private key) is bound to given domain names or IP addresses**.

When clients connect to a website via HTTPS, the server present its certificate.
The client verifies that the certificate contains the domain name it is
connecting to, and that it was issued by a CA it trusts. The server also sends a
message signed using its private key. The client can then verify the signature
using the public key from the certificate. The assumptions are that:

- The private key to sign the message (which is tied to the public key in the
  certificate) is only known to the server serving the specific domain.
- The CA did their due diligence when issuing the certificate to verify that the
  requester controls the included domain names.

It is worth pointing out that **certificates issued by private CAs are
<ins>not</ins> inherently less secure than certificates issued by publicly
trusted CAs**. Both use the same underlying technology and cryptographic
algorithms.

Private CAs **may** perform less rigorous validations, and their private keys
**may** have fewer protections, like the library membership or retail store
fidelity cards. But it is totally feasible to operate private CAs in a very
secure manner, like it is done with employee badges.

# Anatomy of a Certificate

The format of X.509 digital certificates is detailed in
[RFC 5280](https://datatracker.ietf.org/doc/html/rfc5280). A certificate is made
of a sequence of 3 fields:

- The
  [tbsCertificate](https://datatracker.ietf.org/doc/html/rfc5280#section-4.1.1.1)
  is the data structure representing the certificate to be signed (tbs).
- The
  [signatureAlgorithm](https://datatracker.ietf.org/doc/html/rfc5280#section-4.1.1.2)
  identifies/defines how the `signatureValue` is computed.
- The
  [signatureValue](https://datatracker.ietf.org/doc/html/rfc5280#section-4.1.1.3)
  is the digital signature computed upon the `tbsCertificate` using
  `signatureAlgorithm`.

This section only covers the `tbsCertificate` data structure of publicly trusted
DV (Domain Validated) TLS web server leaf/end-entity certificates.

{{% details title="Click here to view the example certificate being dissected." open=false %}}

We are only going to look at the content of the `Data:` field, which corresponds
to the `tbsCertificate`.

```
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            02:71:83:d4:a8:25:79:07:0a:3d:8f:60:9e:27:cd:e5
        Signature Algorithm: ecdsa-with-SHA256
        Issuer: C = US, O = Google Trust Services, CN = WE2
        Validity
            Not Before: Mar 31 08:56:27 2025 GMT
            Not After : Jun 23 08:56:26 2025 GMT
        Subject: CN = www.google.com
        Subject Public Key Info:
            Public Key Algorithm: id-ecPublicKey
                Public-Key: (256 bit)
                pub:
                    04:11:e3:07:97:e4:71:0c:5f:61:38:c3:56:6b:cc:
                    e0:9a:6b:e1:18:63:2f:e9:87:e3:2c:a4:f2:54:ef:
                    a1:d5:fd:06:c1:70:4c:cc:1b:69:9d:c0:1f:ba:cf:
                    7f:d3:ef:ea:cd:ef:33:d0:81:19:67:30:0e:45:59:
                    6c:35:d2:e0:d4
                ASN1 OID: prime256v1
                NIST CURVE: P-256
        X509v3 extensions:
            X509v3 Key Usage: critical
                Digital Signature
            X509v3 Extended Key Usage:
                TLS Web Server Authentication
            X509v3 Basic Constraints: critical
                CA:FALSE
            X509v3 Subject Key Identifier:
                DC:A3:F6:5E:4C:F2:71:46:51:A4:50:98:91:8B:FC:20:EF:29:70:45
            X509v3 Authority Key Identifier:
                75:BE:C4:77:AE:89:F6:44:37:7D:CF:B1:68:1F:1D:1A:EB:DC:34:59
            Authority Information Access:
                OCSP - URI:http://o.pki.goog/we2
                CA Issuers - URI:http://i.pki.goog/we2.crt
            X509v3 Subject Alternative Name:
                DNS:www.google.com
            X509v3 Certificate Policies:
                Policy: 2.23.140.1.2.1
            X509v3 CRL Distribution Points:
                Full Name:
                  URI:http://c.pki.goog/we2/xuzt3PU9F_w.crl
            CT Precertificate SCTs:
                Signed Certificate Timestamp:
                    Version   : v1 (0x0)
                    Log ID    : CF:11:56:EE:D5:2E:7C:AF:F3:87:5B:D9:69:2E:9B:E9:
                                1A:71:67:4A:B0:17:EC:AC:01:D2:5B:77:CE:CC:3B:08
                    Timestamp : Mar 31 09:56:28.830 2025 GMT
                    Extensions: none
                    Signature : ecdsa-with-SHA256
                                30:45:02:21:00:C7:F7:8F:00:6E:B4:79:C5:9C:7A:35:
                                69:4C:7D:7B:E1:1B:36:C8:0F:D9:6C:C2:63:28:4C:DE:
                                8F:47:D7:72:74:02:20:20:2D:9B:F5:90:5B:74:A6:8D:
                                3B:B6:99:65:3A:F9:3F:2C:0C:F1:F1:1B:4E:F6:5D:C8:
                                0C:7C:6C:D2:AE:C0:5D
                Signed Certificate Timestamp:
                    Version   : v1 (0x0)
                    Log ID    : A2:E3:0A:E4:45:EF:BD:AD:9B:7E:38:ED:47:67:77:53:
                                D7:82:5B:84:94:D7:2B:5E:1B:2C:C4:B9:50:A4:47:E7
                    Timestamp : Mar 31 09:56:29.795 2025 GMT
                    Extensions: none
                    Signature : ecdsa-with-SHA256
                                30:46:02:21:00:8B:BB:DB:F5:A1:0F:EF:2D:CA:E0:57:
                                63:22:7B:B2:A5:21:4D:7B:AE:AE:CE:0C:2C:92:2A:0D:
                                C4:F6:63:A5:12:02:21:00:D0:39:B3:EF:C0:29:FA:03:
                                DB:9D:48:49:F9:48:9F:C5:1F:23:43:6B:41:5F:B2:76:
                                EF:07:C7:EA:C6:DF:97:76
    Signature Algorithm: ecdsa-with-SHA256
    Signature Value:
        30:44:02:20:64:e4:a1:aa:cf:67:8d:77:b3:e5:67:a7:fd:bb:
        aa:65:62:99:b1:7c:39:ec:13:e2:75:2a:23:1d:42:79:22:1a:
        02:20:24:5d:e6:e0:84:18:0a:73:47:9b:06:e7:4e:80:9d:5b:
        30:ac:f2:9d:32:d8:97:1f:b4:6a:74:e1:a2:c5:b5:ee
-----BEGIN CERTIFICATE-----
MIIDlTCCAzygAwIBAgIQAnGD1KgleQcKPY9gnifN5TAKBggqhkjOPQQDAjA7MQsw
CQYDVQQGEwJVUzEeMBwGA1UEChMVR29vZ2xlIFRydXN0IFNlcnZpY2VzMQwwCgYD
VQQDEwNXRTIwHhcNMjUwMzMxMDg1NjI3WhcNMjUwNjIzMDg1NjI2WjAZMRcwFQYD
VQQDEw53d3cuZ29vZ2xlLmNvbTBZMBMGByqGSM49AgEGCCqGSM49AwEHA0IABBHj
B5fkcQxfYTjDVmvM4Jpr4RhjL+mH4yyk8lTvodX9BsFwTMwbaZ3AH7rPf9Pv6s3v
M9CBGWcwDkVZbDXS4NSjggJCMIICPjAOBgNVHQ8BAf8EBAMCB4AwEwYDVR0lBAww
CgYIKwYBBQUHAwEwDAYDVR0TAQH/BAIwADAdBgNVHQ4EFgQU3KP2XkzycUZRpFCY
kYv8IO8pcEUwHwYDVR0jBBgwFoAUdb7Ed66J9kQ3fc+xaB8dGuvcNFkwWAYIKwYB
BQUHAQEETDBKMCEGCCsGAQUFBzABhhVodHRwOi8vby5wa2kuZ29vZy93ZTIwJQYI
KwYBBQUHMAKGGWh0dHA6Ly9pLnBraS5nb29nL3dlMi5jcnQwGQYDVR0RBBIwEIIO
d3d3Lmdvb2dsZS5jb20wEwYDVR0gBAwwCjAIBgZngQwBAgEwNgYDVR0fBC8wLTAr
oCmgJ4YlaHR0cDovL2MucGtpLmdvb2cvd2UyL3h1enQzUFU5Rl93LmNybDCCAQUG
CisGAQQB1nkCBAIEgfYEgfMA8QB2AM8RVu7VLnyv84db2Wkum+kacWdKsBfsrAHS
W3fOzDsIAAABleuhkB4AAAQDAEcwRQIhAMf3jwButHnFnHo1aUx9e+EbNsgP2WzC
YyhM3o9H13J0AiAgLZv1kFt0po07tpllOvk/LAzx8RtO9l3IDHxs0q7AXQB3AKLj
CuRF772tm3447Udnd1PXgluElNcrXhssxLlQpEfnAAABleuhk+MAAAQDAEgwRgIh
AIu72/WhD+8tyuBXYyJ7sqUhTXuurs4MLJIqDcT2Y6USAiEA0Dmz78Ap+gPbnUhJ
+UifxR8jQ2tBX7J27wfH6sbfl3YwCgYIKoZIzj0EAwIDRwAwRAIgZOShqs9njXez
5Wen/buqZWKZsXw57BPidSojHUJ5IhoCICRd5uCEGApzR5sG506AnVswrPKdMtiX
H7RqdOGixbXu
-----END CERTIFICATE-----
```

{{% /details %}}

## Version

```
        Version: 3 (0x2)
```

All digital certificates use the X.509 version 3. The key addition to X.509
version 3 is the support for extensions. Version 3 was standardized in 1999 in
[RFC 2459](https://datatracker.ietf.org/doc/html/rfc2459#section-3.1), which is
the precursor of RFC 5280.

## Serial Number

```
        Serial Number:
            02:71:83:d4:a8:25:79:07:0a:3d:8f:60:9e:27:cd:e5
```

The serial number uniquely identifies the certificate. CRLs (Certificate
Revocation Lists) reference revoked certificates by serial number. Serial
numbers are unique across all certificates issued by a CA.

## Signature Algorithm

```
        Signature Algorithm: ecdsa-with-SHA256
```

The inner signature algorithm in the `tbsCertificate` data structure has the
same value as the outer one. The inner signature algorithm is part of the signed
data (to guarantee its integrity), while the outer one isn't. When decoding a
certificate, the signature of the raw `tbsCertificate` is verified before
decoding it. This is why the signature algorithm must also be set outside of the
`tbsCertifiate`.

## Issuer

```
        Issuer: C = US, O = Google Trust Services, CN = WE2
```

The issuer holds the value of the subject field of the issuing CA certificate.
It is used to Facilitate the construction of the certificate chain.

The following command downloads the issuing CA certificate and extracts its
subject field.

```bash
$ curl -s http://i.pki.goog/we2.crt | openssl x509 -inform DER -noout -subject
subject=C = US, O = Google Trust Services, CN = WE2
```

## Validity

```
        Validity
            Not Before: Mar 31 08:56:27 2025 GMT
            Not After : Jun 23 08:56:26 2025 GMT
```

The validity defines the timestamps between which the certificate is valid. The
`notBefore` timestamp is usually backdated by a few minutes to ensure the
certificate is recognized as immediately valid, even by machines not having an
accurate clock. Refer to
[the page dedicated to certificate validities](/webpki/cert-lifetime/) for more
information.

## Subject

```
        Subject: CN = www.google.com
```

The subject is a is made of a sequence of key-value pairs. `CN` stands for
`commonName`. This is where the domain name was **historically** set. **The
subject field is long deprecated for DV TLS web server leaf certificates**.
Nowadays, domain names are set in the `Subject Alternative Name` extension.

Modern clients should ignore the subject field of DV TLS web server leaf
certificates. Some CAs still set it to ensure that very outdated clients (not
aware that the `Subject Alternative Name` should be used instead) are still able
to process the certificate.

OV (Organization Validated), EV (Extended Validation) and QWAC (Qualified Web
Authentication Certificate) certificates set additional key-value pairs in the
subject field, like for example `C=` (`countryName`) or `O=`
(`organizationName`).

## Subject Public Key Info

```
        Subject Public Key Info:
            Public Key Algorithm: id-ecPublicKey
                Public-Key: (256 bit)
                pub:
                    04:11:e3:07:97:e4:71:0c:5f:61:38:c3:56:6b:cc:
                    e0:9a:6b:e1:18:63:2f:e9:87:e3:2c:a4:f2:54:ef:
                    a1:d5:fd:06:c1:70:4c:cc:1b:69:9d:c0:1f:ba:cf:
                    7f:d3:ef:ea:cd:ef:33:d0:81:19:67:30:0e:45:59:
                    6c:35:d2:e0:d4
                ASN1 OID: prime256v1
                NIST CURVE: P-256
```

The subject public key info carries the public key and identifies the algorithm
with which the key is used.

## Extensions

Extensions may be marked as **critical**. Critical extensions must be processed
by clients. Clients must refuse certificates having critical extensions they do
not recognize.

### Key Usage

```
            X509v3 Key Usage: critical
                Digital Signature
```

The key usage extension defines the purpose of the key. The server must prove to
clients that it possesses the private key corresponding to the public key in the
certificate. When using modern TLS ciphers, the server
[proves possession of the private key by digitally signing data](https://datatracker.ietf.org/doc/html/rfc8446#section-4.4.3).
This is why "Digital Signature" is the only purpose of the key.

### Extended Key Usage {#eku}

```
            X509v3 Extended Key Usage:
                TLS Web Server Authentication
```

The extended key usage extension indicates the purposes for which the
certificate may be used. For website certificates, "TLS Web Server
Authentication" (`id-kp-serverAuth`) must be set.

### Basic Constraints

```
            X509v3 Basic Constraints: critical
                CA:FALSE
```

The basic constraints extension indicates whether the certificate is a CA
certificate. In a perfect world, this extensions shouldn't need to be explicitly
set to `CA:FALSE` in leaf certificates since this is the default value.

But, as demonstrated in
[this BlackHat talk from 2009](https://www.blackhat.com/presentations/bh-dc-09/Marlinspike/BlackHat-DC-09-Marlinspike-Defeating-SSL.pdf),
some clients may not validate certificate chains properly, so some CAs prefer to
keep this extension to avoid introducing very nasty vulnerabilities in such
clients.

### Subject Key Identifier

```
            X509v3 Subject Key Identifier:
                DC:A3:F6:5E:4C:F2:71:46:51:A4:50:98:91:8B:FC:20:EF:29:70:45
```

The subject key identifier extension holds a value derived from the public key,
usually by computing its hash.

This information is very important in CA certificates because it facilitates the
construction of the certificate chain since all certificates must also include
the "authority key identifier" extension.

This extension isn't strictly needed in leaf certificates since it isn't used to
validate the certificate chain. One reason to include it anyway is to easily
find all certificates bound to the same public key.

### Authority Key Identifier

```
            X509v3 Authority Key Identifier:
                75:BE:C4:77:AE:89:F6:44:37:7D:CF:B1:68:1F:1D:1A:EB:DC:34:59
```

The authority key identifier extension references the subject key identifier of
the issuing CA certificate. It is used to facilitate the construction of the
certificate chain.

The following command downloads the issuing CA certificate and extracts its
subject key identifier extension.

```bash
$ curl -s http://i.pki.goog/we2.crt | openssl x509 -inform DER -noout -ext subjectKeyIdentifier
X509v3 Subject Key Identifier:
    75:BE:C4:77:AE:89:F6:44:37:7D:CF:B1:68:1F:1D:1A:EB:DC:34:59
```

### Authority Information Access {#aia}

```
            Authority Information Access:
                OCSP - URI:http://o.pki.goog/we2
                CA Issuers - URI:http://i.pki.goog/we2.crt
```

The authority information access (AIA) extension indicates how to access
information and services for the issuer of the certificate. Here the extension
holds two URLs:

- The
  [OCSP (Online Certificate Status Protocol)](https://datatracker.ietf.org/doc/html/rfc6960)
  URL used to check whether the certificate is revoked. Note that
  [OCSP is being deprecated](https://letsencrypt.org/2024/12/05/ending-ocsp/).
- The URL to download the issuing CA certificate. Note that clients should not
  rely on it to build the certificate chain since the issuing CA certificate may
  have been
  [cross-signed](https://scotthelme.co.uk/cross-signing-alternate-trust-paths-how-they-work/).
  Instead, websites should serve the full certificate chain to their visitors.

### Subject Alternative Name {#san}

```
            X509v3 Subject Alternative Name:
                DNS:www.google.com
```

The subject alternative name (SAN) extension lists all identifiers (domain names
and IP addresses) for which this certificate can be used.

### Certificate Policies

```
            X509v3 Certificate Policies:
                Policy: 2.23.140.1.2.1
```

The `certificatePolicies` indicate the policy under which the certificate has
been issued and the purposes for which the certificate may be used. The OID
(Object Identifier) `2.23.140.1.2.1` signifies
"[CA/B Forum](https://cabforum.org) DV (Domain Validated) Certificate".

### CRL Distribution Points

```
            X509v3 CRL Distribution Points:
                Full Name:
                  URI:http://c.pki.goog/we2/xuzt3PU9F_w.crl
```

The CRL distribution points extension references the URL of the CRL (Certificate
Revocation List) for that certificate. CRLs lists all revoked certificates by a
CA. To limit the file size in case many certificates are revoked, some CAs
partition their CRLs in multiple files.

If the certificate is revoked, it will appear on the linked CRL file.

### CT Precertificate SCTs {#sct}

```
            CT Precertificate SCTs:
                Signed Certificate Timestamp:
                    Version   : v1 (0x0)
                    Log ID    : CF:11:56:EE:D5:2E:7C:AF:F3:87:5B:D9:69:2E:9B:E9:
                                1A:71:67:4A:B0:17:EC:AC:01:D2:5B:77:CE:CC:3B:08
                    Timestamp : Mar 31 09:56:28.830 2025 GMT
                    Extensions: none
                    Signature : ecdsa-with-SHA256
                                30:45:02:21:00:C7:F7:8F:00:6E:B4:79:C5:9C:7A:35:
                                69:4C:7D:7B:E1:1B:36:C8:0F:D9:6C:C2:63:28:4C:DE:
                                8F:47:D7:72:74:02:20:20:2D:9B:F5:90:5B:74:A6:8D:
                                3B:B6:99:65:3A:F9:3F:2C:0C:F1:F1:1B:4E:F6:5D:C8:
                                0C:7C:6C:D2:AE:C0:5D
                Signed Certificate Timestamp:
                    Version   : v1 (0x0)
                    Log ID    : A2:E3:0A:E4:45:EF:BD:AD:9B:7E:38:ED:47:67:77:53:
                                D7:82:5B:84:94:D7:2B:5E:1B:2C:C4:B9:50:A4:47:E7
                    Timestamp : Mar 31 09:56:29.795 2025 GMT
                    Extensions: none
                    Signature : ecdsa-with-SHA256
                                30:46:02:21:00:8B:BB:DB:F5:A1:0F:EF:2D:CA:E0:57:
                                63:22:7B:B2:A5:21:4D:7B:AE:AE:CE:0C:2C:92:2A:0D:
                                C4:F6:63:A5:12:02:21:00:D0:39:B3:EF:C0:29:FA:03:
                                DB:9D:48:49:F9:48:9F:C5:1F:23:43:6B:41:5F:B2:76:
                                EF:07:C7:EA:C6:DF:97:76
```

SCTs (Signed Certificate Timestamps) are promises from CT (Certificate
Transparency) logs to include the (pre)certificate. These are SCTs from "[Google
'Xenon2025h1' log]" and "[Let's Encrypt 'Oak2025h1']", respectively. We can
verify that by
[converting the hex-encoded Log ID to base64](<https://gchq.github.io/CyberChef/#recipe=Find_/_Replace(%7B'option':'Regex','string':'Log%20ID%20*:'%7D,'',true,true,false,false)From_Hex('Auto')To_Base64('A-Za-z0-9%2B/%3D')>)
and looking for that "`log_id`" in
https://www.gstatic.com/ct/log_list/v3/log_list.json.

See [the page dedicated to Certificate Transparency](/webpki/ct/) for more
information.

[Google 'Xenon2025h1' log]:
  https://gchq.github.io/CyberChef/#recipe=Find_/_Replace(%7B'option':'Regex','string':'Log%20ID%20*:'%7D,'',true,true,false,false)From_Hex('Auto')To_Base64('A-Za-z0-9%2B/%3D')&input=ICAgICAgICAgICAgICAgICAgICBMb2cgSUQgICAgOiBDRjoxMTo1NjpFRTpENToyRTo3QzpBRjpGMzo4Nzo1QjpEOTo2OToyRTo5QjpFOToKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAxQTo3MTo2Nzo0QTpCMDoxNzpFQzpBQzowMTpEMjo1Qjo3NzpDRTpDQzozQjowOA
[Let's Encrypt 'Oak2025h1']:
  https://gchq.github.io/CyberChef/#recipe=Find_/_Replace(%7B'option':'Regex','string':'Log%20ID%20*:'%7D,'',true,true,false,false)From_Hex('Auto')To_Base64('A-Za-z0-9%2B/%3D')&input=ICAgICAgICAgICAgICAgICAgICBMb2cgSUQgICAgOiBBMjpFMzowQTpFNDo0NTpFRjpCRDpBRDo5Qjo3RTozODpFRDo0Nzo2Nzo3Nzo1MzoKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBENzo4Mjo1Qjo4NDo5NDpENzoyQjo1RToxQjoyQzpDNDpCOTo1MDpBNDo0NzpFNw

# Encoding

Certificates are
[ASN.1](https://letsencrypt.org/docs/a-warm-welcome-to-asn1-and-der/) data
structures. Certificates are generally encoded using either DER (Distinguished
Encoding Rules) or PEM (Privacy-Enhanced Mail).

DER is an unambiguous (as opposed to BER (Basic Encoding Rules)) binary encoding
format for representing ASN.1 data structures as a sequence of bytes.

PEM is a text-based encoding format, which is essentially a wrapper around the
DER encoding, making it suitable for transmission in text-based systems (like
email, as its name suggests) and for easy copy-pasting. PEM certificates are
made of:

- A "`-----BEGIN CERTIFICATE-----`" header line.
- The DER-encoded certificate data, converted to base64.
- A "`-----END CERTIFICATE-----`" footer line.

You can take the base64 DER-encoded certificate data and decode it using an
ASN.1 decoder such as https://lapo.it/asn1js/
([example](https://lapo.it/asn1js/#MIIDlTCCAzygAwIBAgIQAnGD1KgleQcKPY9gnifN5TAKBggqhkjOPQQDAjA7MQswCQYDVQQGEwJVUzEeMBwGA1UEChMVR29vZ2xlIFRydXN0IFNlcnZpY2VzMQwwCgYDVQQDEwNXRTIwHhcNMjUwMzMxMDg1NjI3WhcNMjUwNjIzMDg1NjI2WjAZMRcwFQYDVQQDEw53d3cuZ29vZ2xlLmNvbTBZMBMGByqGSM49AgEGCCqGSM49AwEHA0IABBHjB5fkcQxfYTjDVmvM4Jpr4RhjL-mH4yyk8lTvodX9BsFwTMwbaZ3AH7rPf9Pv6s3vM9CBGWcwDkVZbDXS4NSjggJCMIICPjAOBgNVHQ8BAf8EBAMCB4AwEwYDVR0lBAwwCgYIKwYBBQUHAwEwDAYDVR0TAQH_BAIwADAdBgNVHQ4EFgQU3KP2XkzycUZRpFCYkYv8IO8pcEUwHwYDVR0jBBgwFoAUdb7Ed66J9kQ3fc-xaB8dGuvcNFkwWAYIKwYBBQUHAQEETDBKMCEGCCsGAQUFBzABhhVodHRwOi8vby5wa2kuZ29vZy93ZTIwJQYIKwYBBQUHMAKGGWh0dHA6Ly9pLnBraS5nb29nL3dlMi5jcnQwGQYDVR0RBBIwEIIOd3d3Lmdvb2dsZS5jb20wEwYDVR0gBAwwCjAIBgZngQwBAgEwNgYDVR0fBC8wLTAroCmgJ4YlaHR0cDovL2MucGtpLmdvb2cvd2UyL3h1enQzUFU5Rl93LmNybDCCAQUGCisGAQQB1nkCBAIEgfYEgfMA8QB2AM8RVu7VLnyv84db2Wkum-kacWdKsBfsrAHSW3fOzDsIAAABleuhkB4AAAQDAEcwRQIhAMf3jwButHnFnHo1aUx9e-EbNsgP2WzCYyhM3o9H13J0AiAgLZv1kFt0po07tpllOvk_LAzx8RtO9l3IDHxs0q7AXQB3AKLjCuRF772tm3447Udnd1PXgluElNcrXhssxLlQpEfnAAABleuhk-MAAAQDAEgwRgIhAIu72_WhD-8tyuBXYyJ7sqUhTXuurs4MLJIqDcT2Y6USAiEA0Dmz78Ap-gPbnUhJ-UifxR8jQ2tBX7J27wfH6sbfl3YwCgYIKoZIzj0EAwIDRwAwRAIgZOShqs9njXez5Wen_buqZWKZsXw57BPidSojHUJ5IhoCICRd5uCEGApzR5sG506AnVswrPKdMtiXH7RqdOGixbXu)).
