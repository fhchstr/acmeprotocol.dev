---
weight: 13
bookToC: false
title: Merkle Tree Certificates (MTCs)
params:
  tabTitle: Merkle Tree Certificates
description:
  Learn how Merkle Tree Certificates could offer a better transition
  to post-quantum cryptography in the WebPKI and evolve the certificate
  transparency system to remain viable as certificate volumes grow.
---

# Merkle Tree Certificates (MTCs)

Merkle Tree Certificates (MTCs) represent a fundamental redesign of web
authentication, shifting away from traditional X.509 certificate chains
that rely on individual, per-certificate issuer signatures.

Instead of attaching a unique cryptographic signature to every single
certificate, a Certificate Authority (CA) signs a single cryptographic
commitment to the root or head of a Merkle tree—that can represent millions
of issued certificates simultaneously.

During a TLS handshake, the browser receives a compact "inclusion proof"
consisting of a sequence of hashes from the leaf to the tree root.

This architecture fundamentally changes how certificates are structured,
treating the cryptographic proof of inclusion as the certificate itself.

## Solving the Post-Quantum Performance Challenge

The primary catalyst driving the development of MTCs is the impending
transition to post-quantum cryptography (PQC). The currently standardized
PQC algorithms produce signatures and public keys that are much larger
than classical elliptic curve signatures—sometimes up to 40 times the size.

If traditional X.509 chains simply swapped in these quantum-resistant
algorithms, TLS handshakes could bloat from around 1,200 bytes to over
15 kilobytes, introducing severe latency and degrading page load times.
MTCs solve this issue by replacing massive per-certificate PQC signatures
with highly compact, hash-based inclusion proofs, allowing the authentication
data transmitted over the network to remain incredibly small while achieving
robust quantum resistance.

## Evolving the WebPKI Ecosystem

The deployment of MTCs, currently being standardized through the
[IETF’s PLANTS working group](https://datatracker.ietf.org/doc/draft-ietf-plants-merkle-tree-certs/)
will fundamentally evolve the Web Public-Key Infrastructure (WebPKI) by
seamlessly integrating Certificate Transparency (CT) directly into the
issuance process. In this new paradigm, it becomes impossible for a CA to
issue an MTC without inherently publishing it to an auditable, append-only
public log. Furthermore, MTCs introduce "landmark"—specific subtree hashes
that are periodically pre-distributed to browsers out-of-band, much like
routine browser updates. When a client already possesses a landmark hash,
a web server can present a "signatureless" certificate relying entirely on
the inclusion proof, streamlining certificate validation.
