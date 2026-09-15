# Carso Anchor Transparency Log

## What is this?

Carso Cybernetics builds systems that keep verifiable records for the freight
industry — records that drivers, carriers, brokers, and courts can check for
themselves instead of taking anyone's word.

Every day (and on every significant data event), Carso's systems compute a
cryptographic **fingerprint** of the data they hold — a Merkle root — and have
that fingerprint **timestamped by two independent time-stamping authorities**
(FreeTSA and DigiCert) under RFC 3161, the same standard used for legally
recognized digital timestamps. Those authorities are not Carso: their stamps
prove a fingerprint existed by a given moment, and Carso cannot forge or
backdate them.

This repository is the **public mirror of those fingerprints**. Each file under
`anchors/` is the exact record served by Carso's public verification endpoint
for that date.

## Why publish it here?

The timestamps prove *when*. This repository proves *there is only one
history*. Because every day's fingerprints are committed here — in a public git
history that Carso cannot silently rewrite — anyone can detect if a different
version of the record were ever presented anywhere else. Between the two
independent timestamp authorities and this public log, faking or rewriting a
record would require simultaneously fooling three parties Carso does not
control.

## What this repository does NOT contain

Only fingerprints (SHA-256 hashes), timestamps, counts, and a public
verification key. No freight data, no personal data, no file names, no
business records, and no secret keys of any kind. A fingerprint is one-way:
the underlying data cannot be recovered from it.

## How to verify

1. Fetch any date's file here and the same date from the live endpoint —
   `https://engine.carso.exchange/verify/snapshot/<YYYY-MM-DD>` — they should
   match, and this repo's git history shows the record has not changed since
   it was committed.
2. Verify the RFC 3161 tokens against the recorded root with standard
   `openssl ts -verify` tooling and the issuing authorities' certificate
   chains.
3. Anchors marked `rfc6962-v2` build the Merkle tree per RFC 6962 (leaf prefix
   `0x00`, node prefix `0x01`, odd node promoted). The earliest anchor
   (seq 78019, 2026-09-15) predates the rule field and used the prior rule
   (unprefixed leaves, duplicated odd node); it is recorded as-served and
   never reinterpreted.
4. From seq 78121 onward, each anchor's manifest commits the timestamp tokens
   of the prior anchors as leaves — chaining the anchors so each fingerprint is
   also provably assembled **no earlier** than the previous anchors' timestamp
   moments, giving every anchor a two-sided, third-party time bracket.

Files are added by an automated job at anchor time and never modified after
commit. The git history is the record.

—

**Carso Cybernetics LLC** · https://carsocybernetics.com · Verification
endpoint: https://engine.carso.exchange/verify/snapshot/2026-09-15
