## Indy DID Method Identifiers

The did:indy Method DID identifier has four components that are concatenated to make a DID specification conformant identifier. The components are:

- **DID**: the hardcoded string `did:` to indicate the identifier is a DID.
- **DID Indy Method**: the hardcoded string `indy:` indicating that the identifier uses this DID Method specification. The hardcoded string `indy:besu:` indicates the use of [`indy-besu`](https://github.com/hyperledger-indy/indy-besu) implementation for Indy DID Method.
- **DID Indy Namespace**: a string that identifies the name of the primary Indy ledger, followed by a `:`. The namespace string may optionally have a secondary ledger name prefixed by a `:` following the primary name. If there is no secondary ledger element, the DID resides on the primary ledger, else it resides on the secondary ledger. By convention, the primary is a production ledger while the secondary ledgers are non-production ledgers (e.g. staging, test, development) associated with the primary ledger. Examples include, `sovrin`, `sovrin:staging` and `idunion`.
- **Namespace Identifier**: an identifier unique to the given DID Indy namespace. 
  - The identifier may be self-certifying, meaning that the identifier is derived from the initial verkey associated with the identifier. See the [DID Creation](#nym-transaction-version) section of this document for the derivation details.
  - The identifier of `did:indy:besu` is an Ethereum address similarly to `did:ethr` method, but there multiple significant differences between them:
    - API consist of more traditional `create`, `update`, `deactivate` methods
    - The associated `Did Document` is stored in the contract storage in complete form
    - In order to resolve Did Document you only need to call single method
    - DID must be registered by executing one of `create` contract methods
    - State proof can be obtained for resolved Did Record

The components are assembled as follows:

`did:indy:<namespace>:<namespace identifier>`

`did:indy:besu:<namespace>:<namespace identifier>`

Some examples of `did:indy` DID Method identifiers are:

* A DID written to the Sovrin MainNet ledger:
    * `did:indy:sovrin:7Tqg6BwSSWapxgUDm9KKgg`
* A DID written to the Sovrin StagingNet ledger:
    * `did:indy:sovrin:staging:6cgbu8ZPoWTnR5Rv5JcSMB`
* A DID on the IDUnion Test ledger:
    * `did:indy:idunion:test:2MZYuPv2Km7Q1eD4GCsSb6`
* A DID written to the Sovrin StagingNet ledger used Indy Besu implementation:
  * `did:indy:besu:sovrin:staging:0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266`
