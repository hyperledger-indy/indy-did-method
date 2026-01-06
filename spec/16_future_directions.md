## Future Directions

### Multiple Signature NYMs

While not part of this version of the Indy DID Method specification, the group defining the specification recommends that the Indy `NYM` be extended to support multiple signature scenarios, where the [[ref: NYM]] can be updated to include multiple verification keys, and ledger rules defined for authentication of update transactions, such as M signatures of N total signatures are required. Such a change would be reflected in the `verificationMethod` and `authentication` items in the generated DIDDoc.

### Recovery Mechanism

While not part of this version of the Indy DID Method specification, the group defining the specification recommends that the Indy `NYM` be extended to support a recovery mechanism, such as defined in [KERI](https://keri.one). To summarize, a recovery mechanism involves the controller creating not only a verification key pair, but also a recovery public/private key pair during create and update operations. The recovery key is added in some form to (in the case of Indy) the [[ref: NYM]]. With a recovery mechanism in place, if only the active key becomes compromised, only the controller can rotate to the recovery key. Assuming the recovery private key is maintained in an even more secure location than the active private key, recovery should always be possible. Without a recovery mechanism, a compromised active key can be used to rotate the verification key and take control of the [[ref: NYM]] (and DID) from the controller.

### KERI Support

Some consideration was given in designing the initial version of the `did:indy` DID method to including support for supporting KERI identifiers as part of the DID method. At the time of completing the initial version of the spec there was not a clear definition of what "support" would mean, and the concept was moved to this section of the specification.

### Cross Registering Indy Ledgers

Some consideration was given to the idea of allowing network discovery by registering network configuration data (such as Indy genesis files) for a ledger on other ledgers. With such a capability with Indy, connecting to one ledger would allow the  discovery of all other Indy ledgers registered on that ledger. The idea was partially explored in [this document](https://docs.google.com/document/d/1qLCaUiPtFZVNVUkAcLOhkPDPFs-ealTQmmy4HvYYhXQ/edit?usp=sharing), but the design was not completed in time for inclusion.

### Other Signature Schemes

Indy implicitly uses the ED25519 Signature scheme for the `verkey`. We recommend that the [[ref: NYM]]s be evolved to explicitly state the signature scheme so that other signature key schemes can be used on Indy networks.
