# Catena-X Verifiable Credential Profile Specification

This document defines the Catena-X Verifiable Credential Profile used
with [The Identity and Trust Protocol (IATP)](https://github.com/eclipse-tractusx/identity-trust).

## 1. Interoperable Credential Data Model and Encodings

Verifiable Credentials (VC) are based on the [W3C VC Data Model](https://www.w3.org/TR/vc-data-model/#dfn-holders).
The interoperable encoding format for VCs and Verifiable Presentations (VP) is JWT `ES256K/secp256k1` as defined in
[CBOR Object Signing and Encryption (COSE) and JSON Object Signing and Encryption (JOSE) Registrations for Web Authentication (WebAuthn) Algorithms](https://datatracker.ietf.org/doc/html/rfc8812#section-3.1).

VCs MUST contain an `issuanceDate` and MAY `expirationDate`. JWTs MUST contain an `iat` claim equal to
the `issuanceDate` and an `exp` equal to the `expirationDate`, if present.

## 2. Interoperable Credential Types

The following credential types MUST be supported by participants.

### 2.1. The `BpnCredential` Type

Specified in [BPN Credential Schema](../schema/credentials/bpn.credential.schema.json).

### 2.2. The `DismantlerCredential` Type

Specified in [Dismantler Credential Schema](../schema/credentials/dismantler.credential.schema.json).

### 2.3. The `MembershipCredential` Type

Specified in [Membership Credential Schema](../schema/credentials/membership.credential.schema.json).

### 2.4. Framework Credential Types

#### 2.4.1. The `BehavioralTwinCredential` Type

Specified in [Behavioral Twin Credential Schema](../schema/credentials/behavioral.twin.credential.schema.json).

#### 2.4.2. The `PcfCredential` Type

Specified in [PCF Credential Schema](../schema/credentials/pcf.credential.schema.json).

#### 2.4.3. The `QualityCredential` Type

Specified in [Quality Credential Schema](../schema/credentials/quality.credential.schema.json).

#### 2.4.4. The `ResiliencyCredential` Type

Specified in [Resiliency Credential Schema](../schema/credentials/resiliency.credential.schema.json).

#### 2.4.5. The `SustainabilityCredential` Type

Specified in [Sustainability Credential Schema](../schema/credentials/sustainability.credential.schema.json).

#### 2.4.6. The `TraceabilityCredential` Type

Specified in [Traceability Credential Schema](../schema/credentials/traceability.credential.schema.json).
