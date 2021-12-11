## Motivation and Assumptions

### Assumption: Number of Indy Instances

We are anticipating that there will be at most on the order of "low hundreds" of Indy network instances, with the potential for several (usually 3) sub-namespaces per network instance for production, test (staging), and development deployments. This assumption is based on the likelihood of their being some (likely small) number of global Indy instances, and some number of national Indy instances. Assuming at most one per country, we would have around 200-300 total, leading to the anticipated maximum of "low hundreds" of Indy network instances.

### Including a Network-specific Identifier

Including a network-specific identifier within an Indy DID identifier enables a "network of networks" capability where an Indy DID can be uniquely resolved to a specific DIDDoc stored on a specific Indy network using a common resolver software component. Given a `did:indy` DID, the network-specific identifier embedded in the DID can be extracted to determine where to send the request to resolve the DID Doc. This enables several useful properties:

- Decentralization: Additional instances of networks can be deployed and easily used.
- Scalability: Additional DID ledgers can be deployed and DIDs on those ledgers easily referenced.
- Fit for purpose: A DID ledger can be deployed and easily used for specific purposes, such as part of a nation's critical infrastructure.
- Governance: A DID ledger can be deployed under a specific governance structure appropriate to those writing to that ledger.

### Aligning Indy with the DID Core Specification

The DID Indy method specification formalizes the transformation of an Indy ledger object (a [[ref: NYM]]) into a DIDDoc as defined in the DID Core specification from W3C, ensuring that identifiers written to and read from Indy ledgers are W3C standard DIDs.

### Resolving a DIDDoc in a single transaction

Previous approaches to resolving Indy ledger objects into DIDDocs required the client read one or more ledger objects (notably, [[ref: NYM]]s and [[ref: ATTRIB]]s) before assembly. This is at best "challenging" for the client, and at worst, extremely slow without specialized ledger support, particularly when a non-current version of the DID is being resolved. A preferred approach is to enable the resolution of a DID via a single read transaction that returns a single object off the ledger, including a state proof for that object.

### Cross-Ledger Object References

The [[ref: NYM]] controller of all objects on an Indy ledger MUST reside on the same Indy ledger as the object. Thus, the DID (e.g. the Indy [[ref: NYM]]) of the Issuer of a verifiable credential type must reside on the same ledger as the [[ref: CLAIM_DEF]], [[ref: REV_REG_DEF]] and [[ref: REV_REG_ENTRY]] objects for that type of verifiable credential.

Note that the constraint above does not apply to a [[ref: SCHEMA]] referenced by a [[ref: CLAIM_DEF]], since a [[ref: CLAIM_DEF]] may use a [[ref: SCHEMA]] written by another [[ref: NYM]]. As such, a [[ref: CLAIM_DEF]] may reference a [[ref: SCHEMA]] on a different Indy ledger.
