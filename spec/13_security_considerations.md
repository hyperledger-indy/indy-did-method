## Security Considerations

> To Do: Add a brief summary of Indy (public permissioned, consensus) and the assumptions on which the security considerations are documented -- notably that the DID Controller protect their private key(s).

Secure communication to the Indy Ledger uses CurveZMQ to mitigate attacks like eavesdropping, replay, message insertion, deletion, modification, impersonation, and man-in-the-middle. Any vulnerabilities in that protocol will apply to Indy.

The following sections decribe how the `did:indy` DID Method adheres to the security considerations outlined in the [DID Core 1.0 specification](https://w3c.github.io/did-core).

- A DID method specifications MUST follow all guidelines and normative language provided in RFC3552: Writing Security Considerations Sections for the DID operations defined in the DID method specification.

::: todo add documentation to spec
Review https://datatracker.ietf.org/doc/html/rfc3552 and ensure documentation is written to spec.
:::

- The Security Considerations section MUST document the following forms of attack for the DID operations defined in the DID method specification: eavesdropping, replay, message insertion, deletion, modification, denial of service, storage or network amplification, and man-in-the-middle. Other known forms of attack SHOULD also be documented.
  
  - eavesdropping
    - Doesn't matter -- data written is public
  - replay
    - Nonce included -- prevents replay
  - message insertion
    - Not possible
  - deletion
    - Not possible
  - modification
    - Consensus prevents
  - denial of service
    - Separate interfaces to prevent loss of node-to-node communications
  - storage or network amplification
    - Definition: https://github.com/w3c/did-core/pull/730/files
  - man-in-the-middle
    - Authorization prevents

::: todo document mitigations for the forms of attack
Document mitigations for each the forms of attack; add others that might be relevant.
:::

- The Security Considerations section MUST discuss residual risks, such as the risks from compromise in a related protocol, incorrect implementation, or cipher after threat mitigation was deployed.

::: todo Identify residual attacks and mitigations
Identify residual attacks and mitigations
:::

- The Security Considerations section MUST provide integrity protection and update authentication for all operations required by Section § 8.2 Method Operations.

::: todo review write operations
Brief review of the write operations -- DID Controller must sign operation, as needed others may need to sign it. Risk of loss of control if private key for the DID Controller is lost.
:::

- If authentication is involved, particularly user-host authentication, the security characteristics of the authentication method MUST be clearly documented.
  - No user-host authentication is used. The appropriate signatures are needed on write transactions, where the DIDs and verkeys of the signatories must be one the ledger.

::: todo expand authentication
Expand
:::

- The Security Considerations section MUST discuss the policy mechanism by which DIDs are proven to be uniquely assigned.
  - DIDs are self-certifying, derived from the initial verkey of an ED25519 key pair, which makes them extremely likely to be unique. Any attempt to write the same DID would only work if the signature matched (e.g. if the seed to create the DID had been lost so the literal same DID was attempted to be written), which would result in no change to the ledger, and goes against assumption of the DID Controller not protecting their seed/private key.

::: todo expand security considerations
Expand
:::

- Method-specific endpoint authentication MUST be discussed. Where DID methods make use of DLTs with varying network topology, sometimes offered as light node or thin client implementations to reduce required computing resources, the security assumptions of the topology available to implementations of the DID method MUST be discussed.
  - No endpoint authentication is used.

::: todo expand method-specific endpoint authentication
Expand
:::

- If a protocol incorporates cryptographic protection mechanisms, the DID method specification MUST clearly indicate which portions of the data are protected and by what protections, and it SHOULD give an indication of the sorts of attacks to which the cryptographic protection is susceptible. Some examples are integrity only, confidentiality, and endpoint authentication.

::: todo expand data protection and sort of susceptible attacks
Expand
:::

- Data which is to be held secret (keying material, random seeds, and so on) SHOULD be clearly labeled.
  - No ledger data needs to be kept secret. The private keys associated with the public keys written to the ledger must be kept secret.
  - The seed used for private key generation

::: todo expand secret data labeling
Expand
:::

- DID method specifications SHOULD explain and specify the implementation of signatures on DID documents, if applicable.

::: todo document write authorization signature scheme
Document the signature scheme used to authorize writes to the ledger.
:::

- Where DID methods use peer-to-peer computing resources, such as with all known DLTs, the expected burdens of those resources SHOULD be discussed in relation to denial of service.

::: todo document expected resource burden related to DOS
Document this for Indy
:::

- DID methods that introduce new authentication service types, as described in § 5.4 Services, SHOULD consider the security requirements of the supported authentication protocol.

> To Do: Confirm not applicable.
