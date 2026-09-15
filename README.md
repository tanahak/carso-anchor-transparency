# Carso Anchor Transparency Log

This repository is a public, append-only mirror of the daily data-integrity anchors
published by Carso Cybernetics LLC.

Each file under `anchors/` is the exact JSON served by the public verification
endpoint on that date:

    https://engine.carso.exchange/verify/snapshot/<YYYY-MM-DD>

Each anchor records a Merkle root over that snapshot's artifact manifest, the
Merkle rule used to build it, and the SHA-256 hashes of RFC 3161 timestamp tokens
issued for that root by two independent time-stamping authorities (FreeTSA and
DigiCert).

## Why this repository exists

The RFC 3161 tokens prove, independently of Carso, that each root existed no
later than the token's timestamp. This repository adds a second, independent
property: **equivocation resistance**. Because the roots are committed here, in a
venue with public commit history that Carso cannot silently rewrite, any attempt
to present a different history of roots for the same dates becomes detectable by
comparing against this log.

## How to verify

1. Fetch an anchor file here and the same date from the public endpoint; they
   should match.
2. Verify the RFC 3161 tokens against the recorded root using standard
   `openssl ts -verify` tooling and the issuing authorities' certificate chains.
3. Anchors using rule `rfc6962-v2` build the Merkle tree per RFC 6962 (leaf
   prefix `0x00`, node prefix `0x01`, odd node promoted). The earliest anchor
   (seq 78019, 2026-09-15) predates the rule field and was built under the
   prior rule (unprefixed leaves, duplicated odd node); it is recorded here
   as-served and is not reinterpreted.
4. From seq 78121 onward, each anchor's manifest commits the timestamp tokens of
   the prior anchors as leaves, chaining the anchors so that each root is also
   provably assembled no earlier than the previous anchors' timestamp moments.

Files in this repository are added by an automated job at anchor time and are
never modified after commit. The git history is the record.

Carso Cybernetics LLC
