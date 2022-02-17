## Other Indy Ledger Object Identifiers

Indy ledgers may hold object types other than DIDs, and each of the other object types must also be resolvable to a specific Indy network instance. The identifiers for these objects are used in data structures that are exchanged by Indy clients (e.g. Aries Agents)--verifiable credentials, presentation requests, presentations and so on. Transitioning to the `did:indy` DID Method requires transitioning Indy clients/resolvers to use the identifiers defined in this section.

### DID URLs for Indy Object Identifiers

The structure of identifiers for all non-DID Indy ledger objects is the following DID URL structure, based on the DID of the object's DID controller:

- `<did>/<object-family>/<object-family-version>/<object-type>/<object-type-identifier>`

The components of the DID URL are:

- `<did>` the `did:indy` DID of the object-owning controller
- `<object-family>` family of the object
- `<object-family-version>` version of the object family
- `<object-type>` one of [[ref: SCHEMA]], [[ref: CLAIM_DEF]], [[ref: REV_REG_DEF]], [[ref: REV_REG_ENTRY]], [[ref: ATTRIB]]
- `<object-type-identifier>` an object type unique identifier defined by Indy by object type.

The data returned from resolving such DID URLs is the ledger object and relevant state proof; the same data returned from the Indy Node read object transactions, such as the [GET_SCHEMA](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-schema) transaction, and dependent on the type of the object.

The following sections cover each ledger object type, providing:

- an example DID URL identifier,
- a link to an example object residing on the Sovrin MainNet Indy ledger (courtesy of [indyscan.io](https://indyscan.io)),
- the appropriate object family and version,
- the format of the response when resolving the DID URL,
- the pre-`did:indy` identifier for each object, and
- notes about the elements of the pre-`did:indy` identifier.

This first version of the `did:indy` DID Method will use an `<object-family>` value of `anoncreds` and an `<object-family-version>` of `v0` to match the
pre-specification, open source version of anoncreds as implemented in the [indy-sdk](https://github.com/hyperledger/indy-sdk/tree/master/docs/design/002-anoncreds).
Later versions of the `did:indy` specification will use a higher `<object-family-version>` as the AnonCreds standardization work proceeds
and the required dependency on Hyperledger Indy is removed. In this initial version, the DID URLs are closely aligned with the existing object identifiers.

#### Schema

DID URL: [`did:indy:sovrin:F72i3Y3Q4i466efjYJYCHM/anoncreds/v0/SCHEMA/npdb/4.3.4`](https://indyscan.io/tx/SOVRIN_MAINNET/domain/56495)

- Object Family: `anoncreds`
- Family Version: `v0`
- Name, example `npdb`: The client-defined schema name
- Schema Version, example `4.3.4`: The client-defined version of the [[ref: SCHEMA]]

Response: Same as the Indy Node [GET_SCHEMA Txn](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-schema)

Existing identifier: `F72i3Y3Q4i466efjYJYCHM:2:npdb:4.3.4`

- `2` is the enumerated object type
- Name and Schema Version elements defined above.

#### Claim Def

DID URL: [`did:indy:sovrin:5nDyJVP1NrcPAttP3xwMB9/anoncreds/v0/CLAIM_DEF/56495/npdb`](https://indyscan.io/tx/SOVRIN_MAINNET/domain/56496)

- Object Family: `anoncreds`
- Family Version: `v0`
- Schema ID, example `56495`: A unique identifier for the schema upon which the ClaimDef is defined. In v0, the value is also the Hyperledger Indy instance sequence number for the Schema object used by this Claim Def.
  In later versions, we expect that the schema identifier will either be removed from the ClaimDef DID URL, or take a different form.
- Name, example `npdb`: The client-defined claim def name.

Response: Same as the Indy Node [GET_CLAIM_DEF Txn](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-claim-def)

Existing identifier: `5nDyJVP1NrcPAttP3xwMB9:3:CL:56495:npdb`

- `3` is the enumerated object type
- `CL` is the signature type for the claim def, which is `CL` for all claim defs on all existing Indy ledgers
- Schema ID and Name elements defined above.

We recommend that AnonCred credential issuers use a unique Name item per Claim Def, and not rely on the embedded Schema ID
remaining in the DID URL for a Claim Def in future versions of the `did:indy` method.

#### Revocation Registry Definition

DID URL: [`did:indy:sovrin:5nDyJVP1NrcPAttP3xwMB9/anoncreds/v0/REV_REG_DEF/56495/npdb/TAG1`](https://indyscan.io/tx/SOVRIN_MAINNET/domain/56497)

- Object Family: `anoncreds`
- Family Version: `v0`
- Schema ID, example `56495`: A unique identifier for the schema upon which the ClaimDef/RevReg are defined. In v0, the value is also the Hyperledger Indy instance sequence number for the Schema object used by this Claim Def.
  In later versions, we expect that the schema identifier will either be removed from the RevReg DID URL, or take a different form.
- Claim Def Name, example `npdb`: The client-defined claim def name.
- Tag, example `TAG1`: The client-defined rev reg tag (name).

Response: Same as the Indy Node [GET_REVOC_REG_DEF Txn](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-revoc-reg-def)

Existing Identifier: `5nDyJVP1NrcPAttP3xwMB9:4:5nDyJVP1NrcPAttP3xwMB9:3:CL:56495:npdb:CL_ACCUM:TAG1`

- `4` is the enumerated object type
- `5nDyJVP1NrcPAttP3xwMB9:3:CL:56495:npdb` is the identifier of the associated Claim Def
- Tag element defined above.

#### Revocation Registry Entry

DID URL: [`did:indy:sovrin:5nDyJVP1NrcPAttP3xwMB9/anoncreds/v0/REV_REG_ENTRY/56495/npdb/TAG1`](https://indyscan.io/tx/SOVRIN_MAINNET/domain/58567)

- Object Family: `anoncreds`
- Family Version: `v0`
- Schema ID, example `56495`: A unique identifier for the schema upon which the ClaimDef/RevReg are defined. In v0, the value is also the Hyperledger Indy instance sequence number for the Schema object used by this Claim Def.
  In later versions, we expect that the schema identifier will either be removed from the RevReg DID URL, or take a different form.
- Claim Def Name, example `npdb`: The client-defined claim def name.
- Tag, example `TAG1`: The client-defined rev reg tag (name).

The DID URL resolution response depends on the query parameters used, as follows:

- None
  - Response is the same as the Indy Node [GET_REVOC_REG Txn](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-revoc-reg) with the current time used for the timestamp.
- `?versionTime=<timestamp>`
  - Response is the same as the Indy Node [GET_REVOC_REG Txn](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-revoc-reg).
- `?from=<timestamp>&to=<timestamp>`
  - Response is the same as the Indy Node [GET_REVOC_REG_DELTA Txn](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-revoc-reg-delta)
  - As noted in the transaction documentation, the `from` parameter is optional.
  - If the `to` parameter is left off the current time is used for the `to` timestamp.

Existing Identifier: `5:5nDyJVP1NrcPAttP3xwMB9:4:5nDyJVP1NrcPAttP3xwMB9:3:CL:56495:npdb:CL_ACCUM:TAG1`

- `5` is the enumerated object type
- The remainder of the identifier is the identifier for the applicable Revocation Registry

#### ATTRIB

DID URL: [`did:indy:sovrin:5nDyJVP1NrcPAttP3xwMB9/ATTRIB/<raw>`](https://indyscan.io/tx/SOVRIN_MAINNET/domain/54743), where `<raw>` is the name of the JSON object that is the value of the `raw` [[ref: ATTRIB]] value. In the example linked at the start of this paragraph, the `<raw>` value would be `endpoint`.

Response: Same as the Indy Node [GET_ATTRIB Txn](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-attrib). Only the `raw` parameter form of the transaction is supported.

Existing Identifier: `F72i3Y3Q4i466efjYJYCHM:1:b6bf...d9e`

- `1` is the enumerated object type
- `b6bf...d9e` is an identifier for the [[ref: ATTRIB]], likely a hash of the content
