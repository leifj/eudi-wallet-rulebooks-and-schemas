# EWC LSP - EUDI Wallet Rulebooks and Data Schemas for Electronic Attestations of Attributes

This repository contains the rulebooks and data schemas used in the EWC Large Scale Pilot use cases, as a reference for interoperability, governance and compliance in digital identity frameworks. Its layout is the one that registry-cli reads with its `rulebook-catalog` plugin, so the attestation types are published in the SIROS Credential Registry at <https://registry.siros.org/ewc-consortium/> without conversion. See [docs/registry-publishing.md](docs/registry-publishing.md).

Authors create new rulebooks and data schemas on a separate branch. Once agreed within the work package, they raise a pull request to `main`. The `Validate schemas and registry build` check builds the repository with registry-cli and reports any problem before merge.

**Note:** The status **Approved** for rulebooks and data schemas applies to EWC LSP phase 02.

## Layout

```
data-schemas/
  sd-jwt/dsNNN-<slug>-sd-jwt.json   one SD-JWT VC claim schema per attestation type
  mdoc/dsNNN-<slug>-mdoc.json       optional mdoc schema for the same type
  other/                            schemas that do not describe an attestation
rulebooks/
  rb-<slug>/README.md               the rulebook of the attestation type with the same slug
assets/                             optional card artwork per slug
```

## Naming conventions

### Data schemas

1. A data schema for an attestation type is `data-schemas/sd-jwt/dsNNN-<slug>-sd-jwt.json`. `NNN` is the next free number in `main`, and `<slug>` is lower-case words joined by hyphens.
2. The schema describes the SD-JWT VC payload: a flat set of top-level claims, `properties.vct.const` with the credential type, `title`, `description`, `version` and JSON Schema 2020-12 in `$schema`. Revocation is the SD-JWT VC `status` claim.

### Rulebooks

1. A rulebook is `rulebooks/rb-<slug>/README.md`, with the same `<slug>` as its data schema. The rulebook number `RB-NNN` is part of its title.
2. The attribute tables of the rulebook list the same attributes, descriptions and required flags as the data schema.

## Attestation types

| **Schema #** | **Title** | **vct** | **Version** | **Data schema** | **Rulebook** |
| --- | --- | --- | --- | --- | --- |
| DS-001 | EU Company Certificate | eu.aptitude.eu-company-certificate.1 | 1.0.0 | [schema](data-schemas/sd-jwt/ds001-eu-company-certificate-sd-jwt.json) | [RB-002](rulebooks/rb-eu-company-certificate/README.md) |
| DS-002 | IBAN Attestation | eu.aptitude.iban-attestation.1 | 1.0.0 | [schema](data-schemas/sd-jwt/ds002-iban-attestation-sd-jwt.json) | [RB-003](rulebooks/rb-iban-attestation/README.md) |
| DS-003 | Signatory Rights Attestation | eu.aptitude.signatory-rights-attestation.1 | 1.0.0 | [schema](data-schemas/sd-jwt/ds003-signatory-rights-attestation-sd-jwt.json) | [RB-004](rulebooks/rb-signatory-rights-attestation/README.md) |
| DS-004 | Legal Person Identification Data | eu.aptitude.legal-person-identification-data.1 | 1.0.0 | [schema](data-schemas/sd-jwt/ds004-legal-person-identification-data-sd-jwt.json) | [RB-001](rulebooks/rb-legal-person-identification-data/README.md) |
| DS-005 | Ferry Boarding Pass | VerifiableFerryBoardingPassCredentialSDJWT | 0.9.0 | [schema](data-schemas/sd-jwt/ds005-ferry-boarding-pass-sd-jwt.json) | - |
| DS-006 | Ultimate Beneficial Owners Attestation | eu.aptitude.ultimate-beneficial-owners-attestation.1 | 0.9.0 | [schema](data-schemas/sd-jwt/ds006-ultimate-beneficial-owners-attestation-sd-jwt.json) | [RB-005](rulebooks/rb-ultimate-beneficial-owners-attestation/README.md) |
| DS-007 | Payment Wallet Attestation | PaymentWalletAttestation | 0.9.0 | [schema](data-schemas/sd-jwt/ds007-payment-wallet-attestation-sd-jwt.json) | - |
| DS-009 | Personal Contact Details | eu.europa.ec.eudi.pcd.1 | 0.9.0 | [schema](data-schemas/sd-jwt/ds009-personal-contact-details-document-sd-jwt.json) | - |
| DS-010 | Student ID | VerifiableStudentIDSDJWT | 0.9.0 | [schema](data-schemas/sd-jwt/ds010-student-id-sd-jwt.json) | - |
| DS-011 | vReceipt | VerifiablevReceiptSDJWT | 0.9.0 | [schema](data-schemas/sd-jwt/ds011-vReceipts-sd-jwt.json) | - |
| DS-012 | Person Identification Data | urn:eudi:pid:aptitude:1 | 1.0.0 | [schema](data-schemas/sd-jwt/ds012-person-identification-data-sd-jwt.json) | - |
| DS-013 | Photo ID | eu.europa.ec.eudi.photoid.1 | 1.0.0 | [schema](data-schemas/sd-jwt/ds013-photo-id-sd-jwt.json) | - |
| DS-014 | ID Proofing ETSI 119 461 | eu.aptitude.idproofing-etsi461.1 | 1.0.0 | [schema](data-schemas/sd-jwt/ds014-idproofing-etsi461-sd-jwt.json) | [RB-006](rulebooks/rb-idproofing-etsi461/README.md) |
| DS-015 | IBAN Attestation for Individuals | eu.aptitude.iban-attestation-for-individuals.1 | 1.0.0 | [schema](data-schemas/sd-jwt/ds015-iban-attestation-for-individuals-sd-jwt.json) | [RB-007](rulebooks/rb-iban-attestation-for-individuals/README.md) |

Status is Approved for the schemas at version 1.0.0 and Under development for those at 0.9.0. The `vct` values that start with `eu.aptitude.` and `urn:eudi:pid:aptitude:1` are the identifiers of this repository; they differ from the identifiers in the WE BUILD catalogue so that a registry can tell the types apart. The other values are the identifiers the types used before.

DS-008 (Payment Data Confirmation) describes transaction data that a wallet confirms during a presentation. It is in [data-schemas/other](data-schemas/other/ds008-payment-data-confirmation.json).

## Rulebooks

| **Rulebook #** | **Rulebook Title** | **Status** |
| --- | --- | --- |
| RB-001 | [Legal Person Identification Data (LPID) Rulebook](rulebooks/rb-legal-person-identification-data/README.md) | Approved |
| RB-002 | [EU Company Certificate Data Rulebook](rulebooks/rb-eu-company-certificate/README.md) | Approved |
| RB-003 | [IBAN Data Rulebook](rulebooks/rb-iban-attestation/README.md) | Approved |
| RB-004 | [Signatory Rights Rulebook](rulebooks/rb-signatory-rights-attestation/README.md) | Approved |
| RB-005 | [Ultimate Beneficial Owners Rulebook](rulebooks/rb-ultimate-beneficial-owners-attestation/README.md) | Under development |
| RB-006 | [ID Proofing ETSI Rulebook](rulebooks/rb-idproofing-etsi461/README.md) | Approved |
| RB-007 | [IBAN Attestation for Individuals Rulebook](rulebooks/rb-iban-attestation-for-individuals/README.md) ([PDF](rulebooks/rb-iban-attestation-for-individuals/rulebook.pdf)) | Approved |
