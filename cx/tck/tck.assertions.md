# Catena-X Profile Assertions

## Overview

The Catena-X Test Compatibility Kit (CX TCK) will be built using the official Eclipse Dataspace Working Group DSP and
DCP TCKs. The core framework for these TCKs is currently
hosted [here](https://github.com/orgs/eclipse-tractusx/projects/26/views/24?sliceBy%5Bvalue%5D=edc&pane=issue&itemId=58394874).

The CX TCK will extend the DSP/DCP TCKs to include ODRL Policy and Verifiable Credential (VC) assertions for the
Catena-X Dataspace Profile (CX Profile) defined [here](https://github.com/eclipse-tractusx/tractusx-profiles).

Note that consumer assertions (the verification of a connector fulfilling a data consumer role) are not included in this
concept due to the inability to verify consumer behavior in an automated fashion.

Note that contract negotiation assertions are already covered by the DSP TCK.

Note that data transfer tests are beyond the scope of the CX TCK.

## Provider Assertions

Provider assertions verify a connector fulfilling a data provider role. The provider connecter must be "primed" (or
configured) with test data. Since connector configuration is implementation-specific, this process is the responsibility
of the entity conducting the test. The TCK will have extensibility points that enable automated configuration by
supplying custom setup code. The CX TCK will supply a test client that exercises the assertions defined in this section.

The following assertions will be tested.

### 1. Membership Policy

#### Setup

The provider connector will be primed with a dataset that requires fulfillment of the `Membership` policy and the
presentation of a C-X Membership Credential:

```
{
  "constraint": {
    "leftOperand": "Membership",
    "operator": "eq",
    "rightOperand": "active"
  }
}
```

### 1. 1. Positive Membership Test

The test client will request a DSP catalog from the provider using the DSP/DCP protocol combination and provide a
valid [Membership Credential](https://github.com/eclipse-tractusx/tractusx-profiles/blob/main/cx/credentials/schema/credentials/membership.credential.schema.json).

#### Assertion

The assertion will verify that a valid `DCAT 3` catalog is returned containing an `Offer` for the dataset.

### 1. 2. Negative No Membership Credential

The test client will request a DSP catalog from the provider using the DSP/DCP protocol combination and not provide
a `Membership Credential`.

#### Assertion

The assertion will verify that an `HTTP 4XX` error is returned.

### 1. 3. Negative Expired Membership Credential

The test client will request a DSP catalog from the provider using the DSP/DCP protocol combination and provide an
expired `Membership Credential`.

#### Assertion

The assertion will verify that an `HTTP 4XX` error is returned.

### 1. 4. Negative Invalid DID Membership Credential

The test client will request a DSP catalog from the provider using the DSP/DCP protocol combination and provide
a `Membership Credential` with an invalid `DID` for the `id` entry.

#### Assertion

The assertion will verify that an `HTTP 4XX` error is returned.

### 1. 5. Negative Invalid Holder Identifier Membership Credential

The test client will request a DSP catalog from the provider using the DSP/DCP protocol combination and provide
a `Membership Credential` without a `holderIdentifier` entry.

#### Assertion

The assertion will verify that an `HTTP 4XX` error is returned.

### 2. Dismantler Policy

The provider connector will be primed with a dataset, that requires the fulfillment of the `Dismanter` policy and
presentation of a C-X Dismantler Credential:

```
{
  "constraint": {
    "leftOperand": "Dismantler.activityType",
    "operator": "eq",
    "rightOperand": "vehicleDismantle"
  }
}
```

### 2. 1. Positive Dismantler Test

The test client will request a DSP catalog from the provider using the DSP/DCP protocol combination and provide a
valid [Dismantler Credential](https://github.com/eclipse-tractusx/tractusx-profiles/blob/main/cx/credentials/schema/credentials/dismantler.credential.schema.json).

#### Assertion

The assertion will verify that a valid `DCAT 3` catalog is returned containing an `Offer` for the dataset.

### 2. 2. Negative No Dismantler Credential

The test client will request a DSP catalog from the provider using the DSP/DCP protocol combination and not provide
a `Dismantler Credential`.

#### Assertion

The assertion will verify that an `HTTP 4XX` error is returned.

### 2. 3. Negative Expired Dismantler Credential

The test client will request a DSP catalog from the provider using the DSP/DCP protocol combination and provide an
expired `Dismantler Credential`.

#### Assertion

The assertion will verify that an `HTTP 4XX` error is returned.

### 2. 4. Negative Invalid DID Dismantler Credential

The test client will request a DSP catalog from the provider using the DSP/DCP protocol combination and provide
a `Dismantler Credential` with an invalid `DID` for the `id` entry.

#### Assertion

The assertion will verify that an `HTTP 4XX` error is returned.

### 2. 5. Negative Invalid Holder Identifier Dismantler Credential

The test client will request a DSP catalog from the provider using the DSP/DCP protocol combination and provide
a `Dismantler Credential` without a `holderIdentifier` entry.

#### Assertion

The assertion will verify that an `HTTP 4XX` error is returned.

### 3. Framework Policies

The provider connector will be primed with datasets that require the fulfillment of each `Framework` policy subtype and
the presentation of a corresponding C-X Framework Credential:

```
{
  "constraint": {
    "leftOperand": "FrameworkAgreement",
    "operator": "eq",
    "rightOperand": "[subtype]:[version]"
  }
}
```

### 3. 1. Positive Framework Test

For each framework agreement subtype, the test client will request a DSP catalog from the provider using the DSP/DCP
protocol combination and provide a valid Framework `Credential Credential` subtype.

#### Assertion

The assertion will verify that a valid `DCAT 3` catalog is returned containing an `Offer` for the dataset corresponding
to each framework agreement subtype.

### 3. 2. Negative No Dismantler Credential

The test client will request a DSP catalog from the provider using the DSP/DCP protocol combination and not provide
a `Framework Credential`.

#### Assertion

The assertion will verify that an `HTTP 4XX` error is returned.

### 3. 3. Negative Expired Dismantler Credential

The test client will request a DSP catalog from the provider using the DSP/DCP protocol combination and provide an
expired `Framework Credential`.

#### Assertion

The assertion will verify that an `HTTP 4XX` error is returned.

### 3. 4. Negative Invalid DID Dismantler Credential

The test client will request a DSP catalog from the provider using the DSP/DCP protocol combination and provide
a `Framework Credential` with an invalid `DID` for the `id` entry.

#### Assertion

The assertion will verify that an `HTTP 4XX` error is returned.

### 3. 5. Negative Invalid Holder Identifier Dismantler Credential

The test client will request a DSP catalog from the provider using the DSP/DCP protocol combination and provide
a `Framework Credential` without a `holderIdentifier` entry.

#### Assertion

The assertion will verify that an `HTTP 4XX` error is returned.


