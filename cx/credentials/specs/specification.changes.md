# Spec Changes

1. Remove `startTime`, `memberOf`, and `status` from the `Membership` credential 
2. Remove `credentialSubject` and `type` attributes from all credentials
3. Create separate credential context using the URI: https://w3id.org/catenax/credentials/v1.0.0 
4. Create separate policy context using the URI: https://w3id.org/tractusx/policy/v1.0.0
5. Create JSON Schema definitions for all credentials using the URI: https://w3id.org/catenax/schemas/v1.0.0/
6. Adopt JWT-based VCs using JWS algorithm `ES256K` and JWK curve `secp256k1`
7. Separate namespaces for `tractusx` and `catenax`
