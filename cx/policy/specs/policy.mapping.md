# Mapping Credentials

## 1. Definition of terms

- Credential type: a [W3C Verifiable Credential type](https://w3c.github.io/vc-data-model/#types), for
  example `DismantlerCredential`. For every credential type there must be a resolvable context, which contains further
  schema and type information. Credentials can have multiple types. This is comparable to a class definition in OOP
  languages. Also referred to as: CredentialType, type

- Credential: an instance of a credential type, i.e. one materialization of a particular schema. This is comparable to a
  class instance in OOP languages. Also referred to as: vc, VC, VerifiableCredential

- Framework credential: informal term for credential types that are used to assert adherence to a particular use case.
  Holders of a "framework credential" ara assumed to have signed the respective Framework Agreement Contract. Note that
  each framework credential still has a distinct credential type. Also referred to as: use case credential, use case

- Contract version: refers to the `credentialSubject.contractVersion` property and is expected to be
  a [SemVer string](https://semver.org/). The `contractVersion` discriminates which framework agreement use case version
  the credential attests to. Also referred to as a `framework agreement version`.

- Credential version: a credential version does not exist as a first-class concept.

- EBNF: refers to the [Extended Backus Naur Form](https://www.cl.cam.ac.uk/~mgk25/iso-14977.pdf) used to describe
  allowed sequences of terms

## 2. Referencing credentials in policies

Please check the syntax [here](./policy.definitions.md)

## 3. Mapping Framework credentials

Automatic mapping policy constraint expressions to credential type is possible assuming all framework credentials follow
the same schema. Assuming that is the case, the evaluation of the policy (= the evaluation function) can be generic.

In an example a policy constraint requiring the "Pcf" credential in version 0.4.2, would look like this:

```json
{
  "constraint": {
    "leftOperand": "FrameworkAgreement",
    "operator": "eq",
    "rightOperand": "Pcf:0.4.2"
  }
}
```

If a specific contract version is referenced in the policy constraint, like in the example above, it follows that the
policy **must be updated**, if version `0.4.3` of the `Pcf` credential is released, and the policy should reference it.

If no contract version is specified in the policy constraint, then the policy **need not be updated**.

### 3.1 Mapping Policy Constraint -> Credential Type

Use case credentials are identified by their left operand, which is always `FrameworkAgreement`, and their right operand 
which contains the use case identifier and optionally a version.
Formally, a policy expression, that requires a use case credential, is defined by the following EBNF:

```ebnf
POLICYLEFTOPERAND ::= FRAMEWORKLITERAL ;
POLICYOPERATOR    ::= "eq"
FRAMEWORKLITERAL  ::= "FrameworkAgreement" ;
POLICYRIGHTOPERAND::= USECASEIDENT [COLON SEMVERSTRING]
USECASEIDENT      ::= #'[a-z][a-zA-Z0-9]+' ;
COLON             ::= ":" ;
SEMVERSTRING      ::= <any_semantic_version_string> ;
```


In order to reconstruct the actual credential type, which in the example would be `PcfCredential`, we perform relatively
simple String manipulation:

- remove `FrameworkAgreement`. Hint: use it to trigger the transformation.
- capitalize the right-operand if necessary (i.e. `pcf --> Pcf`)
- append the `Credential` literal

Formally, that transformation must adhere to the following EBNF:

```ebnf
USECASETYPE         ::= USECASEIDENT CREDENTIALLITERAL ;
USECASEIDENT        ::= #'[A-Z][a-z0-9]+' ;
CREDENTIALLITERAL   ::= 'Credential' ;
```

Again, this would result in the term `PcfCredential`. In addition, parsing the `rightOperand` would yield the (optional)
version identifier. Please note that the version identifier `0.4.2` is **not** processed by the Resolution API and will
be filtered on the client side (=EDC).

### 3.2 Mapping Credential Type -> Scope String

To obtain credentials from the CredentialService a scope string is required identifying the _Credential type_, for
example when requesting the `PcfCredential` from the CredentialService, one would use a scope string like
`org.eclipse.tractusx.vc.type:PcfCredential:read`.

The conversion from credential type to scope string can be performed without any static mapping, but again by
relatively simple string manipulation:

- start with the (dataspace-specific) scope prefix (`org.eclipse.tractusx.vc.type`). This is a configuration value.
- append a `:`
- append the credential type
- append a `:`
- append the operator (`read`, `write`, `*`)

resulting in `org.eclipse.tractusx.vc.type:PcfCredential:read`.

The scope prefix is configurable per dataspace, but in Tractus-X it *must* be `org.eclipse.tractusx.vc.type`.

Formally, a scope string must adhere to the following EBNF:

```ebnf
SCOPESTRING         ::= PREFIX COLON CREDENTIALTYPE COLON OPERATOR ;
DOT                 ::= ".";
PREFIX              ::= "org.eclipse.tractusx.vc.type" ;
DIGIT               ::= "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" ;
OPERATOR            ::= "write" | "read" | "*" ;
COLON               ::= ":" ;
CREDENTIALTYPE      ::= TYPEPREFIX CREDENTIALLITERAL ;
TYPEPREFIX          ::= #'[A-Z][a-z0-9]+' ;
CREDENTIALLITERAL   ::= "'"Credential"'" ;
```

### 3.3 Mapping Scope String -> database query

Performing the inverse mapping of the scope string would once again produce the credential type `PcfCredential` which
could then be used to construct a database query in the CredentialService similar to

```sql
SELECT *
FROM credentials
WHERE credentials.type = 'PcfCredential';
```

*NB: this is just an example*.

## 4. Mapping other credentials

This refers to credentials which are **not** use case credentials, like the `DismantlerCredential`.

### 4.1 Mapping Policy Constraint -> Credential Type

For all other credentials the policy expression is similar to use case credentials. For example

```json
{
  "constraint": {
    "leftOperand": "Dismantler",
    "operator": "eq",
    "rightOperand": "active"
  }
}
```

would be interpreted as "Must have an active DismantlerCredential". To construct the credential type one simply
appends `Credential` to the leftOperand extracted from the policy constraint.

### 4.2 Mapping Credential Type -> Scope String

[see previous section](#32-mapping-credential-type---scope-string)

### 4.2 Mapping Scope String -> database query

[see previous section](#33-mapping-scope-string---database-query)

### 4.3 Using credential-specific policy constraints

Credential types that are not a use case credential may have specific credential subjects, which can allow specific
policy constraint expressions. These are defined in [this document](./policy.definitions.md). For example,
the [DismantlerCredential](../../credentials/samples/dismantler.credential.json) has in its credential subject
information about the vehicle brands that can be worked on.

This has no influence on either the Scope String or the Resolution API, because those types of constraints would be
evaluated in the client side (=EDC) and would thus require **custom logic**.
