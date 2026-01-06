## Indy Ledger Objects: Glossary

Instances of Hyperledger Indy networks persist different kind of (internal) data objects in the ledger. The following section describes those objects.

[[def: DID Document]]

~ A DID document is an entity containing a distributed identifier and properties of the owner of this `did`.

~ A DID Document conform to the [Decentralized Identifiers (DIDs) Core specification](https://https://www.w3.org/TR/did-core/).

[[def: NYM]]

~ A NYM is a legacy Indy entity

~ A NYM (short for "Verinym") is associated with the Legal Identity of an Identity Owner and is a Hyperledger Indy specific term for a data object, which holds DID data of one concrete identity returned during DID resolution. While a NYM can be [read](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-nym) from a Hyperledger Indy Node by any client, a NYM can only be [written](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#nym) to a Hyperledger Indy network as long as the writing entity possess the proper permissions.

~ A NYM object itself does not conform to the [Decentralized Identifiers (DIDs) Core specification](https://https://www.w3.org/TR/did-core/) scheme but rather includes all DID related data of a single identity and therefore its resolution into a DID document does. Therefore writing a NYM to a Hyperledger Indy instance basically results in writing a DID to the ledger. The author of a NYM write transaction is then the owner of the NYM respectively and its embedded DID.

~ When reading NYM transaction from the ledger, clients transform / extract the NYM data into a DID document. Often the terms "NYM" and "DID" are used synonymously, although a "NYM" is "just" Hyperledger Indy's specific way of storing DIDs into the ledger.

[[def: ATTRIB]] - **Deprecated**

~ A Hyperledger Indy ATTRIB (short for "attribute") object extends a specific DID (also known as [[ref: NYM]]) of its owner with further information (attributes) and can be [read](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-attrib) from a Hyperledger Indy Node by any client. An ATTRIB object can only be [written](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#attrib) to a Hyperledger Indy network by an owner of the DID on that network.

~ An ATTRIB is a legacy entity. The use of ATTRIB is **deprecated** with the introduction of the `did:indy` DID Method. The only common use of ATTRIBs in Hyperledger Indy prior to `did:indy` was to define DIDDoc service endpoints for a DID. Since with `did:indy` such a service endpoint can be added directly to the DID (along with any other DIDDoc data) there is no need to continue the use of the older ATTRIB `endpoint` convention. While a Hyperledger Indy client (such as [[ref: Indy VDR]]) MAY continue to try to resolve an `endpoint` ATTRIB when there is no DIDDoc content in a resolved DID, the ongoing practice of using an ATTRIB for that or any other purpose is discouraged.

~ The `did:indy` method introduces legacy support for retrieving the service endpoint of a past version of an attribute by `versionId` or `versionTime`, using a process similar to resolving [DID Versions](\https://hyperledger.github.io/indy-did-method/#did-versions) by these parameters.

[[def: SCHEMA]]

~ A SCHEMA object is a template that defines a set of attribute (names) which are going to be used by issuers for issuance of [Verifiable Credentials](https://www.w3.org/TR/vc-data-model/) within a Hyperledger Indy network. SCHEMAs have a name, version and can be [written](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/transactions.html#schema) to the ledger by any entity with proper permissions. Schemas can be [read](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-schema) from a Hyperledger Indy Node by any client.

~ SCHEMAs define the list of attribute (names) of issued credentials based on a [[ref: CRED_DEF]] (see below).

[[def: CRED_DEF]]

~ A CRED_DEF (short for "credential definition") object contains data required for credential issuance as well as
credential validation and can be [read](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-claim-def) by any Hyperledger Indy client. A CRED_DEF object references a [[ref: SCHEMA]], references a DID of the issuer and can be [written](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#claim-def) by any issuer who intends to issue credentials based on that specific [[ref: SCHEMA]] to the ledger and has the proper permissions in doing so. A public key of the issuer is included within the CRED_DEF which allows validation of the credentials signed by the issuer's private key. When credentials are issued by using the issuers CRED_DEF, the attribute (names) of the [[ref: SCHEMA]] have to be used.

~ Revokable Verifiable Credentials require CRED_DEFs which also reference a [[ref: REV_REG_DEF]] (see below).

[[def: CLAIM_DEF]] - **Deprecated**

~ The deprecated term CLAIM_DEF is sometimes used to describe a CRED_DEF, particularly in existing Hyperledger Indy code.

[[def: REV_REG_DEF]]

~ A REV_REG_DEF object (short for "revocation registry definition") contains information required for verifiers in order to enable them to verify whether a (revokable) verifiable credential has been revoked by the issuer since issuance.

~ REV_REG_DEFs are only needed for revokable verifiable credentials and are most commonly [written](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#claim-def) to the ledger by the owner of a [[ref: CRED_DEF]] immediately after the [[ref: CRED_DEF]] has been written. They can be [read](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-attrib) from a Hyperledger Indy Node by any client and are updated in case of the revocation of a credential, which is based on the used [[ref: CRED_DEF]].

~ Further details about Hyperledger Indy's revocation process can be found [here](https://hyperledger-indy.readthedocs.io/projects/hipe/en/latest/text/0011-cred-revocation/README.html).

[[def: REV_REG_ENTRY]]

~ A REV_REG_ENTRY object (short for "revocation registry entry") marks the current status of one or more revokable verifiable credentials ("revoked" or "not revoked") in the ledger in a privacy preserving manner. A REV_REG_ENTRY is [written](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#revoc-reg-entry) by the owner of a REV_REG_DEF respectively the issuer of the credential(s) based on a [[ref: CRED_DEF]] and its REV_REG_DEF.

~ Any REV_REG_ENTRY condensed with further required information can be [read](https://hyperledger-indy.readthedocs.io/projects/node/en/latest/requests.html#get-revoc-reg-delta) by any Hyperledger Indy client.

~ Further details about Hyperledger Indy's revocation process can be found [here](https://hyperledger-indy.readthedocs.io/projects/hipe/en/latest/text/0011-cred-revocation/README.html).

[[def: Indy VDR]]

~ Hyperledger Indy VDR (for "Verifiable Data Registry") is an open source implementation of an Indy client/resolver for both DIDs and other Indy objects.
The repository is called indy-vdr and can be found [here](https://github.com/hyperledger-indy/indy-vdr).

[[def: Indy Besu VDR]]

~ Hyperledger Indy Besu VDR (for "Verifiable Data Registry") is an open source implementation of an Indy Besu client/resolver for both DIDs and other Indy objects. The repository is called indy-besu contains Indy Besu VDR [here](https://github.com/hyperledger-indy/indy-besu/tree/main/vdr).
