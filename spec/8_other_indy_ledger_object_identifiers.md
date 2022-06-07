## Other Indy Ledger Object Identifiers

Indy ledgers may hold object types other than DIDs, and each of the other object types must also be resolvable to a specific Indy network instance. The identifiers for these objects are used in data structures that are exchanged by Indy clients (e.g. Aries Agents)--verifiable credentials, presentation requests, presentations and so on. Transitioning to the `did:indy` DID Method requires transitioning Indy clients/resolvers to use the identifiers defined in this section.

### DID URLs for Indy Object Identifiers

The structure of identifiers for all non-DID Indy ledger objects is the following DID URL structure, based on the DID of the object's DID controller:

- `<did>/<object-family>/<object-family-version>/<object-type>/<object-type-identifier>`

The components of the DID URL are:

- `<did>` the `did:indy` DID of the object-owning controller
- `<object-family>` family of the object
- `<object-family-version>` version of the object family
- `<object-type>` one of [[ref: SCHEMA]], [[ref: CRED_DEF]], [[ref: REV_REG_DEF]], [[ref: REV_REG_ENTRY]], [[ref: ATTRIB]]
- `<object-type-identifier>` an object type unique identifier defined by Indy by object type.

The data returned from resolving such DID URLs is the ledger object and relevant state proof; the same data returned from the Indy Node read object transactions, such as the [GET_SCHEMA](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-schema) transaction, and dependent on the type of the object.

Since indy allows special characters within the names of the different ledger objects, percent encoding according to [Section 2 of RFC3986](https://datatracker.ietf.org/doc/html/rfc3986#section-2) has to be applied to access these objects via DID URLs.

While there are no restrictions regarding the used characters, we strongly encourage avoiding special characters in the names of ledger objects.

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

#### Cred Def

DID URL: [`did:indy:sovrin:5nDyJVP1NrcPAttP3xwMB9/anoncreds/v0/CLAIM_DEF/56495/npdb`](https://indyscan.io/tx/SOVRIN_MAINNET/domain/56496)

- Object Family: `anoncreds`
- Family Version: `v0`
- Schema ID, example `56495`: A unique identifier for the schema upon which the CredDef is defined. In v0, the value is also the Hyperledger Indy instance sequence number for the Schema object used by this Cred Def.
  In later versions, we expect that the schema identifier will either be removed from the CredDef DID URL, or take a different form.
- Name, example `npdb`: The client-defined cred def name.

Response: Same as the Indy Node [GET_CLAIM_DEF Txn](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-claim-def)

Existing identifier: `5nDyJVP1NrcPAttP3xwMB9:3:CL:56495:npdb`

- `3` is the enumerated object type
- `CL` is the signature type for the cred def, which is `CL` for all cred defs on all existing Indy ledgers
- Schema ID and Name elements defined above.

We recommend that AnonCred credential issuers use a unique Name item per Cred Def, and not rely on the embedded Schema ID
remaining in the DID URL for a Cred Def in future versions of the `did:indy` method.

#### Revocation Registry Definition

DID URL: [`did:indy:sovrin:5nDyJVP1NrcPAttP3xwMB9/anoncreds/v0/REV_REG_DEF/56495/npdb/TAG1`](https://indyscan.io/tx/SOVRIN_MAINNET/domain/56497)

- Object Family: `anoncreds`
- Family Version: `v0`
- Schema ID, example `56495`: A unique identifier for the schema upon which the CredDef/RevReg are defined. In v0, the value is also the Hyperledger Indy instance sequence number for the Schema object used by this Cred Def.
  In later versions, we expect that the schema identifier will either be removed from the RevReg DID URL, or take a different form.
- Cred Def Name, example `npdb`: The client-defined cred def name.
- Tag, example `TAG1`: The client-defined rev reg tag (name).

Response: Same as the Indy Node [GET_REVOC_REG_DEF Txn](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-revoc-reg-def)

Existing Identifier: `5nDyJVP1NrcPAttP3xwMB9:4:5nDyJVP1NrcPAttP3xwMB9:3:CL:56495:npdb:CL_ACCUM:TAG1`

- `4` is the enumerated object type
- `5nDyJVP1NrcPAttP3xwMB9:3:CL:56495:npdb` is the identifier of the associated Cred Def
- Tag element defined above.

#### Revocation Registry Entry

DID URL: [`did:indy:sovrin:5nDyJVP1NrcPAttP3xwMB9/anoncreds/v0/REV_REG_ENTRY/56495/npdb/TAG1`](https://indyscan.io/tx/SOVRIN_MAINNET/domain/58567)

- Object Family: `anoncreds`
- Family Version: `v0`
- Schema ID, example `56495`: A unique identifier for the schema upon which the CredDef/RevReg are defined. In v0, the value is also the Hyperledger Indy instance sequence number for the Schema object used by this Cred Def.
  In later versions, we expect that the schema identifier will either be removed from the RevReg DID URL, or take a different form.
- Cred Def Name, example `npdb`: The client-defined cred def name.
- Tag, example `TAG1`: The client-defined rev reg tag (name).

The DID URL resolution response depends on the query parameters used, as follows:

- None
  - Response is the same as the Indy Node [GET_REVOC_REG Txn](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-revoc-reg) with the current time used for the `versionTime`.
- `?versionTime=<XML Datetime>`
  - Response is the same as the Indy Node [GET_REVOC_REG Txn](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-revoc-reg).
- `?from=<XML Datetime>&to=<XML Datetime>`
  - Response is the same as the Indy Node [GET_REVOC_REG_DELTA Txn](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-revoc-reg-delta)
  - At least one of the parameters has to be set. 
  - If only `from` is set, then `to` is implicitly set to the current time.
  - If only `to` is set, then all deltas up to this time are returned.

Existing Identifier: `5:5nDyJVP1NrcPAttP3xwMB9:4:5nDyJVP1NrcPAttP3xwMB9:3:CL:56495:npdb:CL_ACCUM:TAG1`

- `5` is the enumerated object type
- The remainder of the identifier is the identifier for the applicable Revocation Registry

#### ATTRIB

No DID URL representation is defined for the Hyperledger Indy [[ref: ATTRIB]] object, as
the use of the ATTRIB object is **deprecated** with the introduction of the
`did:indy` DID Method. Where an ATTRIB might have been used in the past, an Indy
client updated for `did:indy` should put the required data directly into the
`diddocContent` item in a DID (NYM) update transaction.
