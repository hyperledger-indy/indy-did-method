## Other Indy Ledger Object Identifiers

Indy ledgers may hold object types other than DIDs, and each of the other object types must also be resolvable to a specific Indy network instance. The identifiers for these objects are used in data structures that are exchanged by Indy clients (e.g. Aries Agents)--verifiable credentials, presentation requests, presentations and so on. Transitioning to the `did:indy` DID Method requires transitioning Indy clients/resolvers to use the identifiers defined in this section.

### DID URLs for Indy Object Identifiers

The structure of identifiers for all non-DID Indy ledger objects is the following DID URL structure, based on the DID of the object's DID controller:

- `<did>/<object-type>/<object-type-identifier>`

The components of the DID URL are:

- `<did>` the `did:indy` DID of the object-owning controller
- `<object-type>` one of [[ref: SCHEMA]], [[ref: CLAIM_DEF]], [[ref: REV_REG_DEF]], [[ref: REV_REG_ENTRY]], [[ref: ATTRIB]]
- `<object-type-identifier>` an object type unique identifier defined by Indy by object type.

The data returned from resolving such DID URLs is the ledger object and relevant state proof; the same data returned from the Indy Node read object transactions, such as the [GET_SCHEMA](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-schema) transaction, and dependent on the type of the object.

The following sections cover each ledger object type, providing:

- an example DID URL identifier,
- a link to an example object residing on the Sovrin MainNet Indy ledger (courtesy of [indyscan.io](https://indyscan.io)),
- the format of the response when resolving the DID URL,
- the pre-`did:indy` identifier for each object, and
- notes about the elements of the pre-`did:indy` identifier.

#### Schema

DID URL: [`did:indy:sovrin:F72i3Y3Q4i466efjYJYCHM/SCHEMA/npdb/4.3.4`](https://indyscan.io/tx/SOVRIN_MAINNET/domain/56495)

Response: Same as the Indy Node [GET_SCHEMA Txn](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-schema)

Existing identifier: `F72i3Y3Q4i466efjYJYCHM:2:npdb:4.3.4`

- `2` is the enumerated object type
- `npdb` is the client-defined schema name
- `4.3.4` is the client-defined version of the [[ref: SCHEMA]]

#### Claim Def

DID URL: [`did:indy:sovrin:5nDyJVP1NrcPAttP3xwMB9/CLAIM_DEF/npdb`](https://indyscan.io/tx/SOVRIN_MAINNET/domain/56496)

Response: Same as the Indy Node [GET_CLAIM_DEF Txn](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-claim-def)

Existing identifier: `did:indy:sovrin:5nDyJVP1NrcPAttP3xwMB9:3:CL:56495:npdb`

- `3` is the enumerated object type
- `CL` is the signature type for the claim def, which is `CL` for all claim defs on all existing Indy ledgers
- `56495` is the sequence number for the Schema object used by this Claim Def
- `npdb` is the client-defined claim def name

Note that the DID URL format adds a constraint on the client-defined claim def name from the `did:sov` DID Method. Specifically, the same named claim def can no longer be associated with different [[ref: SCHEMA]] objects.

#### Revocation Registry Definition

DID URL: [`did:indy:sovrin:5nDyJVP1NrcPAttP3xwMB9/REV_REG_DEF/npdb/TAG1`](https://indyscan.io/tx/SOVRIN_MAINNET/domain/56497)

Response: Same as the Indy Node [GET_REVOC_REG_DEF Txn](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-revoc-reg-def)

Existing Identifier: `5nDyJVP1NrcPAttP3xwMB9:4:5nDyJVP1NrcPAttP3xwMB9:3:CL:56495:npdb:CL_ACCUM:TAG1`

- `4` is the enumerated object type
- `5nDyJVP1NrcPAttP3xwMB9:3:CL:56495:npdb` is the identifier of the associated Claim Def
-  `TAG1` is the client-defined rev reg name

#### Revocation Registry Entry

DID URL: [did:indy:sovrin:5nDyJVP1NrcPAttP3xwMB9/REV_REG_ENTRY/npdb/TAG1](https://indyscan.io/tx/SOVRIN_MAINNET/domain/58567)

The DID URL resolution response depends on the query parameters used, as follows:

- `?versionId=<timestamp>`
    - Response is the same as the Indy Node [GET_REVOC_REG Txn](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-revoc-reg)
    - If the parameter is left off the current time is used for the timestamp.
- `?from=<timestamp>&to=<timestamp>`
    - Response is the same as the Indy Node [GET_REVOC_REG_DELTA Txn](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-revoc-reg-delta)
    - As noted in the transaction documentation, the from parameter is optional.
    - If the `to` parameter is left off the current time is used for the `to` timestamp.

Existing Identifier: `5:5nDyJVP1NrcPAttP3xwMB9:4:5nDyJVP1NrcPAttP3xwMB9:3:CL:56495:npdb:CL_ACCUM:TAG1`
- `5` is the enumerated object type
- The remainder of the identifier is the identifier for the applicable Revocation Registry

#### ATTRIB

No DID URL representation is defined for the Hyperledger Indy ATTRIB object, as
the use of the ATTRIB object is **deprecated** with the introduction of the
`did:indy` DID Method. Where an ATTRIB might have been used in the past, an Indy
client updated for `did:indy` should put the required data directly into the
`diddocContent` item in a DID (NYM) update transaction.
