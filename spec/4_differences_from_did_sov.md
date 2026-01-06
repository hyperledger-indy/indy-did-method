## Differences between `did:sov`, `did:indy` and `did:indy:besu`

Early instances of Indy Networks used the `did:sov` DID Method. The following summarizes the differences between that `did:sov` and `did:indy`.

- A `did:indy` DID includes a namespace component that enables resolving a DID to a specific instance of an Indy network (e.g. Sovrin, IDUnion, etc.).
- Identifiers for Indy ledger objects other than [[ref: NYM]]s are adjusted to contain a namespace component.
- `did:indy` DID validation is determined by original NYM transaction `version`, as described in the [DID Creation](#nym-transaction-version) section. These restrictions MUST be enforced by the ledger.
- `did:indy:besu` DID validation placed in [[def: Indy Besu VDR]].
- For original indy the specification includes rules for transforming a [[ref: NYM]] into a DIDDoc that meets the DID Core Specification.
    - An optional [[ref: NYM]] data item allows entities to extend the DIDDoc returned from a [[ref: NYM]] in arbitrary ways.
    - The controller can decide whether the DIDDoc will be JSON or JSON-LD.
    - Before writing a [[ref: NYM]] to the ledger, the [[ref: NYM]] content is verified to ensure that transforming the [[ref: NYM]] to a DIDDoc produces a valid JSON and may include DIDDoc validity checking.
    - The transformation of a read [[ref: NYM]] to a DIDDoc is left to the client of an Indy ledger.
- For `did:indy:besu` transforming of ledger data into a DIDDoc is not needed. Objects are stored already in the [Decentralized Identifiers (DIDs) Core specification](https://https://www.w3.org/TR/did-core/) compatible format.
- A convention for storing Indy network instance config ("genesis") files in a Hyperledger Indy project GitHub repository ("indy-did-networks") is introduced.

### Compatibility `did:indy:besu` with other identifiers

The idea is using of a basic mapping between other DIDs identifiers and Ethereum accounts instead of introducing a new DID method.

* There is a mapping structure `legacyIdentifier => ethereumAccount` for storing `did:indy` and `did:sov` DID identifiers to the corresponding account address
    * Note, that user must pass signature over identifier to prove ownership
* On migration, DID owners willing to preserve resolving of legacy formatted DIDs and id's must add mapping between legacy identifier and Ethereum account defining
    * DID identifier itself
    * Associated public key
    * Ed25519 signature owner identifier proving ownership
* After migration, clients in order to resolve legacy identifier for DID document or a specific entity:
  * firstly should get a new identifier of the required Document DID or another entity, using DID or resource mapping
  * next resolve with the new identifier

### Migration from `did:indy` to `did:indy:besu`

At some point company managing (Issuer,Holder,Verifier) decide to migrate from [Indy Node](https://github.com/hyperledger-indy/indy-node) to [Indy Besu Ledger](https://github.com/hyperledger-indy/indy-besu).

In order to do that, their Issuer's applications need to publish their data to Indy Besu Ledger.
Issuer need to run migration tool manually (on the machine containing Indy Wallet storing Credential Definitions) which migrate data.

* Issuer:
  * All issuer applications need to run migration tool manually (on the machine containing Indy Wallet with Keys and Credential Definitions) in order to move data to Indy Besu Ledger properly. The migration process consist of multiple steps which will be described later.
  * After the data migration, issuer services should issue new credentials using Indy Besu ledger.
* Holder:
  * Holder applications can keep stored credentials as is. There is no need to run migration for credentials which already stored in the wallet.
  * Holder applications should start using Indy Besu ledger to resolve DID Documents, Schemas, Credential Definitions or other on-ledger entities once Issuer completed migration.
* Verifier:
  * Verifier applications should start using Indy Besu ledger to resolve DID Documents, Schemas, Credential Definitions or other on-ledger entities once Issuer completed migration.
  * Verifier applications should keep using old styled restriction in order to request credentials which were received before the migration.

1. Wallet and Client setup 
   1. All applications need to integrate Indy Besu vdr library
2. DID ownership moving to Indy Besu Ledger:
   1. Issuer create Ed25519 key (with seed) in the Indy Besu wallet
   2. Issuer create a new Secp256k1 keypair in Indy Besu wallet
   3. Issuer publish Secp256k1 key to Indy ledger using ATTRIB transaction: `{ "besu": { "key": secp256k1_key } }`
     * Now Indy Besu Secp256k1 key is associated with the Issuer DID which is published on the Indy Ledger.
     * ATTRIB transaction is signed with Ed25519 key. No signature request for `secp256k1_key`.
3. Issuer builds DID Document which will include:
   * DID - fully qualified form should be used: `did:besu:<network>:<did_value>` of DID which was published as NYM transaction to Indy Ledger
   * Two Verification Methods must be included:
     * `Ed25519VerificationKey2018` key published as NYM transaction to Indy Ledger
       * Key must be represented in multibase as base58 form was deprecated
     * `EcdsaSecp256k1VerificationKey2019` key published as ATTRIB transaction to Indy Ledger
       * Key must be represented in multibase
       * This key will be used in future to sign transactions sending to Indy Besu ledger
         * Transaction signature proves ownership of the key
         * Indy Besu account will be derived from the public key part
   * Two corresponding authentication methods must be included.
   * Service including endpoint which was published as ATTRIB transaction to Indy Ledger
4. Issuer publishes DID Document to Indy Besu ledger:
    ```
     let did_doc = build_did_doc(&issuer.did, &issuer.edkey, &issuer.secpkey, &issuer.service);
     let receipt = DidRegistry::create_did(&client, &did_document).await
    ```
   * Transaction is signed using Secp256k1 key `EcdsaSecp256k1VerificationKey2019`.
     * This key is also included into Did Document associated with DID.
     * Transaction level signature validated by the ledger that proves key ownership.
   * `Ed25519VerificationKey2018` - Indy Besu ledger will not require signature for proving ownership this key.
     * key just stored as part of DID Document and is not validated
     * potentially, we can add verification through the passing an additional signature
     ```
     { 
         context: "https://www.w3.org/ns/did/v1", 
         id: "did:indy:besu:sovrin:staging:0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266", 
         controller: "did:indy:besu:sovrin:staging:0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266", 
         verificationMethod: [
             { 
                 id: "did:indy:besu:sovrin:staging:0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266#KEY-1", 
                 type: Ed25519VerificationKey2018, 
                 controller: "did:indy:besu:sovrin:staging:0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266",
                 publicKeyMultibase: "zH3C2AVvLMv6gmMNam3uVAjZpfkcJCwDwnZn6z3wXmqPV"
             }, 
             {
                 id: "did:indy:besu:sovrin:staging:0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266#KEY-2", 
                 type: EcdsaSecp256k1VerificationKey2019, 
                 controller: "did:indy:besu:sovrin:staging:0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266", 
                 publicKeyMultibase: "zNaqS2qSLZTJcuKLvFAoBSeRFXeivDfyoUqvSs8DQ4ajydz4KbUvT6vdJyz8i9gJEqGjFkCN27niZhoAbQLgk3imn
             }
         ], 
         authentication: [
             "did:indy:besu:sovrin:staging:0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266#KEY-1", 
             "did:indy:besu:sovrin:staging:0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266#KEY-2"
         ], 
         assertionMethod: [], 
         capabilityInvocation: [], 
         capabilityDelegation: [], 
         keyAgreement: [], 
         service: [
             { 
                 id: "#inline-1", 
                 type: "DIDCommService", 
                 serviceEndpoint: "127.0.0.1:5555" 
             }
        ]
     }
     ```
5. Issuer builds and publishes all connected entities as Schemas and Credential Definitions on Indy Besu ledger



