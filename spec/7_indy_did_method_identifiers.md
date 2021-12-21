## Indy DID Method Identifiers

The did:indy Method DID identifier has four components that are concatenated to make a DID specification conformant identifier. The components are:

- **DID**: the hardcoded string `did:` to indicate the identifier is a DID
- **DID Indy Method**: the hardcoded string `indy:` indicating that the identifier uses this DID Method specification.
- **DID Indy Namespace**: a string that identifies the name of the primary Indy ledger, followed by a `:`. The namespace string may optionally have a secondary ledger name prefixed by a `:` following the primary name. If there is no secondary ledger element, the DID resides on the primary ledger, else it resides on the secondary ledger. By convention, the primary is a production ledger while the secondary ledgers are non-production ledgers (e.g. staging, test, development) associated with the primary ledger. Examples include, `sovrin`, `sovrin:staging` and `idunion`.
- **Namespace Identifier**: a self-certifying identifier unique to the given DID Indy namespace. To be 
  self-certifying the identifier must be derived from the initial verkey associated with the identifier. See the 
  [DID Creation](#creation) section of this document for the derivation details.

The components are assembled as follows:

`did:indy:<namespace>:<namespace identifier>`

Some examples of `did:indy` DID Method identifiers are:

* A DID written to the Sovrin MainNet ledger:
    * `did:indy:sovrin:7Tqg6BwSSWapxgUDm9KKgg`
* A DID written to the Sovrin StagingNet ledger:
    * `did:indy:sovrin:staging:6cgbu8ZPoWTnR5Rv5JcSMB`
* A DID on the IDUnion Test ledger:
    * `did:indy:idunion:test:2MZYuPv2Km7Q1eD4GCsSb6`
