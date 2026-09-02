# evidence-anchors
Daily cryptographic anchors of Amrachi supervision-evidence chains, for independent verification.
# Amrachi — Evidence Anchors

Public, append-only record of the daily cryptographic anchors of Amrachi's
supervision-evidence chains.

It exists so that anyone — a customer, an auditor, a regulator — can
independently confirm that the supervision evidence a firm holds in Amrachi has
**not been altered since it was anchored**, without having to trust Amrachi's
word for it.

## Status

🚧 **Being provisioned.** Automated publication begins September 07th 26. Until then
this repository is a placeholder and contains no anchor data.

## What an anchor is

For each organisation, once per UTC day, Amrachi computes a SHA-256 hash over
that day's ordered supervision audit records and chains it to the previous
day's anchor. The result is a short fingerprint of that day's whole evidence
record.

Publishing that fingerprint here — in a public repository, under an
Amrachi-controlled organisation account, carrying GitHub's own commit
timestamps and with force-push protection — means the fingerprint cannot later
be changed without it being obvious.

> **Tamper-evident, not tamper-proof.** This does not prevent alteration; it
> makes alteration detectable. It says nothing about whether an underlying
> supervision decision was correct — only that the evidence record is intact
> and consistent since anchoring.

## What is *not* here

No client names, no personal data, no portfolio contents, no decision text —
only hashes, dates, counts and organisation identifiers.

## Layout

anchors/<org>/YYYY-MM-DD.json

Each file contains at least:

| Field | Meaning |
|---|---|
| `date` | UTC day covered (`YYYY-MM-DD`) |
| `org` | organisation identifier |
| `record_count` | number of audit records included |
| `anchor_sha256` | this day's anchor (64 hex chars) |
| `prev_anchor_sha256` | the previous published day's `anchor_sha256` for the same org — the chain link |
| `algo` | `sha256` |
| `published_at` | ISO-8601 timestamp of the commit that added this file |

The `prev_anchor_sha256` of each day equals the `anchor_sha256` of that org's
previous published day, forming an unbroken chain. A gap or mismatch is a
signal.

## How to verify

1. In Amrachi, open the verification view for the day or record you care about — the Verify link in an evidence export (it carries a token: amrachi.ch/verify/<token>), or try the public demo at https://amrachi.ch/verify/demo.
2. Amrachi recomputes the anchor from the live evidence and shows you the
   value.
3. Compare it to `anchor_sha256` in the file for that org and date in this
   repository.
4. Check that `prev_anchor_sha256` links to the prior day, and that the commit
   which added the file is dated on or shortly after that day.

Equal values **+** an unbroken chain **+** a contemporaneous commit date = the
evidence you were shown has not changed since it was anchored.

Full methodology: <https://amrachi.ch/regulatory-methodology>

## Integrity of this repository

- **Public** — anyone can read and clone the full history.
- `main` is protected against force-push and deletion.
- Files are written only by an automated publisher using a repository-scoped
  token; the commit history is the audit trail.

## Contact

`security@amrachi.ch`
