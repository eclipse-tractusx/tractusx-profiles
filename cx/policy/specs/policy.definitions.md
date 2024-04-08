# 1. Introduction

The document provides a policy specification for Catena-X verifiable credentials (VC).

### 1.1. Decoupling Policy Definitions from VC Schemas

The key architectural principle underlying this specification is that policy definitions must be decoupled from their
corresponding VC schema. Namely, the specific constraints and shape of the VC schema must not be reflected in the
policy definition. This allows VC schemas to be altered without impacting policy definitions.

### 1.2. Policy Constraints

Usage requirements involving VCs are expressed as **_ODRL Policy Constraints_**.

### 1.2.1. Left Operand Constraint Schema

Catena-X will use the left and right operands of a _constraint_ to associate a specific VC or other requirement. The
left operand value will correspond to the form:

`[requirement type].[discriminator]

The `requirement type` may be mapped to a VC or other requirement such as acceptance of terms and conditions.
The `discriminator` segment is optional.

### 1.2.2. The Right Operand

The right operand consists of the following segments separated by the `:` delimiter:

`[subtype | active]:[version]`

The `subtype` segment is optional and is used to specify a refinement of the `requirement type`. If no
specific `subtype` value is specified, `active` is used.

The version segment can optionally be used to specify a requirement version, for example, mandating a specific version
can be expressed as:

`active:2.0.0`

The version should follow [semantic versioning](https://semver.org/) If the version is omitted, the latest version will
be used.

### 1.3 A Note on Json-Ld Prefixes

Json-Ld prefixes are not show in the examples since the Json-Ld context includes term aliases that make prefixes
optional for known constraint types. For example, assuming the `cx` prefix, `Membership` and `cx-policy:Membership` are equal
and will be processed in the same way.

# 2. Membership Constraint

The `Membership` constraint requires a participant to possess a VC attesting to C-X membership. It is expressed as
follows:

```json
{
  "constraint": {
    "leftOperand": "Membership",
    "operator": "eq",
    "rightOperand": "active"
  }
}
```

The `Membership` constraint may only be used as an ODRL `Permission`.

Valid `rightOperand` values are: `active`.

# 3. Dismantler Constraint

The `Dismantler` constraint requires a participant to possess a VC attesting to its status as a verified dismantler. The
constraint has two subtypes

## 3.1. Dismantler

The `Dismantler` root constraint requires a participant to be a certified dismantler. It is expressed as follows:

```json
{
  "constraint": {
    "leftOperand": "Dismantler",
    "operator": "eq",
    "rightOperand": "active"
  }
}
```

The `Dismantler` constraint and its subtypes may only be used as an ODRL `Permission`.

## 3.2. Dismantler.activityType

***Not supported in 3.2**

The `Dismantler.activityType` subtype requires the dismantler to be certified for a given activity. It is expressed as
follows:

```json
{
  "constraint": {
    "leftOperand": "Dismantler.activityType",
    "operator": "eq",
    "rightOperand": "vehicleDismantle"
  }
}
```

## 3.3. Dismantler.allowedBrands

***Not supported in 3.2**

The `Dismantler.allowedBrands` subtype requires the dismantler to be certified for a set of brands. It is expressed
using an `in` operator as follows:

```json
{
  "constraint": {
    "leftOperand": "Dismantler.allowedBrands",
    "operator": "in",
    "rightOperand": [
      "Brand A",
      "Brand B"
    ]
  }
}
```

The `rightOperand` is an array of type `string` containing one or more elements. The elements are evaluated using a
logical OR.

# 4. Framework Agreement Constraints

Framework agreement constraints adhere to the following syntax:

```json
{
  "constraint": {
    "leftOperand": "FrameworkAgreement",
    "operator": "eq",
    "rightOperand": "[subtype]:[version]"
  }
}
```

The `subtype` segment specifies the framework case agreement. Note `active` is not a valid subtype. The version postfix
is optional.

Framework agreement constraints may only be used as an ODRL `Permission`.

## 4.1. PCF

The PCF framework agreement is expressed as:

```json
{
  "constraint": {
    "leftOperand": "FrameworkAgreement",
    "operator": "eq",
    "rightOperand": "pcf:[version]"
  }
}
```

## 4.2. Sustainability

The Sustainability framework agreement is expressed as:

```json
{
  "constraint": {
    "leftOperand": "FrameworkAgreement",
    "operator": "eq",
    "rightOperand": "sustainability:[version]"
  }
}
```

## 4.3. Quality

The Quality framework agreement is expressed as:

```json
{
  "constraint": {
    "leftOperand": "FrameworkAgreement",
    "operator": "eq",
    "rightOperand": "quality:[version]"
  }
}
```

## 4.4. Resiliency

The Resiliency framework agreement is expressed as:

```json
{
  "constraint": {
    "leftOperand": "FrameworkAgreement",
    "operator": "eq",
    "rightOperand": "resiliency:[version]"
  }
}
```

## 4.5. Traceability

The Traceability framework agreement is expressed as:

```json
{
  "constraint": {
    "leftOperand": "FrameworkAgreement",
    "operator": "eq",
    "rightOperand": "traceability:[version]"
  }
}
```

## 4.6. Behavioral Twin

The Behavior framework agreement is expressed as:

```json
{
  "constraint": {
    "leftOperand": "FrameworkAgreement",
    "operator": "eq",
    "rightOperand": "behavioraltwin:[version]"
  }
}
```

## 4.7. BPN

> ISSUE: The Membership VC appears to contain the same information. Is the BPN VC needed?

## 4.8. ContractReference

The `Membership` constraint is used to reference applicable terms and conditions or other applicable rules. It is
expressed as follows:

```json
{
  "constraint": {
    "leftOperand": "ContractReference",
    "operator": "eq",
    "rightOperand": "[string]:[version]"
  }
}
```

The `ContractReference` constraint may only be used as an ODRL `Permission`.

Valid `rightOperand` values must contain a `string` segment. This segment may be any valid set of characters, including
a `URL`, contract number, or other relevant data.

## 4.9. UsagePurpose

The `UsagePurpose` constraint is used to denote a governing purpose. It is expressed as follows:

```json
{
  "constraint": {
    "leftOperand": "UsagePurpose",
    "operator": "eq",
    "rightOperand": "[string]:[version]"
  }
}
```

The `UsagePurpose` constraint may only be used as an ODRL `Permission`.

Valid `rightOperand` values must contain a `string` segment. This segment may be any valid set of characters.

# 5. Policy Example

The following is an `Offer` that requires the data user to be a certified dismantler for `Brand A` vehicles and to have
an active signed traceability agreement:

```json
 {
  "@context": [
    "https://www.w3.org/ns/odrl.jsonld",
    "https://w3id.org/tractusx/policy/v1.0.0"
  ],
  "@type": "Offer",
  "@id": "a343fcbf-99fc-4ce8-8e9b-148c97605aab",
  "permission": [
    {
      "action": "use",
      "constraint": {
        "and": [
          {
            "leftOperand": "Dismantler.allowedBrands",
            "operator": "in",
            "rightOperand": [
              "Brand A"
            ]
          },
          {
            "leftOperand": "FrameworkAgreement",
            "operator": "eq",
            "rightOperand": "traceability"
          }
        ]
      }
    }
  ]
}
```
