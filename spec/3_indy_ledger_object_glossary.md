## Indy Ledger Objects: Glossary

Instances of Hyperledger Indy networks persist different kind of (internal) data objects in the ledger. The following section describes those objects.


[[def: NYM]]

~ A NYM (short for "Verinym") is associated with the Legal Identity of an Identity Owner and is a Hyperledger Indy specific term for a data object, which holds DID data of one concrete identity returned during DID resolution. While a NYM can be [read](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-nym) from a Hyperledger Indy Node by any client, a NYM can only be [written](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#nym) to a Hyperledger Indy network as long as the writing entity possess the proper permissions.

~ A NYM object itself does not conform to the [Decentralized Identifiers (DIDs) Core specification](https://https://www.w3.org/TR/did-core/) scheme but rather includes all DID related data of a single identity and therefore its resolution into a DID document does. Therefore writting a NYM to a Hyperledger Indy instance basically results in writting a DID to the ledger. The author of a NYM write transaction is then the owner of the NYM respectively its embedded DID.

~ When reading NYM transaction from the ledger, clients transform / extract the NYM data into a DID document. Often the terms "NYM" and "DID" are used synonymously, although a "NYM" is "just" Hyperledger Indy's specific way of storing DIDs into the ledger.

::: todo finalize NYM glossary entry
finalize
:::

[[def: ATTRIB]]

~ A Hyperledger Indy ATTRIB (short for "attribute") object extends a specific DID (respectively the [[ref: NYM]]) of its owner with further information (attributes) and can be [read](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-attrib) from a Hyperledger Indy Node by any client. An ATTRIB object can only be [written](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#attrib) to a Hyperledger Indy network by an owner of the DID on that network.

::: todo finalize ATTRIB glossary entry
finalize
:::

[[def: SCHEMA]]

~ A SCHEMA object is a template that defines a set of attribute(names) which are going to be used by issuers for issuance of [Verifiable Credentials](https://www.w3.org/TR/vc-data-model/) within a Hyperledger Indy network. SCHEMAs have a name, version and can be [written](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/transactions.html#schema) to the ledger by any entity with proper permissions. Schemas can be [read](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-schema)from a Hyperledger Indy Node by any client.

~ SCHEMAs define the list of attribute(names) of issued credentials based on a [[ref: CLAIM_DEF]] (see below).

::: todo finalize SCHEMA glossary entry
finalize
:::


[[def: CLAIM_DEF]]
~ A CLAIM_DEF (short for "claim definition") object contains data required for credential issuance as well as 
credential validation and can be [read](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-claim-def) by any Hyperledger Indy client. A CLAIM_DEF object references a [[ref: SCHEMA]], references a DID of the issuer and can be [written](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#claim-def) by any issuer who intends to issue credentials based on that specific [[ref: SCHEMA]] to the ledger and has the proper permissions in doing so. A public key of the issuer is included within the CLAIM_DEF which allows validation of the credentials signed by the issuer's private key. When credentials are issued by using the issuers CLAIM_DEF, the attribute(names) of the [[ref: SCHEMA]] have to be used.

~ Revokable Verifiable Credentials require CLAIM_DEFs which also reference a [[ref: REV_REG_DEF]] (see below).

::: todo finalize CLAIM_DEF glossary entry
finalize
:::

[[def: REV_REG_DEF]]

~ A REV_REG_DEF object (short for "revocation registry definition") contains information required for verifiers in order to enable them to verify whether a (revokable) verifiable credential has been revoked by the issuer since issuance.

~ REV_REG_DEFs are only needed for revokable verifiable credentials and are most commonly [written](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#claim-def) to the ledger by the owner of a [[ref: CLAIM_DEF]] immediatly after the [[ref: CLAIM_DEF]] has been written. They can be [read](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-attrib) from a Hyperledger Indy Node by any client and are updated in case of the revocation of a credential, which is based on the used [[ref: CLAIM_DEF]].

~ Further details about Hyperledger Indy's revocation process can be found [here](https://hyperledger-indy.readthedocs.io/projects/hipe/en/latest/text/0011-cred-revocation/README.html).

[[def: REV_REG_ENTRY]]

~ A REV_REG_ENTRY object (short for "revocation registry entry") marks the current status of one or more revokable verifiable credentials ("revoked" or "not revoked") in the ledger in a privacy preserving manner. A REV_REG_ENTRY is [written](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#revoc-reg-entry) by the owner of a REV_REG_DEF respectively the issuer of the credential(s) based on a [[ref: CLAIM_DEF]] and its REV_REG_DEF.

~ Any REV_REG_ENTRY condensed with further required information can be [read](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-revoc-reg-delta) by any Hyperledger Indy client.

~ Further details about Hyperledger Indy's revocation process can be found [here](https://hyperledger-indy.readthedocs.io/projects/hipe/en/latest/text/0011-cred-revocation/README.html).
